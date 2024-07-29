# [L-1] Anyone can front-run the challengeRootL2Block function

An honest `challenger` can detect an incorrect `block number` from a `claim` proposed by a `bad proposer`. 
To do this, the challenger must find the correct `block number`. 
By doing this, they become eligible to receive the `bond` from the `proposer` as an incentive.

However, anyone can simply `front-run` the `challengeRootL2Block` function and receive the `bonds` instead of the `challenger`. 
https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L532
```
function challengeRootL2Block(
    Types.OutputRootProof calldata _outputRootProof,
    bytes calldata _headerRLP
)
    external
{
    if (status != GameStatus.IN_PROGRESS) revert GameNotInProgress();
    if (l2BlockNumberChallenged) revert L2BlockNumberChallenged();
    if (Hashing.hashOutputRootProof(_outputRootProof) != rootClaim().raw()) revert InvalidOutputRootProof();
    if (keccak256(_headerRLP) != _outputRootProof.latestBlockhash) revert InvalidHeaderRLP();
    RLPReader.RLPItem[] memory headerContents = RLPReader.readList(RLPReader.toRLPItem(_headerRLP));
    bytes memory rawBlockNumber = RLPReader.readBytes(headerContents[HEADER_BLOCK_NUMBER_INDEX]);
    if (rawBlockNumber.length > 32) revert InvalidHeaderRLP();
    uint256 blockNumber;
    assembly {
        blockNumber := shr(shl(0x03, sub(0x20, mload(rawBlockNumber))), mload(add(rawBlockNumber, 0x20)))
    }
    if (blockNumber == l2BlockNumber()) revert BlockNumberMatches();
    l2BlockNumberChallenger = msg.sender; // @audit, here
    l2BlockNumberChallenged = true;
}
```
The `front-runner` doesn't need to do any work or take any risks, making it easy for them to profit unfairly.

# [L-2] Anyone can propose a claim for any L2 block number

The `nodes` at `SPLIT_DEPTH` represent individual `blocks`. 
Suppose the `L2 block number` of the current `starting output root` is `L`; then the `game tree` can represent `blocks` with numbers up to `L+(1<<SPLIT_DEPTH)`.

However, there is no relevant check, so anyone can propose a `claim` for any `L2 block number`. 
Suppose a `bad proposer` submits a `claim` for an extremely large `L2 block number`. 
It is unclear how honest `challengers` can `attack` this claim since the `block` for that number has not been built yet. 
Even if both `players` play the game, the final disputed `L2 block number` is wrongly saved to the `preimage oracle`.
https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L453
```
function addLocalData(uint256 _ident, uint256 _execLeafIdx, uint256 _partOffset) external {
    if (_ident == LocalPreimageKey.L1_HEAD_HASH) {
    } else if (_ident == LocalPreimageKey.DISPUTED_L2_BLOCK_NUMBER) {
        uint256 l2Number = startingOutputRoot.l2BlockNumber + disputedPos.traceIndex(SPLIT_DEPTH) + 1;  // @audit, here

        oracle.loadLocalData(_ident, uuid.raw(), bytes32(l2Number << 0xC0), 8, _partOffset);
    } else if (_ident == LocalPreimageKey.CHAIN_ID) {

    } else {
        revert InvalidLocalIdent();
    }
}
```

Therefore, the following check should be added in the `initialize` function.
```
function initialize() public payable virtual {
    if (l2BlockNumber() <= rootBlockNumber) revert UnexpectedRootClaim(rootClaim());

+    if (l2BlockNumber() > rootBlockNumber + (SPLIT_DEPTH << 1)) revert();
}
```

# [L-3] The _numToResolve parameter is used incorrectly

The `resolveClaim` function has a `_numToResolve` parameter, which can be set to `0` to check all child `subgames` in one call. 
However, this only works for the first call because it exists within an `if` block. 
https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L601
```
function resolveClaim(uint256 _claimIndex, uint256 _numToResolve) external {
    if (!checkpoint.initialCheckpointComplete) {
        checkpoint.leftmostPosition = Position.wrap(type(uint128).max);
        checkpoint.initialCheckpointComplete = true;

        if (_numToResolve == 0) _numToResolve = challengeIndicesLen; // @audit, here
    }
}
```
As a result, users must specify the number of remaining `subgames` if they want to complete all `subgames`.

The fix should be as follow:
```
function resolveClaim(uint256 _claimIndex, uint256 _numToResolve) external {
    if (!checkpoint.initialCheckpointComplete) {
        checkpoint.leftmostPosition = Position.wrap(type(uint128).max);
        checkpoint.initialCheckpointComplete = true;

-        if (_numToResolve == 0) _numToResolve = challengeIndicesLen;
    }
+    if (_numToResolve == 0) _numToResolve = challengeIndicesLen;
}
```

# [L-4] The PartOffsetOOB check is incorrect in the _extractPreimagePart function

The `PartOffsetOOB` check in the `_extractPreimagePart` function is as follows.
https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L742
```
function _extractPreimagePart(
    bytes calldata _input,
    uint256 _uuid,
    bool _finalize,
    LPPMetaData _metaData
)
    internal
{
    uint256 offset = _metaData.partOffset();
    uint256 claimedSize = _metaData.claimedSize();
    uint256 currentSize = _metaData.bytesProcessed();

    if (offset < 8 && currentSize == 0) {
        
    } else if (offset >= 8 && (offset = offset - 8) >= currentSize && offset < currentSize + _input.length) {
        uint256 relativeOffset = offset - currentSize;
        if (relativeOffset + 32 >= _input.length && !_finalize) revert PartOffsetOOB(); // @audit, here
    }
}
``` 
However, it should work when `relativeOffset + 32 = _input.length`. 
In this case, the last index of the `proposal part` will be `relativeOffset + 31 = _input.length - 1` and it's in bound obviously.

# [L-5] The challengeLPP function can be front-run

Anyone can `front-run` the `challengeLPP` function on behalf of an honest `challenger`, allowing them to receive the `bonds` from the `proposer` instead.
https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L605
```
function challengeLPP(
    address _claimant,
    uint256 _uuid,
    LibKeccak.StateMatrix memory _stateMatrix,
    Leaf calldata _preState,
    bytes32[] calldata _preStateProof,
    Leaf calldata _postState,
    bytes32[] calldata _postStateProof
)
    external
{
    
    // Pay out the bond to the challenger.
    _payoutBond(_claimant, _uuid, msg.sender); // @audit, here
}
```
In order to `challenge` an incorrect `commitment`, `honest challengers` need to detect it. 
However, if they know their transactions could be `front-run` by others, causing them to receive nothing, they would have no `incentive` to do so.

# [L-6] The memory assignment in the `findLatestGames` function is incorrect

In the `findLatestGames` function, we allocate `32 * n bytes` for the `GameSearchResult` array. 
https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L164
```
function findLatestGames(
    GameType _gameType,
    uint256 _start,
    uint256 _n
)
    external
    view
    returns (GameSearchResult[] memory games_)
{
    if (_start >= _disputeGameList.length || _n == 0) return games_;

    // Allocate enough memory for the full array, but start the array's length at `0`. We may not use all of the
    // memory allocated, but we don't know ahead of time the final size of the array.
    assembly {
        games_ := mload(0x40)
        mstore(0x40, add(games_, add(0x20, shl(0x05, _n)))) // @audit, here
    }
}
```
However, the size of the `GameSearchResult struct` is larger than `32 bytes`.

# [L-7] There is no need to challenge proposals that have already been challenged

To receive `bonds` from incorrect `proposals`, `challengers` can `challenge` those `proposals`. 
Once the `bond` is sent to one `challenger`, there are no remaining funds for that `proposal`. 
https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L791
```
function _payoutBond(address _claimant, uint256 _uuid, address _to) internal {
    uint256 bond = proposalBonds[_claimant][_uuid];
    proposalBonds[_claimant][_uuid] = 0; // @audit, here
    (bool success,) = _to.call{ value: bond }("");
    if (!success) revert BondTransferFailed();
}
```
However, other `challengers` can still attempt to `challenge` the `proposal` using the `challengeLPP` and `challengeFirstLPP` functions, despite there being no `funds` left.

Please add a check in these two functions to ensure that a `proposal` cannot be `challenged` again if it has already been `challenged`.