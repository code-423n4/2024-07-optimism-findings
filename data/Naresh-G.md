Note:
This report includes additional instances of an issue listed in the automated findings report.

## Summary

| |Issue|Instances| Gas Savings
|-|:-|:-:|:-:|
| [[G-01](#g-01)] | `abi.encode()` is less efficient than `abi.encodepacked()` for non-address arguments | 9| 0|
| [[G-02](#g-02)] | Argument validation should be at the top of the function/block | 59| 0|
| [[G-03](#g-03)] | Use `Array.unsafeAccess()` to avoid repeated array length checks | 2| 4200|
| [[G-04](#g-04)] | Avoid fetching a low-level call's return data by using assembly | 2| 318|
| [[G-05](#g-05)] | Cache array length outside of loop | 2| 194|
| [[G-06](#g-06)] | Use `calldata` instead of `memory` for immutable arguments | 4| 2400|
| [[G-07](#g-07)] | Constructors can be marked as `payable` to save deployment gas | 4| 84|
| [[G-08](#g-08)] | Do not calculate constants | 1| 0|
| [[G-09](#g-09)] | `do-while` is cheaper than `for`-loops when the initial check can be skipped | 2| 0|
| [[G-10](#g-10)] | Duplicated `require()`/`revert()` checks should be refactored to a modifier or function to save deployment gas | 5| 0|
| [[G-11](#g-11)] | Counting down in `for` statements is more gas efficient | 3| 24|
| [[G-12](#g-12)] | Function names can be optimized | 6| 768|
| [[G-13](#g-13)] | Don't initialize variables with default value | 8| 0|
| [[G-14](#g-14)] | Optimize Deployment Size by Fine-tuning IPFS Hash | 6| 9084|
| [[G-15](#g-15)] | `require()`/`revert()` strings longer than 32 bytes cost extra gas | 1| 18|
| [[G-16](#g-16)] | use assembly for Low-Level Calls' Return Data | 1| 159|
| [[G-17](#g-17)] | Multiple accesses of the same mapping/array key/index should be cached | 13| 546|
| [[G-18](#g-18)] | Multiple mappings can be replaced with a single struct mapping | 11| 462|
| [[G-19](#g-19)] | State variables should be cached in stack variables rather than re-reading them from storage | 13| 1261|
| [[G-20](#g-20)] | Use Only Named Returns | 1| 44|
| [[G-21](#g-21)] | Nesting `if`-statements is cheaper than using `&&` | 17| 68|
| [[G-22](#g-22)] | Consider using OpenZeppelin's `EnumerateSet` instead of nested mappings | 7| 7000|
| [[G-23](#g-23)] | Operator `>=`/`<=` costs less gas than operator `>`/`<` | 31| 93|
| [[G-24](#g-24)] | Using `private` for constants saves gas | 19| 64714|
| [[G-25](#g-25)] | Use assembly scratch space for building calldata for external calls | 41| 9020|
| [[G-26](#g-26)] | Usage of smaller `uint`/`int` types causes overhead | 78| 4290|
| [[G-27](#g-27)] | Consider using solady's 'FixedPointMathLib' | 25| 0|
| [[G-28](#g-28)] | Newer versions of solidity are more gas efficient | 6| 0|
| [[G-29](#g-29)] | Stack variable cost less than state variables while used in emiting event | 1| 9|
| [[G-30](#g-30)] | Mappings are cheaper to use than storage arrays | 4| 8400|
| [[G-31](#g-31)] | Gas savings can be achieved by changing the model for assigning value to the structure | 2| 260|
| [[G-32](#g-32)] | Optimize Storage with Byte Truncation for Time Related State Variables | 7| 14000|
| [[G-33](#g-33)] | Trade-offs Between Modifiers and Internal Functions | 36| 378000|
| [[G-34](#g-34)] | Use `uint256(1)`/`uint256(2)` instead of `true`/`false` to save gas for changes | 5| 85500|
| [[G-35](#g-35)] | Divisions can be `unchecked` to save gas | 1| 20|
| [[G-36](#g-36)] | Increments can be `unchecked` to save gas | 3| 90|
| [[G-37](#g-37)] | Avoid unnecessary public variables | 32| 704000|
| [[G-38](#g-38)] | Use assembly to `emit` events | 5| 190|
| [[G-39](#g-39)] | Use assembly to validate `msg.sender` | 2| 24|
| [[G-40](#g-40)] | Simple checks for zero can be done using assembly to save gas | 45| 45|
| [[G-41](#g-41)] | Use `byte32` in place of `string` | 4| 0|
| [[G-42](#g-42)] | Using delete instead of setting mapping to '0' saves gas | 2| 10|
| [[G-43](#g-43)] | `internal` functions not called by the contract should be removed to save deployment gas | 14| 0|
| [[G-44](#g-44)] | Use `x = x + y` instead of `x += y` for state variables | 1| 10|
| [[G-45](#g-45)] | Use solady library where possible to save gas | 1| 1000|
| [[G-46](#g-46)] | Variable declared within iteration | 7| 0|
| [[G-47](#g-47)] | Enable IR-based code generation | 6| 0|
| [[G-48](#g-48)] | WETH address definition can be use directly | 2| 0|
| [[G-49](#g-49)] | Using XOR (^) and AND (&) bitwise equivalents for gas optimizations | 136| 0|

Total: 637 instances over 49 issues with an estimate of  1,305,543 gas saved.

### Gas Risk Issues

### [G-01]<a name="g-01"></a> `abi.encode()` is less efficient than `abi.encodepacked()` for non-address arguments



*There are 9 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

589:         if (keccak256(abi.encode(_stateMatrix)) != _preState.stateCommitment) revert InvalidPreimage();

599:         if (keccak256(abi.encode(_stateMatrix)) == _postState.stateCommitment) revert PostStateMatches();

630:         if (keccak256(abi.encode(stateMatrix)) == _postState.stateCommitment) revert PostStateMatches();

669:         if (keccak256(abi.encode(_stateMatrix)) != _preState.stateCommitment) revert InvalidPreimage();

700:                 treeRoot_ = keccak256(abi.encode(proposalBranches[_owner][_uuid][height], treeRoot_));

702:                 treeRoot_ = keccak256(abi.encode(treeRoot_, zeroHashes[height]));

```


*GitHub* : [589](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L589-L589), [599](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L599-L599), [630](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L630-L630), [669](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L669-L669), [700](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L700-L700), [702](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L702-L702)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol

144:         uuid_ = Hash.wrap(keccak256(abi.encode(_gameType, _rootClaim, _extraData)));

```


*GitHub* : [144](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L144-L144)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

1020:             ? Hash.wrap(keccak256(abi.encode(_disputed, _disputedPos)))

1021:             : Hash.wrap(keccak256(abi.encode(_starting, _startingPos, _disputed, _disputedPos)));

```


*GitHub* : [1020](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L1020-L1020), [1021](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L1021-L1021)

### [G-02]<a name="g-02"></a> Argument validation should be at the top of the function/block

Checks that involve function arguments should come before checks that involve state variables, function calls, and calculations. By doing these checks first, the function is able to revert before wasting a Gcoldsload (2100 gas*) in a function that may ultimately revert in the unhappy case.

*There are 59 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/MIPS.sol

334:             if (_opcode == 4 || _opcode == 5) {

339:             else if (_opcode == 6) {

343:             else if (_opcode == 7) {

347:             else if (_opcode == 1) {

394:             if (_func == 0x10) {

398:             else if (_func == 0x11) {

402:             else if (_func == 0x12) {

406:             else if (_func == 0x13) {

410:             else if (_func == 0x18) {

416:             else if (_func == 0x19) {

424:             else if (_func == 0x1a) {

425:                 if (int32(_rt) == 0) {

434:             else if (_func == 0x1b) {

435:                 if (_rt == 0) {

443:             if (_storeReg != 0) {

478:             if (_linkReg != 0) {

501:             require(_storeReg < 32, "valid register");

504:             if (_storeReg != 0 && _conditional) {

```


*GitHub* : [334](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L334-L334), [339](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L339-L339), [343](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L343-L343), [347](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L347-L347), [394](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L394-L394), [398](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L398-L398), [402](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L402-L402), [406](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L406-L406), [410](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L410-L410), [416](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L416-L416), [424](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L424-L424), [425](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L425-L425), [434](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L434-L434), [435](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L435-L435), [443](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L443-L443), [478](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L478-L478), [501](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L501-L501), [504](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L504-L504)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

111:         if (_offset + 32 >= length + 8) {

134:         if (_partOffset > _size + 8 || _size > 32) {

425:         if (_partOffset >= _claimedSize + 8) revert PartOffsetOOB();

428:         if (_claimedSize < MIN_LPP_SIZE_BYTES) revert InvalidInputSize();

451:         if (_finalize) {

475:         if (blocksProcessed != _inputStartBlock) revert WrongStartingBlock();

543:         if (_finalize) {

582:             !(
583:                 _verify(_preStateProof, root, _preState.index, _hashLeaf(_preState))
584:                     && _verify(_postStateProof, root, _postState.index, _hashLeaf(_postState))
585:             )

589:         if (keccak256(abi.encode(_stateMatrix)) != _preState.stateCommitment) revert InvalidPreimage();

592:         if (_preState.index + 1 != _postState.index) revert StatesNotContiguous();

599:         if (keccak256(abi.encode(_stateMatrix)) == _postState.stateCommitment) revert PostStateMatches();

619:         if (!_verify(_postStateProof, root, _postState.index, _hashLeaf(_postState))) revert InvalidProof();

622:         if (_postState.index != 0) revert StatesNotContiguous();

630:         if (keccak256(abi.encode(stateMatrix)) == _postState.stateCommitment) revert PostStateMatches();

662:             !(
663:                 _verify(_preStateProof, root, _preState.index, _hashLeaf(_preState))
664:                     && _verify(_postStateProof, root, _postState.index, _hashLeaf(_postState))
665:             )

669:         if (keccak256(abi.encode(_stateMatrix)) != _preState.stateCommitment) revert InvalidPreimage();

672:         if (_preState.index + 1 != _postState.index || _postState.index != metaData.blocksProcessed() - 1) {

735:         } else if (offset >= 8 && (offset = offset - 8) >= currentSize && offset < currentSize + _input.length) {

742:             if (relativeOffset + 32 >= _input.length && !_finalize) revert PartOffsetOOB();

```


*GitHub* : [111](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L111-L111), [134](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L134-L134), [425](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L425-L425), [428](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L428-L428), [451](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L451-L451), [475](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L475-L475), [543](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L543-L543), [582](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L582-L585), [589](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L589-L589), [592](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L592-L592), [599](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L599-L599), [619](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L619-L619), [622](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L622-L622), [630](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L630-L630), [662](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L662-L665), [669](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L669-L669), [672](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L672-L672), [735](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L735-L735), [742](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L742-L742)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol

100:         if (msg.value != initBonds[_gameType]) revert IncorrectBondAmount();

172:             if (gameType.raw() == _gameType.raw()) {

189:                 if (games_.length >= _n) break;

```


*GitHub* : [100](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L100-L100), [172](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L172-L172), [189](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L189-L189)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

140:         if (_splitDepth >= _maxGameDepth) revert InvalidSplitDepth();

142:         if (_clockExtension.raw() > _maxClockDuration.raw()) revert InvalidClockExtension();

260:         if (_isAttack) {

285:         if (keccak256(_stateData) << 8 != preStateClaim.raw() << 8) revert InvalidPrestate();

327:         if (Claim.unwrap(parent.claim) != Claim.unwrap(_disputed)) revert InvalidDisputedClaimIndex();

339:         if ((_challengeIndex == 0 || nextPositionDepth == SPLIT_DEPTH + 2) && !_isAttack) {

345:         if (l2BlockNumberChallenged && _challengeIndex == 0) revert L2BlockNumberChallenged();

438:         if (_ident == LocalPreimageKey.L1_HEAD_HASH) {

441:         } else if (_ident == LocalPreimageKey.STARTING_OUTPUT_ROOT) {

444:         } else if (_ident == LocalPreimageKey.DISPUTED_OUTPUT_ROOT) {

447:         } else if (_ident == LocalPreimageKey.DISPUTED_L2_BLOCK_NUMBER) {

456:         } else if (_ident == LocalPreimageKey.CHAIN_ID) {

505:         if (Hashing.hashOutputRootProof(_outputRootProof) != rootClaim().raw()) revert InvalidOutputRootProof();

508:         if (keccak256(_headerRLP) != _outputRootProof.latestBlockhash) revert InvalidHeaderRLP();

573:         if (resolvedSubgames[_claimIndex]) revert ClaimAlreadyResolved();

580:         if (challengeIndicesLen == 0 && _claimIndex != 0) {

601:             if (_numToResolve == 0) _numToResolve = challengeIndicesLen;

643:             if (_claimIndex == 0 && l2BlockNumberChallenged) {

883:         if (_isAttack || disputed.position.depth() % 2 == SPLIT_DEPTH % 2) {

```


*GitHub* : [140](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L140-L140), [142](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L142-L142), [260](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L260-L260), [285](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L285-L285), [327](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L327-L327), [339](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L339-L339), [345](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L345-L345), [438](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L438-L438), [441](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L441-L441), [444](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L444-L444), [447](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L447-L447), [456](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L456-L456), [505](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L505-L505), [508](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L508-L508), [573](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L573-L573), [580](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L580-L580), [601](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L601-L601), [643](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L643-L643), [883](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L883-L883)

### [G-03]<a name="g-03"></a> Use `Array.unsafeAccess()` to avoid repeated array length checks

When using storage arrays, solidity adds an internal lookup of the array's length (a Gcoldsload **2100 gas**) to ensure you don't read past the array's end. You can avoid this lookup by using [Array.unsafeAccess()](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/94697be8a3f0dfcd95dfb13ffbd39b5973f5c65d/contracts/utils/Arrays.sol#L57) in cases where the length has already been checked, as is the case with the instances below

*There are 2 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol

182:                 games_[games_.length - 1] = GameSearchResult({

189:                 if (games_.length >= _n) break;

```


*GitHub* : [182](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L182-L182), [189](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L189-L189)

### [G-04]<a name="g-04"></a> Avoid fetching a low-level call's return data by using assembly

Even if you don't assign the call's second return value, it still gets copied to memory. Use assembly instead to prevent this and save **159** [gas](https://gist.github.com/IllIllI000/0e18a40f3afb0b83f9a347b10ee89ad2):
`(bool success,) = payable(receiver).call{gas: gas, value: value}(''); -> bool success; assembly { success := call(gas, receiver, value, 0, 0, 0, 0) }`

*There are 2 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

792:         (bool success,) = _to.call{ value: bond }("");

```


*GitHub* : [792](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L792-L792)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

759:         (bool success,) = _recipient.call{ value: recipientCredit }(hex"");

```


*GitHub* : [759](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L759-L759)

### [G-05]<a name="g-05"></a> Cache array length outside of loop

If not cached, the solidity compiler will always read the length of the array during each iteration. That is, if it is a storage array, this is an extra sload operation (100 additional extra gas for each iteration except for the first) and if it is a memory array, this is an extra mload operation (3 additional gas for each iteration except for the first).

*There are 2 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol

182:                 games_[games_.length - 1] = GameSearchResult({

189:                 if (games_.length >= _n) break;

```


*GitHub* : [182](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L182-L182), [189](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L189-L189)

### [G-06]<a name="g-06"></a> Use `calldata` instead of `memory` for immutable arguments

Mark data types as `calldata` instead of `memory` where possible. This makes it so that the data is not automatically loaded into memory. If the data passed into the function does not need to be changed (like updating values in an array), it can be passed in as `calldata`. The one exception to this is if the argument must later be passed into another function that takes an argument that specifies `memory` storage.

*There are 4 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageKeyLib.sol

// @audit  _preimage
47:     function keccak256PreimageKey(bytes memory _preimage) internal pure returns (bytes32 key_) {

```


*GitHub* : [47](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageKeyLib.sol#L47-L47)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

// @audit  _stateMatrix
568:     function challengeLPP(
569:         address _claimant,
570:         uint256 _uuid,
571:         LibKeccak.StateMatrix memory _stateMatrix,
572:         Leaf calldata _preState,
573:         bytes32[] calldata _preStateProof,
574:         Leaf calldata _postState,
575:         bytes32[] calldata _postStateProof
576:     )
577:         external
578:     {

// @audit  _stateMatrix
640:     function squeezeLPP(
641:         address _claimant,
642:         uint256 _uuid,
643:         LibKeccak.StateMatrix memory _stateMatrix,
644:         Leaf calldata _preState,
645:         bytes32[] calldata _preStateProof,
646:         Leaf calldata _postState,
647:         bytes32[] calldata _postStateProof
648:     )
649:         external
650:     {

// @audit  _leaf
797:     function _hashLeaf(Leaf memory _leaf) internal pure returns (bytes32 leaf_) {

```


*GitHub* : [568](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L568-L578), [640](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L640-L650), [797](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L797-L797)

### [G-07]<a name="g-07"></a> Constructors can be marked as `payable` to save deployment gas

`payable` functions cost less gas to execute, because the compiler does not have to add extra checks to ensure that no payment is provided. A constructor can be safely marked as payable, because only the deployer would be able to pass funds, and the project itself would not pass any funds.

*There are 4 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/MIPS.sol

64:     constructor(IPreimageOracle _oracle) {

```


*GitHub* : [64](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L64-L64)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

89:     constructor(uint256 _minProposalSize, uint256 _challengePeriod) {

```


*GitHub* : [89](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L89-L89)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol

42:     constructor() OwnableUpgradeable() {

```


*GitHub* : [42](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L42-L42)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

125:     constructor(
126:         GameType _gameType,
127:         Claim _absolutePrestate,
128:         uint256 _maxGameDepth,
129:         uint256 _splitDepth,
130:         Duration _clockExtension,
131:         Duration _maxClockDuration,
132:         IBigStepper _vm,
133:         IDelayedWETH _weth,
134:         IAnchorStateRegistry _anchorStateRegistry,
135:         uint256 _l2ChainId
136:     ) {

```


*GitHub* : [125](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L125-L136)

### [G-08]<a name="g-08"></a> Do not calculate constants

Due to how constant variables are implemented (replacements at compile-time), an expression assigned to a constant variable is recomputed each time that the variable is used, which wastes some gas.

*There are 1 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

29:     uint256 public constant MAX_LEAF_COUNT = 2 ** KECCAK_TREE_DEPTH - 1;

```


*GitHub* : [29](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L29-L29)

### [G-09]<a name="g-09"></a> `do-while` is cheaper than `for`-loops when the initial check can be skipped

`for (uint256 i; i < len; ++i){ ... }` -> `do { ...; ++i } while (i < len);`

*There are 2 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

94:         for (uint256 height = 0; height < KECCAK_TREE_DEPTH - 1; height++) {

698:         for (uint256 height = 0; height < KECCAK_TREE_DEPTH; height++) {

```


*GitHub* : [94](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L94-L94), [698](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L698-L698)

### [G-10]<a name="g-10"></a> Duplicated `require()`/`revert()` checks should be refactored to a modifier or function to save deployment gas

This will cost more runtime gas, but will reduce deployment gas when the function is (optionally called via a modifier) called more than once as is the case for the examples below. Most projects do not make this trade-off, but it's available nonetheless.

*There are 5 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/MIPS.sol

426:                     revert("MIPS: division by zero");

436:                     revert("MIPS: division by zero");

959:                     revert("invalid instruction");

1054:                     revert("invalid instruction");

1057:             revert("invalid instruction");

```


*GitHub* : [426](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L426-L426), [436](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L436-L436), [959](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L959-L959), [1054](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1054-L1054), [1057](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1057-L1057)

### [G-11]<a name="g-11"></a> Counting down in `for` statements is more gas efficient

Counting down is more gas efficient than counting up because neither we are making zero variable to non-zero variable and also we will get gas refund in the last transaction when making non-zero to zero variable. [More info](https://solodit.xyz/issues/g-02-counting-down-in-for-statements-is-more-gas-efficient-code4rena-pooltogether-pooltogether-git)

*There are 3 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

94:         for (uint256 height = 0; height < KECCAK_TREE_DEPTH - 1; height++) {

698:         for (uint256 height = 0; height < KECCAK_TREE_DEPTH; height++) {

```


*GitHub* : [94](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L94-L94), [698](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L698-L698)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

607:         for (uint256 i = checkpoint.subgameIndex; i < finalCursor; i++) {

```


*GitHub* : [607](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L607-L607)

### [G-12]<a name="g-12"></a> Function names can be optimized

Function that are `public`/`external` and public state variable names can be optimized to save gas.

Method IDs that have two leading zero bytes can save **128 gas** each during deployment, and renaming functions to have lower method IDs will save **22 gas** per call, per sorted position shifted. [Reference](https://blog.emn178.cc/en/post/solidity-gas-optimization-function-name/)

*There are 6 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/MIPS.sol

23: contract MIPS is ISemver {

```


*GitHub* : [23](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L23-L23)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageKeyLib.sol

6: library PreimageKeyLib {

```


*GitHub* : [6](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageKeyLib.sol#L6-L6)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

15: contract PreimageOracle is IPreimageOracle, ISemver {

```


*GitHub* : [15](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L15-L15)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol

20: library LPPMetadataLib {

```


*GitHub* : [20](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L20-L20)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol

19: contract DisputeGameFactory is OwnableUpgradeable, IDisputeGameFactory, ISemver {

```


*GitHub* : [19](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L19-L19)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

25: contract FaultDisputeGame is IFaultDisputeGame, Clone, ISemver {

```


*GitHub* : [25](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L25-L25)

### [G-13]<a name="g-13"></a> Don't initialize variables with default value



*There are 8 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/MIPS.sol

49:     uint32 internal constant FD_STDIN = 0;

161:             uint32 v0 = 0;

162:             uint32 v1 = 0;

327:             bool shouldBranch = false;

525:             uint256 s = 0;

974:                         uint32 i = 0;

```


*GitHub* : [49](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L49-L49), [161](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L161-L161), [162](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L162-L162), [327](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L327-L327), [525](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L525-L525), [974](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L974-L974)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

94:         for (uint256 height = 0; height < KECCAK_TREE_DEPTH - 1; height++) {

698:         for (uint256 height = 0; height < KECCAK_TREE_DEPTH; height++) {

```


*GitHub* : [94](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L94-L94), [698](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L698-L698)

### [G-14]<a name="g-14"></a> Optimize Deployment Size by Fine-tuning IPFS Hash

The Solidity compiler appends 53 bytes of metadata to the smart contract code, incurring an extra cost of 10,600 gas. This additional expense arises from 200 gas per bytecode, plus calldata cost, which amounts to 16 gas for non-zero bytes and 4 gas for zero bytes. This results in a maximum of 848 extra gas in calldata cost.

Reducing this cost is crucial for the following reasons:

The metadata's 53-byte addition leads to a deployment cost increase of 10,600 gas. It can also result in an additional calldata cost of up to 848 gas. Ways to Minimize Gas Consumption:

Employ the `--no-cbor-metadata` compiler option to exclude metadata. Be cautious as this might impact contract verification. Search for code comments that yield an IPFS hash with more zeros, thereby reducing calldata costs.

*There are 6 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/MIPS.sol

1: // SPDX-License-Identifier: MIT

```


*GitHub* : [1](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1-L1)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageKeyLib.sol

1: // SPDX-License-Identifier: MIT

```


*GitHub* : [1](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageKeyLib.sol#L1-L1)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

1: // SPDX-License-Identifier: MIT

```


*GitHub* : [1](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L1-L1)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol

1: // SPDX-License-Identifier: MIT

```


*GitHub* : [1](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L1-L1)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol

1: // SPDX-License-Identifier: MIT

```


*GitHub* : [1](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L1-L1)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

1: // SPDX-License-Identifier: MIT

```


*GitHub* : [1](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L1-L1)

### [G-15]<a name="g-15"></a> `require()`/`revert()` strings longer than 32 bytes cost extra gas

Each extra memory word of bytes past the original 32 [incurs an MSTORE](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#consider-having-short-revert-strings) which costs 3 gas

*There are 1 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/MIPS.sol

529:             require(s >= (offset_ + 28 * 32), "check that there is enough calldata");

```


*GitHub* : [529](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L529-L529)

### [G-16]<a name="g-16"></a> use assembly for Low-Level Calls' Return Data

Using assembly for low-level calls in Solidity can provide gas savings, especially when dealing with return data. High-level Solidity calls include overhead for decoding return data, which can be bypassed with assembly. By directly accessing return data in assembly, you can eliminate unnecessary memory allocation and data copying, leading to a more gas-efficient execution. However, this approach requires a deep understanding of the Ethereum Virtual Machine (EVM) and is prone to errors. It’s crucial to ensure security and correctness in the implementation. This technique is best suited for advanced users aiming to optimize contract performance in specific, gas-critical scenarios.

*There are 1 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

759:         (bool success,) = _recipient.call{ value: recipientCredit }(hex"");

```


*GitHub* : [759](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L759-L759)

### [G-17]<a name="g-17"></a> Multiple accesses of the same mapping/array key/index should be cached

Access of a value inside a mapping/array, within a function. Caching a mapping's value in a local `storage` or `calldata` variable when the value is accessed [multiple times](https://gist.github.com/IllIllI000/ec23a57daa30a8f8ca8b9681c8ccefb0), saves **~42 gas per access** due to not having to recalculate the key's `keccak256` hash (Gkeccak256 - **30 gas**) and that calculation's associated stack operations. Caching an array's struct avoids recalculating the array offsets into `memory`/`calldata`

*There are 13 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

// @audit 'zeroHashes[height]' also accessed on same line
95:             zeroHashes[height + 1] = keccak256(abi.encodePacked(zeroHashes[height], zeroHashes[height]));

// @audit 'proposalMetadata[msg.sender][_uuid]' also accessed on line 431 
432:         proposalMetadata[msg.sender][_uuid] = metaData.setPartOffset(_partOffset).setClaimedSize(_claimedSize);

// @audit 'proposalBranches[msg.sender][_uuid]' also accessed on line 458 
551:         proposalBranches[msg.sender][_uuid] = branch;

// @audit 'proposalMetadata[msg.sender][_uuid]' also accessed on line 459 
556:         proposalMetadata[msg.sender][_uuid] = metaData;

// @audit 'proposalMetadata[_claimant][_uuid]' also accessed on same line
602:         proposalMetadata[_claimant][_uuid] = proposalMetadata[_claimant][_uuid].setCountered(true);

// @audit 'proposalMetadata[_claimant][_uuid]' also accessed on same line
633:         proposalMetadata[_claimant][_uuid] = proposalMetadata[_claimant][_uuid].setCountered(true);

// @audit 'proposalParts[msg.sender][_uuid]' also accessed on line 734 
750:             proposalParts[msg.sender][_uuid] = preimagePart;

// @audit 'proposalBonds[_claimant][_uuid]' also accessed on line 790
791:         proposalBonds[_claimant][_uuid] = 0;

```


*GitHub* : [95](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L95-L95), [432](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L432-L432), [551](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L551-L551), [556](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L556-L556), [602](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L602-L602), [633](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L633-L633), [750](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L750-L750), [791](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L791-L791)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol

// @audit '_disputeGames[uuid]' also accessed on line 123
129:         _disputeGames[uuid] = id;

```


*GitHub* : [129](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L129-L129)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

// @audit 'claims[claimHash]' also accessed on line 391
392:         claims[claimHash] = true;

// @audit 'resolvedSubgames[_claimIndex]' also accessed on line 573, 640
588:             resolvedSubgames[_claimIndex] = true;

// @audit 'resolutionCheckpoints[_claimIndex]' also accessed on line 593
632:         resolutionCheckpoints[_claimIndex] = checkpoint;

// @audit 'credit[_recipient]' also accessed on line 749
750:         credit[_recipient] = 0;

```


*GitHub* : [392](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L392-L392), [588](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L588-L588), [632](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L632-L632), [750](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L750-L750)

### [G-18]<a name="g-18"></a> Multiple mappings can be replaced with a single struct mapping

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to [not having to recalculate the key's keccak256 hash](https://gist.github.com/IllIllI000/ec23a57daa30a8f8ca8b9681c8ccefb0) (Gkeccak256 - 30 gas) and that calculation's associated stack operations.

*There are 11 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

40:     mapping(bytes32 => uint256) public preimageLengths;

42:     mapping(bytes32 => mapping(uint256 => bytes32)) public preimageParts;

44:     mapping(bytes32 => mapping(uint256 => bool)) public preimagePartOk;

74:     mapping(address => mapping(uint256 => bytes32[KECCAK_TREE_DEPTH])) public proposalBranches;

77:     mapping(address => mapping(uint256 => LPPMetaData)) public proposalMetadata;

79:     mapping(address => mapping(uint256 => uint256)) public proposalBonds;

81:     mapping(address => mapping(uint256 => bytes32)) public proposalParts;

83:     mapping(address => mapping(uint256 => uint64[])) public proposalBlocks;

```


*GitHub* : [40](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L40-L40), [42](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L42-L42), [44](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L44-L44), [74](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L74-L74), [77](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L77-L77), [79](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L79-L79), [81](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L81-L81), [83](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L83-L83)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

104:     mapping(uint256 => uint256[]) public subgames;

107:     mapping(uint256 => bool) public resolvedSubgames;

110:     mapping(uint256 => ResolutionCheckpoint) public resolutionCheckpoints;

```


*GitHub* : [104](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L104-L104), [107](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L107-L107), [110](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L110-L110)

### [G-19]<a name="g-19"></a> State variables should be cached in stack variables rather than re-reading them from storage

The instances below point to the **second+** access of a state variable within a function. Caching of a state variable replaces each Gwarmaccess (**100 gas**) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses.

*There are 13 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/MIPS.sol

// @audit 'EBADF' also accessed on line 244, 293
277:                     v1 = EBADF;

// @audit 'FD_STDIN' also accessed on line 201
287:                     if (a0 == FD_STDIN || a0 == FD_PREIMAGE_READ || a0 == FD_HINT_READ) {

// @audit 'FD_STDOUT' also accessed on line 251
289:                     } else if (a0 == FD_STDOUT || a0 == FD_STDERR || a0 == FD_PREIMAGE_WRITE || a0 == FD_HINT_WRITE) {

```


*GitHub* : [277](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L277-L277), [287](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L287-L287), [289](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L289-L289)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

// @audit 'initialized' also accessed on line 170
220:         initialized = true;

// @audit 'MAX_GAME_DEPTH' also accessed on line 255
269:             preStateClaim = (stepPos.indexAtDepth() % (1 << (MAX_GAME_DEPTH - SPLIT_DEPTH))) == 0

// @audit 'SPLIT_DEPTH' also accessed on line 339, 380
355:         if (nextPositionDepth == SPLIT_DEPTH + 1) {

// @audit 'MAX_CLOCK_DURATION' also accessed on line 369, 381
377:         if (nextDuration.raw() > MAX_CLOCK_DURATION.raw() - CLOCK_EXTENSION.raw()) {

// @audit 'CLOCK_EXTENSION' also accessed on same line
380:                 nextPositionDepth == SPLIT_DEPTH - 1 ? CLOCK_EXTENSION.raw() * 2 : CLOCK_EXTENSION.raw();

// @audit 'l2BlockNumberChallenged' also accessed on line 502
533:         l2BlockNumberChallenged = true;

// @audit 'status' also accessed on line 543
553:         emit Resolved(status = status_);

// @audit 'MAX_GAME_DEPTH' also accessed on line 704
723:         uint256 c = MAX_GAME_DEPTH * FixedPointMathLib.WAD;

// @audit 'MAX_CLOCK_DURATION' also accessed on same line
785:         duration_ = challengeDuration > MAX_CLOCK_DURATION.raw() ? MAX_CLOCK_DURATION : Duration.wrap(challengeDuration);

// @audit 'SPLIT_DEPTH' also accessed on line 941, 957
951:         while ((currentDepth = claim.position.depth()) > SPLIT_DEPTH) {

```


*GitHub* : [220](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L220-L220), [269](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L269-L269), [355](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L355-L355), [377](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L377-L377), [380](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L380-L380), [533](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L533-L533), [553](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L553-L553), [723](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L723-L723), [785](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L785-L785), [951](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L951-L951)

### [G-20]<a name="g-20"></a> Use Only Named Returns

The Solidity compiler can generate more efficient bytecode when using named returns. It's recommended to replace anonymous returns with named returns for potential gas savings.

*There are 1 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/MIPS.sol

640:     function step(bytes calldata _stateData, bytes calldata _proof, bytes32 _localContext) public returns (bytes32) {

```


*GitHub* : [640](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L640-L640)

### [G-21]<a name="g-21"></a> Nesting `if`-statements is cheaper than using `&&`

Using a double if statement instead of logical AND (&&) can provide similar short-circuiting behavior whereas double if is slightly more efficient.

*There are 17 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/MIPS.sol

504:             if (_storeReg != 0 && _conditional) {

741:             if ((opcode >= 4 && opcode < 8) || opcode == 1) {

754:                 if (opcode >= 0x28 && opcode != 0x30) {

766:             if (opcode == 0 && func >= 8 && func < 0x1c) {

788:                 if (func >= 0x10 && func < 0x1c) {

794:             if (opcode == 0x38 && rtReg != 0) {

813:             if (opcode == 0 || (opcode >= 8 && opcode < 0xF)) {

```


*GitHub* : [504](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L504-L504), [741](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L741-L741), [754](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L754-L754), [766](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L766-L766), [788](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L788-L788), [794](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L794-L794), [813](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L813-L813)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

581:         if (
582:             !(
583:                 _verify(_preStateProof, root, _preState.index, _hashLeaf(_preState))
584:                     && _verify(_postStateProof, root, _postState.index, _hashLeaf(_postState))
585:             )

661:         if (
662:             !(
663:                 _verify(_preStateProof, root, _preState.index, _hashLeaf(_preState))
664:                     && _verify(_postStateProof, root, _postState.index, _hashLeaf(_postState))
665:             )

727:         if (offset < 8 && currentSize == 0) {

735:         } else if (offset >= 8 && (offset = offset - 8) >= currentSize && offset < currentSize + _input.length) {

742:             if (relativeOffset + 32 >= _input.length && !_finalize) revert PartOffsetOOB();

```


*GitHub* : [581](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L581-L585), [661](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L661-L665), [727](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L727-L727), [735](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L735-L735), [742](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L742-L742)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

339:         if ((_challengeIndex == 0 || nextPositionDepth == SPLIT_DEPTH + 2) && !_isAttack) {

345:         if (l2BlockNumberChallenged && _challengeIndex == 0) revert L2BlockNumberChallenged();

580:         if (challengeIndicesLen == 0 && _claimIndex != 0) {

621:             if (claim.counteredBy == address(0) && checkpoint.leftmostPosition.raw() > claim.position.raw()) {

643:             if (_claimIndex == 0 && l2BlockNumberChallenged) {

```


*GitHub* : [339](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L339-L339), [345](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L345-L345), [580](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L580-L580), [621](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L621-L621), [643](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L643-L643)

### [G-22]<a name="g-22"></a> Consider using OpenZeppelin's `EnumerateSet` instead of nested mappings

Nested mappings and multi-dimensional arrays in Solidity operate through a process of double hashing, wherein the original storage slot and the first key are concatenated and hashed, and then this hash is again concatenated with the second key and hashed. This process can be quite gas expensive due to the double-hashing operation and subsequent storage operation (sstore).
OpenZeppelin's `EnumerableSet` provides a potential solution to this problem. It creates a data structure that combines the benefits of set operations with the ability to enumerate stored elements, which is not natively available in Solidity. EnumerableSet handles the element uniqueness internally and can therefore provide a more gas-efficient and collision-resistant alternative to nested mappings or multi-dimensional arrays in certain scenarios.

*There are 7 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

42:     mapping(bytes32 => mapping(uint256 => bytes32)) public preimageParts;

44:     mapping(bytes32 => mapping(uint256 => bool)) public preimagePartOk;

74:     mapping(address => mapping(uint256 => bytes32[KECCAK_TREE_DEPTH])) public proposalBranches;

77:     mapping(address => mapping(uint256 => LPPMetaData)) public proposalMetadata;

79:     mapping(address => mapping(uint256 => uint256)) public proposalBonds;

81:     mapping(address => mapping(uint256 => bytes32)) public proposalParts;

83:     mapping(address => mapping(uint256 => uint64[])) public proposalBlocks;

```


*GitHub* : [42](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L42-L42), [44](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L44-L44), [74](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L74-L74), [77](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L77-L77), [79](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L79-L79), [81](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L81-L81), [83](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L83-L83)

### [G-23]<a name="g-23"></a> Operator `>=`/`<=` costs less gas than operator `>`/`<`

The compiler uses opcodes `GT` and `ISZERO` for code that uses `>`, but only requires `LT` for `>=`. A similar behavior applies for `>`, which uses opcodes `LT` and `ISZERO`, but only requires `GT` for `<=`. It can [save 3 gas](https://gist.github.com/IllIllI000/3dc79d25acccfa16dee4e83ffdc6ffde) for each. It should be converted to the `<=`/`>=` equivalent when comparing against integer literals.

*There are 31 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/MIPS.sol

344:                 shouldBranch = int32(_rs) > 0;

351:                     shouldBranch = int32(_rs) < 0;

501:             require(_storeReg < 32, "valid register");

723:             } else if (opcode < 0x20) {

741:             if ((opcode >= 4 && opcode < 8) || opcode == 1) {

766:             if (opcode == 0 && func >= 8 && func < 0x1c) {

788:                 if (func >= 0x10 && func < 0x1c) {

813:             if (opcode == 0 || (opcode >= 8 && opcode < 0xF)) {

953:                     return int32(rs) < int32(rt) ? 1 : 0;

957:                     return rs < rt ? 1 : 0;

```


*GitHub* : [344](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L344-L344), [351](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L351-L351), [501](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L501-L501), [723](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L723-L723), [741](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L741-L741), [766](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L766-L766), [788](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L788-L788), [813](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L813-L813), [953](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L953-L953), [957](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L957-L957)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

94:         for (uint256 height = 0; height < KECCAK_TREE_DEPTH - 1; height++) {

134:         if (_partOffset > _size + 8 || _size > 32) {

419:         if (msg.value < MIN_BOND_SIZE) revert InsufficientBond();

428:         if (_claimedSize < MIN_LPP_SIZE_BYTES) revert InvalidInputSize();

535:         if (blocksProcessed > MAX_LEAF_COUNT) revert TreeSizeOverflow();

698:         for (uint256 height = 0; height < KECCAK_TREE_DEPTH; height++) {

727:         if (offset < 8 && currentSize == 0) {

735:         } else if (offset >= 8 && (offset = offset - 8) >= currentSize && offset < currentSize + _input.length) {

```


*GitHub* : [94](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L94-L94), [134](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L134-L134), [419](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L419-L419), [428](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L428-L428), [535](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L535-L535), [698](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L698-L698), [727](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L727-L727), [735](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L735-L735)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

138:         if (_maxGameDepth > LibPosition.MAX_POSITION_BITLEN - 1) revert MaxDepthTooLarge();

142:         if (_clockExtension.raw() > _maxClockDuration.raw()) revert InvalidClockExtension();

351:         if (nextPositionDepth > MAX_GAME_DEPTH) revert GameDepthExceeded();

377:         if (nextDuration.raw() > MAX_CLOCK_DURATION.raw() - CLOCK_EXTENSION.raw()) {

516:         if (rawBlockNumber.length > 32) revert InvalidHeaderRLP();

570:         if (challengeClockDuration.raw() < MAX_CLOCK_DURATION.raw()) revert ClockNotExpired();

606:         uint256 finalCursor = lastToResolve > challengeIndicesLen ? challengeIndicesLen : lastToResolve;

607:         for (uint256 i = checkpoint.subgameIndex; i < finalCursor; i++) {

621:             if (claim.counteredBy == address(0) && checkpoint.leftmostPosition.raw() > claim.position.raw()) {

704:         if (depth > MAX_GAME_DEPTH) revert GameDepthExceeded();

785:         duration_ = challengeDuration > MAX_CLOCK_DURATION.raw() ? MAX_CLOCK_DURATION : Duration.wrap(challengeDuration);

951:         while ((currentDepth = claim.position.depth()) > SPLIT_DEPTH) {

978:             if (outputPos.indexAtDepth() > 0) {

```


*GitHub* : [138](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L138-L138), [142](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L142-L142), [351](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L351-L351), [377](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L377-L377), [516](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L516-L516), [570](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L570-L570), [606](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L606-L606), [607](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L607-L607), [621](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L621-L621), [704](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L704-L704), [785](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L785-L785), [951](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L951-L951), [978](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L978-L978)

### [G-24]<a name="g-24"></a> Using `private` for constants saves gas

If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that [returns a tuple](https://github.com/code-423n4/2022-08-frax/blob/90f55a9ce4e25bceed3a74290b854341d8de6afa/src/contracts/FraxlendPair.sol#L156-L178) of the values of all currently-public constants. Saves **3406-3606 gas** in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it's used, and not adding another entry to the method ID table

*There are 19 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/MIPS.sol

43:     uint32 public constant BRK_START = 0x40000000;

47:     string public constant version = "1.0.1";

49:     uint32 internal constant FD_STDIN = 0;

50:     uint32 internal constant FD_STDOUT = 1;

51:     uint32 internal constant FD_STDERR = 2;

52:     uint32 internal constant FD_HINT_READ = 3;

53:     uint32 internal constant FD_HINT_WRITE = 4;

54:     uint32 internal constant FD_PREIMAGE_READ = 5;

55:     uint32 internal constant FD_PREIMAGE_WRITE = 6;

57:     uint32 internal constant EBADF = 0x9;

58:     uint32 internal constant EINVAL = 0x16;

```


*GitHub* : [43](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L43-L43), [47](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L47-L47), [49](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L49-L49), [50](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L50-L50), [51](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L51-L51), [52](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L52-L52), [53](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L53-L53), [54](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L54-L54), [55](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L55-L55), [57](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L57-L57), [58](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L58-L58)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

25:     uint256 public constant MIN_BOND_SIZE = 0.25 ether;

27:     uint256 public constant KECCAK_TREE_DEPTH = 16;

29:     uint256 public constant MAX_LEAF_COUNT = 2 ** KECCAK_TREE_DEPTH - 1;

33:     string public constant version = "1.0.0";

```


*GitHub* : [25](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L25-L25), [27](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L27-L27), [29](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L29-L29), [33](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L33-L33)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol

25:     string public constant version = "1.0.0";

```


*GitHub* : [25](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L25-L25)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

64:     Position internal constant ROOT_POSITION = Position.wrap(1);

69:     uint256 internal constant HEADER_BLOCK_NUMBER_INDEX = 8;

73:     string public constant version = "1.2.0";

```


*GitHub* : [64](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L64-L64), [69](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L69-L69), [73](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L73-L73)

### [G-25]<a name="g-25"></a> Use assembly scratch space for building calldata for external calls

If an external call's calldata can fit into two or fewer words, use the scratch space to build the calldata, rather than allowing Solidity to do a memory expansion.

*There are 41 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

460:         uint256 blocksProcessed = metaData.blocksProcessed();

595:         LibKeccak.absorb(_stateMatrix, _postState.input);

596:         LibKeccak.permutation(_stateMatrix);

626:         LibKeccak.absorb(stateMatrix, _postState.input);

627:         LibKeccak.permutation(stateMatrix);

654:         if (metaData.countered()) revert BadProposal();

678:         LibKeccak.absorb(_stateMatrix, _postState.input);

679:         LibKeccak.permutation(_stateMatrix);

680:         bytes32 finalDigest = LibKeccak.squeeze(_stateMatrix);

686:         uint256 partOffset = metaData.partOffset();

697:         uint256 size = proposalMetadata[_owner][_uuid].blocksProcessed();

722:         uint256 offset = _metaData.partOffset();

723:         uint256 claimedSize = _metaData.claimedSize();

792:         (bool success,) = _to.call{ value: bond }("");

```


*GitHub* : [460](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L460-L460), [595](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L595-L595), [596](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L596-L596), [626](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L626-L626), [627](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L627-L627), [654](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L654-L654), [678](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L678-L678), [679](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L679-L679), [680](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L680-L680), [686](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L686-L686), [697](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L697-L697), [722](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L722-L722), [723](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L723-L723), [792](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L792-L792)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol

69:         (, Timestamp timestamp, address proxy) = _disputeGames[uuid].unpack();

79:         (GameType gameType, Timestamp timestamp, address proxy) = _disputeGameList[_index].unpack();

126:         GameId id = LibGameId.pack(_gameType, Timestamp.wrap(uint64(block.timestamp)), address(proxy_));

170:             (GameType gameType, Timestamp timestamp, address proxy) = id.unpack();

181:                 Claim rootClaim = IDisputeGame(proxy).rootClaim();

```


*GitHub* : [69](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L69-L69), [79](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L79-L79), [126](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L126-L126), [170](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L170-L170), [181](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L181-L181)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

173:         (Hash root, uint256 rootBlockNumber) = ANCHOR_STATE_REGISTRY.anchors(GAME_TYPE);

252:         Position stepPos = parentPos.move(_isAttack);

333:         Position nextPosition = parentPos.move(_isAttack);

334:         uint256 nextPositionDepth = nextPosition.depth();

385:         Clock nextClock = LibClock.wrap(nextDuration, Timestamp.wrap(uint64(block.timestamp)));

390:         Hash claimHash = _claim.hashClaimPos(nextPosition, _challengeIndex);

437:         IPreimageOracle oracle = VM.oracle();

440:             oracle.loadLocalData(_ident, uuid.raw(), l1Head().raw(), 32, _partOffset);

443:             oracle.loadLocalData(_ident, uuid.raw(), starting.raw(), 32, _partOffset);

446:             oracle.loadLocalData(_ident, uuid.raw(), disputed.raw(), 32, _partOffset);

455:             oracle.loadLocalData(_ident, uuid.raw(), bytes32(l2Number << 0xC0), 8, _partOffset);

458:             oracle.loadLocalData(_ident, uuid.raw(), bytes32(L2_CHAIN_ID << 0xC0), 8, _partOffset);

556:         ANCHOR_STATE_REGISTRY.tryUpdateAnchorState();

730:         uint256 bOverC = FixedPointMathLib.divWad(b, c);

734:         uint256 numerator = FixedPointMathLib.mulWad(lnA, bOverC);

735:         int256 base = FixedPointMathLib.expWad(int256(numerator));

738:         int256 rawGas = FixedPointMathLib.powWad(base, int256(depth * FixedPointMathLib.WAD));

739:         uint256 requiredGas = FixedPointMathLib.mulWad(baseGasCharged, uint256(rawGas));

756:         WETH.withdraw(_recipient, recipientCredit);

759:         (bool success,) = _recipient.call{ value: recipientCredit }(hex"");

857:         WETH.unlock(_recipient, bond);

879:         Position disputedLeafPos = Position.wrap(_parentPos.raw() + 1);

```


*GitHub* : [173](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L173-L173), [252](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L252-L252), [333](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L333-L333), [334](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L334-L334), [385](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L385-L385), [390](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L390-L390), [437](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L437-L437), [440](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L440-L440), [443](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L443-L443), [446](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L446-L446), [455](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L455-L455), [458](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L458-L458), [556](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L556-L556), [730](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L730-L730), [734](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L734-L734), [735](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L735-L735), [738](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L738-L738), [739](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L739-L739), [756](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L756-L756), [759](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L759-L759), [857](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L857-L857), [879](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L879-L879)

### [G-26]<a name="g-26"></a> Usage of smaller `uint`/`int` types causes overhead

When using a smaller int/uint type it first needs to be converted to it's 258 bit counterpart to be operated, this increases the gass cost and thus should be avoided. However it does make sense to use smaller int/uint values within structs provided you pack the struct properly.

*There are 78 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/MIPS.sol

43:     uint32 public constant BRK_START = 0x40000000;

49:     uint32 internal constant FD_STDIN = 0;

50:     uint32 internal constant FD_STDOUT = 1;

51:     uint32 internal constant FD_STDERR = 2;

52:     uint32 internal constant FD_HINT_READ = 3;

53:     uint32 internal constant FD_HINT_WRITE = 4;

54:     uint32 internal constant FD_PREIMAGE_READ = 5;

55:     uint32 internal constant FD_PREIMAGE_WRITE = 6;

57:     uint32 internal constant EBADF = 0x9;

58:     uint32 internal constant EINVAL = 0x16;

75:     function SE(uint32 _dat, uint32 _idx) internal pure returns (uint32 out_) {

160:             uint32 syscall_no = state.registers[2];

161:             uint32 v0 = 0;

162:             uint32 v1 = 0;

165:             uint32 a0 = state.registers[4];

166:             uint32 a1 = state.registers[5];

167:             uint32 a2 = state.registers[6];

171:                 uint32 sz = a1;

207:                     uint32 mem = readMem(a1 & 0xFFffFFfc, 1); // mask the addr to align it to 4 bytes

256:                     uint32 mem = readMem(a1 & 0xFFffFFfc, 1); // mask the addr to align it to 4 bytes

319:     function handleBranch(uint32 _opcode, uint32 _insn, uint32 _rtReg, uint32 _rs) internal returns (bytes32 out_) {

335:                 uint32 rt = state.registers[_rtReg];

349:                 uint32 rtv = ((_insn >> 16) & 0x1F);

359:             uint32 prevPC = state.pc;

383:     function handleHiLo(uint32 _func, uint32 _rs, uint32 _rt, uint32 _storeReg) internal returns (bytes32 out_) {

391:             uint32 val;

411:                 uint64 acc = uint64(int64(int32(_rs)) * int64(int32(_rt)));

417:                 uint64 acc = uint64(uint64(_rs) * uint64(_rt));

460:     function handleJump(uint32 _linkReg, uint32 _dest) internal returns (bytes32 out_) {

473:             uint32 prevPC = state.pc;

492:     function handleRd(uint32 _storeReg, uint32 _val, bool _conditional) internal returns (bytes32 out_) {

520:     function proofOffset(uint8 _proofIndex) internal pure returns (uint256 offset_) {

538:     function readMem(uint32 _addr, uint8 _proofIndex) internal pure returns (uint32 out_) {

592:     function writeMem(uint32 _addr, uint8 _proofIndex, uint32 _val) internal pure {

700:             uint32 insn = readMem(state.pc, 0);

701:             uint32 opcode = insn >> 26; // 6-bits

706:                 uint32 target = (state.nextPC & 0xF0000000) | (insn & 0x03FFFFFF) << 2;

711:             uint32 rs; // source register 1 value

712:             uint32 rt; // source register 2 / temp value

713:             uint32 rtReg = (insn >> 16) & 0x1F;

717:             uint32 rdReg = rtReg;

745:             uint32 storeAddr = 0xFF_FF_FF_FF;

748:             uint32 mem;

752:                 uint32 addr = rs & 0xFFFFFFFC;

763:             uint32 val = execute(insn, rs, rt, mem) & 0xffFFffFF; // swr outputs more than 4 bytes without the mask

765:             uint32 func = insn & 0x3f; // 6-bits

809:     function execute(uint32 insn, uint32 rs, uint32 rt, uint32 mem) internal pure returns (uint32 out) {

811:             uint32 opcode = insn >> 26; // 6-bits

814:                 uint32 func = insn & 0x3f; // 6-bits

844:                     uint32 shamt = (insn >> 6) & 0x1F;

964:                     uint32 func = insn & 0x3f; // 6-bits

974:                         uint32 i = 0;

996:                     uint32 val = mem << ((rs & 3) * 8);

997:                     uint32 mask = uint32(0xFFFFFFFF) << ((rs & 3) * 8);

1014:                     uint32 val = mem >> (24 - (rs & 3) * 8);

1015:                     uint32 mask = uint32(0xFFFFFFFF) >> (24 - (rs & 3) * 8);

1020:                     uint32 val = (rt & 0xFF) << (24 - (rs & 3) * 8);

1021:                     uint32 mask = 0xFFFFFFFF ^ uint32(0xFF << (24 - (rs & 3) * 8));

1026:                     uint32 val = (rt & 0xFFFF) << (16 - (rs & 2) * 8);

1027:                     uint32 mask = 0xFFFFFFFF ^ uint32(0xFFFF << (16 - (rs & 2) * 8));

1032:                     uint32 val = rt >> ((rs & 3) * 8);

1033:                     uint32 mask = uint32(0xFFFFFFFF) >> ((rs & 3) * 8);

1042:                     uint32 val = rt << (24 - (rs & 3) * 8);

1043:                     uint32 mask = uint32(0xFFFFFFFF) << (24 - (rs & 3) * 8);

```


*GitHub* : [43](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L43-L43), [49](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L49-L49), [50](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L50-L50), [51](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L51-L51), [52](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L52-L52), [53](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L53-L53), [54](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L54-L54), [55](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L55-L55), [57](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L57-L57), [58](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L58-L58), [75](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L75-L75), [160](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L160-L160), [161](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L161-L161), [162](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L162-L162), [165](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L165-L165), [166](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L166-L166), [167](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L167-L167), [171](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L171-L171), [207](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L207-L207), [256](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L256-L256), [319](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L319-L319), [335](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L335-L335), [349](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L349-L349), [359](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L359-L359), [383](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L383-L383), [391](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L391-L391), [411](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L411-L411), [417](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L417-L417), [460](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L460-L460), [473](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L473-L473), [492](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L492-L492), [520](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L520-L520), [538](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L538-L538), [592](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L592-L592), [700](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L700-L700), [701](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L701-L701), [706](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L706-L706), [711](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L711-L711), [712](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L712-L712), [713](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L713-L713), [717](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L717-L717), [745](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L745-L745), [748](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L748-L748), [752](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L752-L752), [763](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L763-L763), [765](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L765-L765), [809](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L809-L809), [811](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L811-L811), [814](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L814-L814), [844](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L844-L844), [964](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L964-L964), [974](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L974-L974), [996](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L996-L996), [997](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L997-L997), [1014](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1014-L1014), [1015](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1015-L1015), [1020](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1020-L1020), [1021](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1021-L1021), [1026](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1026-L1026), [1027](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1027-L1027), [1032](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1032-L1032), [1033](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1033-L1033), [1042](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1042-L1042), [1043](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1043-L1043)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

417:     function initLPP(uint256 _uuid, uint32 _partOffset, uint32 _claimedSize) external payable {

```


*GitHub* : [417](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L417-L417)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol

24:     function setTimestamp(LPPMetaData _self, uint64 _timestamp) internal pure returns (LPPMetaData self_) {

30:     function setPartOffset(LPPMetaData _self, uint32 _partOffset) internal pure returns (LPPMetaData self_) {

36:     function setClaimedSize(LPPMetaData _self, uint32 _claimedSize) internal pure returns (LPPMetaData self_) {

42:     function setBlocksProcessed(LPPMetaData _self, uint32 _blocksProcessed) internal pure returns (LPPMetaData self_) {

48:     function setBytesProcessed(LPPMetaData _self, uint32 _bytesProcessed) internal pure returns (LPPMetaData self_) {

60:     function timestamp(LPPMetaData _self) internal pure returns (uint64 timestamp_) {

66:     function partOffset(LPPMetaData _self) internal pure returns (uint64 partOffset_) {

72:     function claimedSize(LPPMetaData _self) internal pure returns (uint32 claimedSize_) {

78:     function blocksProcessed(LPPMetaData _self) internal pure returns (uint32 blocksProcessed_) {

84:     function bytesProcessed(LPPMetaData _self) internal pure returns (uint32 bytesProcessed_) {

```


*GitHub* : [24](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L24-L24), [30](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L30-L30), [36](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L36-L36), [42](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L42-L42), [48](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L48-L48), [60](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L60-L60), [66](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L66-L66), [72](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L72-L72), [78](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L78-L78), [84](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L84-L84)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

379:             uint64 extensionPeriod =

783:         uint64 challengeDuration =

881:         uint8 vmStatus = uint8(_rootClaim.raw()[0]);

```


*GitHub* : [379](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L379-L379), [783](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L783-L783), [881](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L881-L881)

### [G-27]<a name="g-27"></a> Consider using solady's 'FixedPointMathLib'

Using Solady's 'FixedPointMathLib' for multiplication or division operations in Solidity can lead to significant gas savings. This library is designed to optimize fixed-point arithmetic operations, which are common in financial calculations involving tokens or currencies. By implementing more efficient algorithms and assembly optimizations, 'FixedPointMathLib' minimizes the computational resources required for these operations. For contracts that frequently perform such calculations, integrating this library can reduce transaction costs, thereby enhancing overall performance and cost-effectiveness. However, developers must ensure compatibility with their existing codebase and thoroughly test for accuracy and expected behavior to avoid any unintended consequences.

*There are 25 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/MIPS.sol

411:                 uint64 acc = uint64(int64(int32(_rs)) * int64(int32(_rt)));

417:                 uint64 acc = uint64(uint64(_rs) * uint64(_rt));

429:                 state.lo = uint32(int32(_rs) / int32(_rt));

439:                 state.lo = _rs / _rt;

524:             offset_ = 420 + (uint256(_proofIndex) * (28 * 32));

967:                         return uint32(int32(rs) * int32(rt));

988:                     return SE((mem >> (24 - (rs & 3) * 8)) & 0xFF, 8);

992:                     return SE((mem >> (16 - (rs & 2) * 8)) & 0xFFFF, 16);

996:                     uint32 val = mem << ((rs & 3) * 8);

997:                     uint32 mask = uint32(0xFFFFFFFF) << ((rs & 3) * 8);

1006:                     return (mem >> (24 - (rs & 3) * 8)) & 0xFF;

1010:                     return (mem >> (16 - (rs & 2) * 8)) & 0xFFFF;

1014:                     uint32 val = mem >> (24 - (rs & 3) * 8);

1015:                     uint32 mask = uint32(0xFFFFFFFF) >> (24 - (rs & 3) * 8);

1020:                     uint32 val = (rt & 0xFF) << (24 - (rs & 3) * 8);

1021:                     uint32 mask = 0xFFFFFFFF ^ uint32(0xFF << (24 - (rs & 3) * 8));

1026:                     uint32 val = (rt & 0xFFFF) << (16 - (rs & 2) * 8);

1027:                     uint32 mask = 0xFFFFFFFF ^ uint32(0xFFFF << (16 - (rs & 2) * 8));

1032:                     uint32 val = rt >> ((rs & 3) * 8);

1033:                     uint32 mask = uint32(0xFFFFFFFF) >> ((rs & 3) * 8);

1042:                     uint32 val = rt << (24 - (rs & 3) * 8);

1043:                     uint32 mask = uint32(0xFFFFFFFF) << (24 - (rs & 3) * 8);

```


*GitHub* : [411](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L411-L411), [417](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L417-L417), [429](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L429-L429), [439](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L439-L439), [524](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L524-L524), [967](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L967-L967), [988](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L988-L988), [992](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L992-L992), [996](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L996-L996), [997](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L997-L997), [1006](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1006-L1006), [1010](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1010-L1010), [1014](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1014-L1014), [1015](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1015-L1015), [1020](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1020-L1020), [1021](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1021-L1021), [1026](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1026-L1026), [1027](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1027-L1027), [1032](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1032-L1032), [1033](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1033-L1033), [1042](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1042-L1042), [1043](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1043-L1043)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

380:                 nextPositionDepth == SPLIT_DEPTH - 1 ? CLOCK_EXTENSION.raw() * 2 : CLOCK_EXTENSION.raw();

721:         uint256 a = highGasCharged / baseGasCharged;

742:         requiredBond_ = assumedBaseFee * requiredGas;

```


*GitHub* : [380](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L380-L380), [721](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L721-L721), [742](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L742-L742)

### [G-28]<a name="g-28"></a> Newer versions of solidity are more gas efficient

he solidity language continues to pursue more efficient gas optimization schemes. Adopting a [newer version of solidity](https://github.com/ethereum/solc-js/tags) can be more gas efficient.

*There are 6 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/MIPS.sol

2: pragma solidity 0.8.15;

```


*GitHub* : [2](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L2-L2)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageKeyLib.sol

2: pragma solidity 0.8.15;

```


*GitHub* : [2](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageKeyLib.sol#L2-L2)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

2: pragma solidity 0.8.15;

```


*GitHub* : [2](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L2-L2)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol

2: pragma solidity 0.8.15;

```


*GitHub* : [2](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L2-L2)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol

2: pragma solidity 0.8.15;

```


*GitHub* : [2](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L2-L2)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

2: pragma solidity 0.8.15;

```


*GitHub* : [2](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L2-L2)

### [G-29]<a name="g-29"></a> Stack variable cost less than state variables while used in emiting event

When emitting events in Solidity, using stack variables (local variables within a function) instead of state variables can lead to significant gas savings. Stack variables reside in memory only for the duration of the function execution and are less costly to access compared to state variables, which are stored on the blockchain. When an event is emitted, accessing these stack variables requires less gas than fetching data from state variables, which involves reading from the contract's storage - a more expensive operation. Thus, for efficiency, prefer using local variables within functions for event emission, especially in functions that are called frequently.

*There are 1 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

// @audit  status
553:         emit Resolved(status = status_);

```


*GitHub* : [553](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L553-L553)

### [G-30]<a name="g-30"></a> Mappings are cheaper to use than storage arrays

When using storage arrays, solidity adds an internal lookup of the array's length (a Gcoldsload **2100 gas**) to ensure you don't read past the array's end. You can avoid this lookup by using a `mapping` and storing the number of entries in a separate storage variable. In cases where you have sentinel values (e.g. 'zero' means invalid), you can avoid length checks

*There are 4 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

70:     bytes32[KECCAK_TREE_DEPTH] public zeroHashes;

72:     LargePreimageProposalKeys[] public proposals;

```


*GitHub* : [70](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L70-L70), [72](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L72-L72)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol

39:     GameId[] internal _disputeGameList;

```


*GitHub* : [39](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L39-L39)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

95:     ClaimData[] public claimData;

```


*GitHub* : [95](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L95-L95)

### [G-31]<a name="g-31"></a> Gas savings can be achieved by changing the model for assigning value to the structure

Here's an example to illustrate this:

```solidity
struct MyStruct {
  uint256 a;
  uint256 b;
  uint256 c;
}

function assignValuesToStruct(uint256 _a, uint256 _b, uint256 _c) public {
  MyStruct memory myStruct = MyStruct(_a, _b, _c);
  // Do something with myStruct
}
```

In this example, we have a simple MyStruct data structure with three uint256 fields. The assignValuesToStruct function takes three input parameters _a, _b, and _c, and assigns them to the corresponding fields in a new myStruct variable. This is done using the struct constructor syntax, which creates a new instance of the MyStruct struct with the specified field values.

The following approach can be more efficient than assigning values to the struct fields:

```solidity
function assignValuesToStruct(uint256 _a, uint256 _b, uint256 _c) public {
  MyStruct memory myStruct;
  myStruct.a = _a;
  myStruct.b = _b;
  myStruct.c = _c;
  // Do something with myStruct
}
```

By changing the pattern of assigning value to the structure, gas savings of ~130 per instance are achieved. In addition, this use will provide significant savings in distribution costs.

*There are 2 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol

182:                 games_[games_.length - 1] = GameSearchResult({
183:                     index: i,
184:                     metadata: id,
185:                     timestamp: timestamp,
186:                     rootClaim: rootClaim,
187:                     extraData: extraData
188:                 });

```


*GitHub* : [182](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L182-L188)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

179:         startingOutputRoot = OutputRoot({ l2BlockNumber: rootBlockNumber, root: root });

```


*GitHub* : [179](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L179-L179)

### [G-32]<a name="g-32"></a> Optimize Storage with Byte Truncation for Time Related State Variables

Storage optimization in Solidity contracts is vital for reducing gas costs, especially when storing time-related state variables. Using `uint32` for storing time values like timestamps is often sufficient, given it can represent dates up to the year 2106. By truncating larger default integer types to `uint32`, you significantly save on storage space and consequently on gas costs for deployment and state modifications. However, ensure that the truncation does not lead to overflow issues and that the variable's size is adequate for the application's expected lifespan and precision requirements. Adopting this optimization practice contributes to more efficient and cost-effective smart contract development.

*There are 7 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

21:     uint256 internal immutable CHALLENGE_PERIOD;

89:     constructor(uint256 _minProposalSize, uint256 _challengePeriod) {

407:     function challengePeriod() external view returns (uint256 challengePeriod_) {

```


*GitHub* : [21](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L21-L21), [89](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L89-L89), [407](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L407-L407)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol

24:     function setTimestamp(LPPMetaData _self, uint64 _timestamp) internal pure returns (LPPMetaData self_) {

60:     function timestamp(LPPMetaData _self) internal pure returns (uint64 timestamp_) {

```


*GitHub* : [24](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L24-L24), [60](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L60-L60)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

379:             uint64 extensionPeriod =

783:         uint64 challengeDuration =

```


*GitHub* : [379](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L379-L379), [783](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L783-L783)

### [G-33]<a name="g-33"></a> Trade-offs Between Modifiers and Internal Functions

In Solidity, both modifiers and internal functions can be used to modularize and reuse code, but they have different trade-offs.

Modifiers are primarily used to augment the behavior of functions, often for checks or validations. They can access parameters of the function they modify and are integrated into the function’s code at compile time. This makes them syntactically cleaner for repetitive precondition checks. However, modifiers can sometimes lead to less readable code, especially when the logic is complex or when multiple modifiers are used on a single function.

Internal functions, on the other hand, offer more flexibility. They can contain complex logic, return values, and be called from other functions. This makes them more suitable for reusable chunks of business logic. Since internal functions are separate entities, they can be more readable and easier to test in isolation compared to modifiers.

Using internal functions can result in slightly more gas consumption, as it involves an internal function call. However, this cost is usually minimal and can be a worthwhile trade-off for increased code clarity and maintainability.

In summary, while modifiers offer a concise way to include checks and simple logic across multiple functions, internal functions provide more flexibility and are better suited for complex and reusable code. The choice between the two should be based on the specific use case, considering factors like code complexity, readability, and gas efficiency.

*There are 36 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/MIPS.sol

75:     function SE(uint32 _dat, uint32 _idx) internal pure returns (uint32 out_) {

86:     function outputState() internal returns (bytes32 out_) {

151:     function handleSyscall(bytes32 _localContext) internal returns (bytes32 out_) {

319:     function handleBranch(uint32 _opcode, uint32 _insn, uint32 _rtReg, uint32 _rs) internal returns (bytes32 out_) {

383:     function handleHiLo(uint32 _func, uint32 _rs, uint32 _rt, uint32 _storeReg) internal returns (bytes32 out_) {

460:     function handleJump(uint32 _linkReg, uint32 _dest) internal returns (bytes32 out_) {

492:     function handleRd(uint32 _storeReg, uint32 _val, bool _conditional) internal returns (bytes32 out_) {

520:     function proofOffset(uint8 _proofIndex) internal pure returns (uint256 offset_) {

538:     function readMem(uint32 _addr, uint8 _proofIndex) internal pure returns (uint32 out_) {

592:     function writeMem(uint32 _addr, uint8 _proofIndex, uint32 _val) internal pure {

809:     function execute(uint32 insn, uint32 rs, uint32 rt, uint32 mem) internal pure returns (uint32 out) {

```


*GitHub* : [75](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L75-L75), [86](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L86-L86), [151](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L151-L151), [319](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L319-L319), [383](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L383-L383), [460](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L460-L460), [492](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L492-L492), [520](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L520-L520), [538](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L538-L538), [592](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L592-L592), [809](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L809-L809)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageKeyLib.sol

12:     function localizeIdent(uint256 _ident, bytes32 _localContext) internal view returns (bytes32 key_) {

29:     function localize(bytes32 _key, bytes32 _localContext) internal view returns (bytes32 localizedKey_) {

47:     function keccak256PreimageKey(bytes memory _preimage) internal pure returns (bytes32 key_) {

```


*GitHub* : [12](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageKeyLib.sol#L12-L12), [29](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageKeyLib.sol#L29-L29), [47](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageKeyLib.sol#L47-L47)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

714:     function _extractPreimagePart(
715:         bytes calldata _input,
716:         uint256 _uuid,
717:         bool _finalize,
718:         LPPMetaData _metaData
719:     )
720:         internal
721:     {

756:     function _verify(
757:         bytes32[] calldata _proof,
758:         bytes32 _root,
759:         uint256 _index,
760:         bytes32 _leaf
761:     )
762:         internal
763:         pure
764:         returns (bool isValid_)
765:     {

788:     function _payoutBond(address _claimant, uint256 _uuid, address _to) internal {

797:     function _hashLeaf(Leaf memory _leaf) internal pure returns (bytes32 leaf_) {

```


*GitHub* : [714](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L714-L721), [756](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L756-L765), [788](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L788-L788), [797](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L797-L797)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol

24:     function setTimestamp(LPPMetaData _self, uint64 _timestamp) internal pure returns (LPPMetaData self_) {

30:     function setPartOffset(LPPMetaData _self, uint32 _partOffset) internal pure returns (LPPMetaData self_) {

36:     function setClaimedSize(LPPMetaData _self, uint32 _claimedSize) internal pure returns (LPPMetaData self_) {

42:     function setBlocksProcessed(LPPMetaData _self, uint32 _blocksProcessed) internal pure returns (LPPMetaData self_) {

48:     function setBytesProcessed(LPPMetaData _self, uint32 _bytesProcessed) internal pure returns (LPPMetaData self_) {

54:     function setCountered(LPPMetaData _self, bool _countered) internal pure returns (LPPMetaData self_) {

60:     function timestamp(LPPMetaData _self) internal pure returns (uint64 timestamp_) {

66:     function partOffset(LPPMetaData _self) internal pure returns (uint64 partOffset_) {

72:     function claimedSize(LPPMetaData _self) internal pure returns (uint32 claimedSize_) {

78:     function blocksProcessed(LPPMetaData _self) internal pure returns (uint32 blocksProcessed_) {

84:     function bytesProcessed(LPPMetaData _self) internal pure returns (uint32 bytesProcessed_) {

90:     function countered(LPPMetaData _self) internal pure returns (bool countered_) {

```


*GitHub* : [24](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L24-L24), [30](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L30-L30), [36](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L36-L36), [42](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L42-L42), [48](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L48-L48), [54](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L54-L54), [60](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L60-L60), [66](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L66-L66), [72](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L72-L72), [78](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L78-L78), [84](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L84-L84), [90](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L90-L90)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

849:     function _distributeBond(address _recipient, ClaimData storage _bonded) internal {

863:     function _verifyExecBisectionRoot(
864:         Claim _rootClaim,
865:         uint256 _parentIdx,
866:         Position _parentPos,
867:         bool _isAttack
868:     )
869:         internal
870:         view
871:     {

904:     function _findTraceAncestor(
905:         Position _pos,
906:         uint256 _start,
907:         bool _global
908:     )
909:         internal
910:         view
911:         returns (ClaimData storage ancestor_)
912:     {

931:     function _findStartingAndDisputedOutputs(uint256 _start)
932:         internal
933:         view
934:         returns (Claim startingClaim_, Position startingPos_, Claim disputedClaim_, Position disputedPos_)
935:     {

995:     function _findLocalContext(uint256 _claimIndex) internal view returns (Hash uuid_) {

1007:     function _computeLocalContext(
1008:         Claim _starting,
1009:         Position _startingPos,
1010:         Claim _disputed,
1011:         Position _disputedPos
1012:     )
1013:         internal
1014:         pure
1015:         returns (Hash uuid_)
1016:     {

```


*GitHub* : [849](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L849-L849), [863](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L863-L871), [904](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L904-L912), [931](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L931-L935), [995](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L995-L995), [1007](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L1007-L1016)

### [G-34]<a name="g-34"></a> Use `uint256(1)`/`uint256(2)` instead of `true`/`false` to save gas for changes

Use `uint256(1)` and `uint256(2)` for `true`/`false` to avoid a Gwarmaccess (100 gas), and to avoid Gsset (20000 gas) when changing from `false` to `true`, after having been `true` in the past. Refer to the [source](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/5ae630684a0f57de400ef69499addab4c32ac8fb/contracts/security/ReentrancyGuard.sol#L23-L27).

*There are 5 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

44:     mapping(bytes32 => mapping(uint256 => bool)) public preimagePartOk;

```


*GitHub* : [44](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L44-L44)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

85:     bool internal initialized;

88:     bool public l2BlockNumberChallenged;

101:     mapping(Hash => bool) public claims;

107:     mapping(uint256 => bool) public resolvedSubgames;

```


*GitHub* : [85](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L85-L85), [88](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L88-L88), [101](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L101-L101), [107](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L107-L107)

### [G-35]<a name="g-35"></a> Divisions can be `unchecked` to save gas

The expression `type(int).min/(-1)` is the only case where division causes an overflow. Therefore, uncheck can be used to [save gas](https://gist.github.com/DadeKuma/3bc597338ae774b8b3bd43280d55271f) in scenarios where it is certain that such an overflow will not occur.

*There are 1 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

721:         uint256 a = highGasCharged / baseGasCharged;

```


*GitHub* : [721](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L721-L721)

### [G-36]<a name="g-36"></a> Increments can be `unchecked` to save gas

Using `unchecked` increments can save gas by bypassing the built-in overflow checks. This can save [30-40 gas](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#the-increment-in-for-loop-post-condition-can-be-made-unchecked) per iteration. So it is recommended to use `unchecked` increments when overflow is not possible.

*There are 3 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

94:         for (uint256 height = 0; height < KECCAK_TREE_DEPTH - 1; height++) {

698:         for (uint256 height = 0; height < KECCAK_TREE_DEPTH; height++) {

```


*GitHub* : [94](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L94-L94), [698](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L698-L698)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

607:         for (uint256 i = checkpoint.subgameIndex; i < finalCursor; i++) {

```


*GitHub* : [607](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L607-L607)

### [G-37]<a name="g-37"></a> Avoid unnecessary public variables

Public state variables in Solidity automatically generate getter functions, increasing contract size and potentially leading to higher deployment and interaction costs. To optimize gas usage and contract efficiency, minimize the use of public variables unless external access is necessary. Instead, use internal or private visibility combined with explicit getter functions when required. This practice not only reduces contract size but also provides better control over data access and manipulation, enhancing security and readability. Prioritize lean, efficient contracts to ensure cost-effectiveness and better performance on the blockchain.

*There are 32 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/MIPS.sol

43:     uint32 public constant BRK_START = 0x40000000;

47:     string public constant version = "1.0.1";

```


*GitHub* : [43](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L43-L43), [47](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L47-L47)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

25:     uint256 public constant MIN_BOND_SIZE = 0.25 ether;

27:     uint256 public constant KECCAK_TREE_DEPTH = 16;

29:     uint256 public constant MAX_LEAF_COUNT = 2 ** KECCAK_TREE_DEPTH - 1;

33:     string public constant version = "1.0.0";

40:     mapping(bytes32 => uint256) public preimageLengths;

42:     mapping(bytes32 => mapping(uint256 => bytes32)) public preimageParts;

44:     mapping(bytes32 => mapping(uint256 => bool)) public preimagePartOk;

70:     bytes32[KECCAK_TREE_DEPTH] public zeroHashes;

72:     LargePreimageProposalKeys[] public proposals;

74:     mapping(address => mapping(uint256 => bytes32[KECCAK_TREE_DEPTH])) public proposalBranches;

77:     mapping(address => mapping(uint256 => LPPMetaData)) public proposalMetadata;

79:     mapping(address => mapping(uint256 => uint256)) public proposalBonds;

81:     mapping(address => mapping(uint256 => bytes32)) public proposalParts;

83:     mapping(address => mapping(uint256 => uint64[])) public proposalBlocks;

```


*GitHub* : [25](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L25-L25), [27](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L27-L27), [29](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L29-L29), [33](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L33-L33), [40](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L40-L40), [42](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L42-L42), [44](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L44-L44), [70](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L70-L70), [72](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L72-L72), [74](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L74-L74), [77](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L77-L77), [79](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L79-L79), [81](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L81-L81), [83](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L83-L83)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol

25:     string public constant version = "1.0.0";

28:     mapping(GameType => IDisputeGame) public gameImpls;

31:     mapping(GameType => uint256) public initBonds;

```


*GitHub* : [25](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L25-L25), [28](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L28-L28), [31](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L31-L31)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

73:     string public constant version = "1.2.0";

76:     Timestamp public createdAt;

79:     Timestamp public resolvedAt;

82:     GameStatus public status;

88:     bool public l2BlockNumberChallenged;

92:     address public l2BlockNumberChallenger;

95:     ClaimData[] public claimData;

98:     mapping(address => uint256) public credit;

101:     mapping(Hash => bool) public claims;

104:     mapping(uint256 => uint256[]) public subgames;

107:     mapping(uint256 => bool) public resolvedSubgames;

110:     mapping(uint256 => ResolutionCheckpoint) public resolutionCheckpoints;

113:     OutputRoot public startingOutputRoot;

```


*GitHub* : [73](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L73-L73), [76](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L76-L76), [79](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L79-L79), [82](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L82-L82), [88](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L88-L88), [92](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L92-L92), [95](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L95-L95), [98](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L98-L98), [101](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L101-L101), [104](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L104-L104), [107](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L107-L107), [110](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L110-L110), [113](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L113-L113)

### [G-38]<a name="g-38"></a> Use assembly to `emit` events

To efficiently `emit` events, it's possible to utilize assembly by making use of scratch space and the free memory pointer. This approach has the advantage of potentially avoiding the costs associated with memory expansion.
However, it's important to note that in order to safely optimize this process, it is preferable to cache and restore the free memory pointer.
A good example of such practice can be seen in [Solady's](https://github.com/Vectorized/solady/blob/main/src/tokens/ERC1155.sol#L167) codebase.

*There are 5 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol

131:         emit DisputeGameCreated(address(proxy_), _gameType, _rootClaim);

201:         emit ImplementationSet(address(_impl), _gameType);

207:         emit InitBondUpdated(_gameType, _initBond);

```


*GitHub* : [131](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L131-L131), [201](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L201-L201), [207](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L207-L207)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

415:         emit Move(_challengeIndex, _claim, msg.sender);

553:         emit Resolved(status = status_);

```


*GitHub* : [415](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L415-L415), [553](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L553-L553)

### [G-39]<a name="g-39"></a> Use assembly to validate `msg.sender`

We can use assembly to efficiently validate `msg.sender` with the least amount of opcodes necessary. For more details check the following report [Here](https://code4rena.com/reports/2023-05-juicebox#g-06-use-assembly-to-validate-msgsender)

*There are 2 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

422:         if (msg.sender != tx.origin) revert NotEOA();

465:         if (msg.sender != tx.origin) revert NotEOA();

```


*GitHub* : [422](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L422-L422), [465](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L465-L465)

### [G-40]<a name="g-40"></a> Simple checks for zero can be done using assembly to save gas



*There are 45 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/MIPS.sol

77:             bool isSigned = (_dat >> (_idx - 1)) != 0;

172:                 if (sz & 4095 != 0) {

176:                 if (a0 == 0) {

210:                     if (uint8(preimageKey[0]) == 1) {

340:                 shouldBranch = int32(_rs) <= 0;

344:                 shouldBranch = int32(_rs) > 0;

350:                 if (rtv == 0) {

351:                     shouldBranch = int32(_rs) < 0;

354:                     shouldBranch = int32(_rs) >= 0;

425:                 if (int32(_rt) == 0) {

435:                 if (_rt == 0) {

443:             if (_storeReg != 0) {

478:             if (_linkReg != 0) {

504:             if (_storeReg != 0 && _conditional) {

719:             if (opcode == 0 || opcode == 0x1c) {

766:             if (opcode == 0 && func >= 8 && func < 0x1c) {

774:                     return handleRd(rdReg, rs, rt == 0);

778:                     return handleRd(rdReg, rs, rt != 0);

794:             if (opcode == 0x38 && rtReg != 0) {

813:             if (opcode == 0 || (opcode >= 8 && opcode < 0xF)) {

975:                         while (rs & 0x80000000 != 0) {

```


*GitHub* : [77](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L77-L77), [172](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L172-L172), [176](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L176-L176), [210](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L210-L210), [340](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L340-L340), [344](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L344-L344), [350](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L350-L350), [351](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L351-L351), [354](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L354-L354), [425](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L425-L425), [435](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L435-L435), [443](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L443-L443), [478](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L478-L478), [504](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L504-L504), [719](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L719-L719), [766](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L766-L766), [774](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L774-L774), [778](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L778-L778), [794](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L794-L794), [813](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L813-L813), [975](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L975-L975)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

468:         if (metaData.claimedSize() == 0) revert NotInitialized();

471:         if (metaData.timestamp() != 0) revert AlreadyFinalized();

622:         if (_postState.index != 0) revert StatesNotContiguous();

727:         if (offset < 8 && currentSize == 0) {

```


*GitHub* : [468](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L468-L468), [471](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L471-L471), [622](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L622-L622), [727](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L727-L727)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol

97:         if (address(impl) == address(0)) revert NoImplementation(_gameType);

123:         if (GameId.unwrap(_disputeGames[uuid]) != bytes32(0)) revert GameAlreadyExists(uuid);

158:         if (_start >= _disputeGameList.length || _n == 0) return games_;

168:         for (uint256 i = _start; i >= 0 && i <= _start;) {

```


*GitHub* : [97](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L97-L97), [123](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L123-L123), [158](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L158-L158), [168](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L168-L168)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

176:         if (root.raw() == bytes32(0)) revert AnchorRootNotFound();

269:             preStateClaim = (stepPos.indexAtDepth() % (1 << (MAX_GAME_DEPTH - SPLIT_DEPTH))) == 0

303:         bool parentPostAgree = (parentPos.depth() - postState.position.depth()) % 2 == 0;

307:         if (parent.counteredBy != address(0)) revert DuplicateStep();

339:         if ((_challengeIndex == 0 || nextPositionDepth == SPLIT_DEPTH + 2) && !_isAttack) {

345:         if (l2BlockNumberChallenged && _challengeIndex == 0) revert L2BlockNumberChallenged();

549:         status_ = claimData[0].counteredBy == address(0) ? GameStatus.DEFENDER_WINS : GameStatus.CHALLENGER_WINS;

580:         if (challengeIndicesLen == 0 && _claimIndex != 0) {

586:             address recipient = counteredBy == address(0) ? subgameRootClaim.claimant : counteredBy;

601:             if (_numToResolve == 0) _numToResolve = challengeIndicesLen;

621:             if (claim.counteredBy == address(0) && checkpoint.leftmostPosition.raw() > claim.position.raw()) {

643:             if (_claimIndex == 0 && l2BlockNumberChallenged) {

652:                 _distributeBond(countered == address(0) ? subgameRootClaim.claimant : countered, subgameRootClaim);

753:         if (recipientCredit == 0) revert NoCreditToClaim();

978:             if (outputPos.indexAtDepth() > 0) {

1019:         uuid_ = _startingPos.raw() == 0

```


*GitHub* : [176](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L176-L176), [269](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L269-L269), [303](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L303-L303), [307](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L307-L307), [339](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L339-L339), [345](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L345-L345), [549](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L549-L549), [580](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L580-L580), [586](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L586-L586), [601](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L601-L601), [621](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L621-L621), [643](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L643-L643), [652](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L652-L652), [753](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L753-L753), [978](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L978-L978), [1019](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L1019-L1019)

### [G-41]<a name="g-41"></a> Use `byte32` in place of `string`

For strings of 32 char strings and below you can use bytes32 instead as it's more gas efficient

*There are 4 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/MIPS.sol

47:     string public constant version = "1.0.1";

```


*GitHub* : [47](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L47-L47)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

33:     string public constant version = "1.0.0";

```


*GitHub* : [33](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L33-L33)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol

25:     string public constant version = "1.0.0";

```


*GitHub* : [25](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L25-L25)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

73:     string public constant version = "1.2.0";

```


*GitHub* : [73](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L73-L73)

### [G-42]<a name="g-42"></a> Using delete instead of setting mapping to '0' saves gas



*There are 2 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

791:         proposalBonds[_claimant][_uuid] = 0;

```


*GitHub* : [791](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L791-L791)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

750:         credit[_recipient] = 0;

```


*GitHub* : [750](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L750-L750)

### [G-43]<a name="g-43"></a> `internal` functions not called by the contract should be removed to save deployment gas



*There are 14 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageKeyLib.sol

12:     function localizeIdent(uint256 _ident, bytes32 _localContext) internal view returns (bytes32 key_) {

47:     function keccak256PreimageKey(bytes memory _preimage) internal pure returns (bytes32 key_) {

```


*GitHub* : [12](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageKeyLib.sol#L12-L12), [47](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageKeyLib.sol#L47-L47)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol

24:     function setTimestamp(LPPMetaData _self, uint64 _timestamp) internal pure returns (LPPMetaData self_) {

30:     function setPartOffset(LPPMetaData _self, uint32 _partOffset) internal pure returns (LPPMetaData self_) {

36:     function setClaimedSize(LPPMetaData _self, uint32 _claimedSize) internal pure returns (LPPMetaData self_) {

42:     function setBlocksProcessed(LPPMetaData _self, uint32 _blocksProcessed) internal pure returns (LPPMetaData self_) {

48:     function setBytesProcessed(LPPMetaData _self, uint32 _bytesProcessed) internal pure returns (LPPMetaData self_) {

54:     function setCountered(LPPMetaData _self, bool _countered) internal pure returns (LPPMetaData self_) {

60:     function timestamp(LPPMetaData _self) internal pure returns (uint64 timestamp_) {

66:     function partOffset(LPPMetaData _self) internal pure returns (uint64 partOffset_) {

72:     function claimedSize(LPPMetaData _self) internal pure returns (uint32 claimedSize_) {

78:     function blocksProcessed(LPPMetaData _self) internal pure returns (uint32 blocksProcessed_) {

84:     function bytesProcessed(LPPMetaData _self) internal pure returns (uint32 bytesProcessed_) {

90:     function countered(LPPMetaData _self) internal pure returns (bool countered_) {

```


*GitHub* : [24](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L24-L24), [30](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L30-L30), [36](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L36-L36), [42](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L42-L42), [48](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L48-L48), [54](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L54-L54), [60](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L60-L60), [66](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L66-L66), [72](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L72-L72), [78](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L78-L78), [84](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L84-L84), [90](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L90-L90)

### [G-44]<a name="g-44"></a> Use `x = x + y` instead of `x += y` for state variables

Using `x = x + y` instead of `x += y` for simple state variables can save 10 gas. The same applies to `-=` , `*=` , etc.

*There are 1 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

854:         credit[_recipient] += bond;

```


*GitHub* : [854](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L854-L854)

### [G-45]<a name="g-45"></a> Use solady library where possible to save gas

The following OpenZeppelin imports have a Solady equivalent, as such they can be used to save GAS as Solady modules have been specifically designed to be as GAS efficient as possible

*There are 1 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol

5: import { OwnableUpgradeable } from "@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol";

```


*GitHub* : [5](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L5-L5)

### [G-46]<a name="g-46"></a> Variable declared within iteration

Please elaborate and generalise the following with detail and feel free to use your own knowledge and lmit ur words to 100 words please:

*There are 7 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol

169:             GameId id = _disputeGameList[i];

170:             (GameType gameType, Timestamp timestamp, address proxy) = id.unpack();

180:                 bytes memory extraData = IDisputeGame(proxy).extraData();

181:                 Claim rootClaim = IDisputeGame(proxy).rootClaim();

```


*GitHub* : [169](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L169-L169), [170](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L170-L170), [180](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L180-L180), [181](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L181-L181)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

608:             uint256 challengeIndex = challengeIndices[i];

613:             ClaimData storage claim = claimData[challengeIndex];

952:             uint256 parentIndex = claim.parentIndex;

```


*GitHub* : [608](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L608-L608), [613](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L613-L613), [952](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L952-L952)

### [G-47]<a name="g-47"></a> Enable IR-based code generation

Enabling Intermediate Representation (IR)-based code generation in Solidity, via the --via-ir compiler flag or setting {'viaIR': true} in the configuration, activates an advanced optimization phase. This approach allows the compiler to perform more sophisticated optimizations across multiple functions, potentially leading to significant gas savings. IR-based compilation can optimize contract bytecode more efficiently, making smart contracts cheaper to deploy and execute by reducing the gas required for transactions

*There are 6 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/MIPS.sol

23: contract MIPS is ISemver {

```


*GitHub* : [23](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L23-L23)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageKeyLib.sol

6: library PreimageKeyLib {

```


*GitHub* : [6](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageKeyLib.sol#L6-L6)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

15: contract PreimageOracle is IPreimageOracle, ISemver {

```


*GitHub* : [15](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L15-L15)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol

20: library LPPMetadataLib {

```


*GitHub* : [20](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L20-L20)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol

19: contract DisputeGameFactory is OwnableUpgradeable, IDisputeGameFactory, ISemver {

```


*GitHub* : [19](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L19-L19)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

25: contract FaultDisputeGame is IFaultDisputeGame, Clone, ISemver {

```


*GitHub* : [25](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L25-L25)

### [G-48]<a name="g-48"></a> WETH address definition can be use directly

WETH is a wrap Ether contract with a specific address in the Ethereum network, giving the option to define it may cause false recognition, it is healthier to define it directly.

    Advantages of defining a specific contract directly:
    
    It saves gas,
    Prevents incorrect argument definition,
    Prevents execution on a different chain and re-signature issues,
    WETH Address : 0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2

*There are 2 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

51:     IDelayedWETH internal immutable WETH;

133:         IDelayedWETH _weth,

```


*GitHub* : [51](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L51-L51), [133](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L133-L133)

### [G-49]<a name="g-49"></a> Using XOR (^) and AND (&) bitwise equivalents for gas optimizations

Given 4 variables a, b, c and d represented as such:

```

0 0 0 0 0 1 1 0 <- a
0 1 1 0 0 1 1 0 <- b
0 0 0 0 0 0 0 0 <- c
1 1 1 1 1 1 1 1 <- d

```

To have a == b means that every 0 and 1 match on both variables. Meaning that a XOR (operator ^) would evaluate to 0 ((a ^ b) == 0), as it excludes by definition any equalities.Now, if a != b, this means that there’s at least somewhere a 1 and a 0 not matching between a and b, making (a ^ b) != 0.Both formulas are logically equivalent and using the XOR bitwise operator costs actually the same amount of gas.However, it is much cheaper to use the bitwise OR operator (|) than comparing the truthy or falsy values.These are logically equivalent too, as the OR bitwise operator (|) would result in a 1 somewhere if any value is not 0 between the XOR (^) statements, meaning if any XOR (^) statement verifies that its arguments are different.

*There are 136 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/MIPS.sol

170:             if (syscall_no == 4090) {

176:                 if (a0 == 0) {

184:             else if (syscall_no == 4045) {

188:             else if (syscall_no == 4120) {

192:             else if (syscall_no == 4246) {

198:             else if (syscall_no == 4003) {

201:                 if (a0 == FD_STDIN) {

205:                 else if (a0 == FD_PREIMAGE_READ) {

210:                     if (uint8(preimageKey[0]) == 1) {

238:                 else if (a0 == FD_HINT_READ) {

248:             else if (syscall_no == 4004) {

251:                 if (a0 == FD_STDOUT || a0 == FD_STDERR || a0 == FD_HINT_WRITE) {

255:                 else if (a0 == FD_PREIMAGE_WRITE) {

282:             else if (syscall_no == 4055) {

285:                 if (a1 == 3) {

287:                     if (a0 == FD_STDIN || a0 == FD_PREIMAGE_READ || a0 == FD_HINT_READ) {

289:                     } else if (a0 == FD_STDOUT || a0 == FD_STDERR || a0 == FD_PREIMAGE_WRITE || a0 == FD_HINT_WRITE) {

334:             if (_opcode == 4 || _opcode == 5) {

336:                 shouldBranch = (_rs == rt && _opcode == 4) || (_rs != rt && _opcode == 5);

339:             else if (_opcode == 6) {

343:             else if (_opcode == 7) {

347:             else if (_opcode == 1) {

350:                 if (rtv == 0) {

353:                 if (rtv == 1) {

394:             if (_func == 0x10) {

398:             else if (_func == 0x11) {

402:             else if (_func == 0x12) {

406:             else if (_func == 0x13) {

410:             else if (_func == 0x18) {

416:             else if (_func == 0x19) {

424:             else if (_func == 0x1a) {

425:                 if (int32(_rt) == 0) {

434:             else if (_func == 0x1b) {

435:                 if (_rt == 0) {

704:             if (opcode == 2 || opcode == 3) {

707:                 return handleJump(opcode == 2 ? 0 : 31, target);

719:             if (opcode == 0 || opcode == 0x1c) {

726:                 if (opcode == 0xC || opcode == 0xD || opcode == 0xe) {

733:             } else if (opcode >= 0x28 || opcode == 0x22 || opcode == 0x26) {

741:             if ((opcode >= 4 && opcode < 8) || opcode == 1) {

766:             if (opcode == 0 && func >= 8 && func < 0x1c) {

767:                 if (func == 8 || func == 9) {

769:                     return handleJump(func == 8 ? 0 : rdReg, rs);

772:                 if (func == 0xa) {

774:                     return handleRd(rdReg, rs, rt == 0);

776:                 if (func == 0xb) {

782:                 if (func == 0xC) {

794:             if (opcode == 0x38 && rtReg != 0) {

813:             if (opcode == 0 || (opcode >= 8 && opcode < 0xF)) {

835:                 if (func == 0x00) {

839:                 else if (func == 0x02) {

843:                 else if (func == 0x03) {

848:                 else if (func == 0x04) {

852:                 else if (func == 0x6) {

856:                 else if (func == 0x07) {

862:                 else if (func == 0x08) {

866:                 else if (func == 0x09) {

870:                 else if (func == 0x0a) {

874:                 else if (func == 0x0b) {

878:                 else if (func == 0x0c) {

883:                 else if (func == 0x0f) {

887:                 else if (func == 0x10) {

891:                 else if (func == 0x11) {

895:                 else if (func == 0x12) {

899:                 else if (func == 0x13) {

903:                 else if (func == 0x18) {

907:                 else if (func == 0x19) {

911:                 else if (func == 0x1a) {

915:                 else if (func == 0x1b) {

920:                 else if (func == 0x20) {

924:                 else if (func == 0x21) {

928:                 else if (func == 0x22) {

932:                 else if (func == 0x23) {

936:                 else if (func == 0x24) {

940:                 else if (func == 0x25) {

944:                 else if (func == 0x26) {

948:                 else if (func == 0x27) {

952:                 else if (func == 0x2a) {

956:                 else if (func == 0x2b) {

963:                 if (opcode == 0x1C) {

966:                     if (func == 0x2) {

970:                     else if (func == 0x20 || func == 0x21) {

971:                         if (func == 0x20) {

983:                 else if (opcode == 0x0F) {

987:                 else if (opcode == 0x20) {

991:                 else if (opcode == 0x21) {

995:                 else if (opcode == 0x22) {

1001:                 else if (opcode == 0x23) {

1005:                 else if (opcode == 0x24) {

1009:                 else if (opcode == 0x25) {

1013:                 else if (opcode == 0x26) {

1019:                 else if (opcode == 0x28) {

1025:                 else if (opcode == 0x29) {

1031:                 else if (opcode == 0x2a) {

1037:                 else if (opcode == 0x2b) {

1041:                 else if (opcode == 0x2e) {

1047:                 else if (opcode == 0x30) {

1051:                 else if (opcode == 0x38) {

```


*GitHub* : [170](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L170-L170), [176](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L176-L176), [184](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L184-L184), [188](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L188-L188), [192](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L192-L192), [198](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L198-L198), [201](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L201-L201), [205](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L205-L205), [210](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L210-L210), [238](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L238-L238), [248](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L248-L248), [251](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L251-L251), [255](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L255-L255), [282](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L282-L282), [285](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L285-L285), [287](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L287-L287), [289](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L289-L289), [334](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L334-L334), [336](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L336-L336), [339](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L339-L339), [343](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L343-L343), [347](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L347-L347), [350](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L350-L350), [353](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L353-L353), [394](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L394-L394), [398](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L398-L398), [402](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L402-L402), [406](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L406-L406), [410](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L410-L410), [416](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L416-L416), [424](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L424-L424), [425](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L425-L425), [434](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L434-L434), [435](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L435-L435), [704](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L704-L704), [707](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L707-L707), [719](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L719-L719), [726](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L726-L726), [733](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L733-L733), [741](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L741-L741), [766](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L766-L766), [767](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L767-L767), [769](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L769-L769), [772](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L772-L772), [774](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L774-L774), [776](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L776-L776), [782](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L782-L782), [794](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L794-L794), [813](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L813-L813), [835](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L835-L835), [839](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L839-L839), [843](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L843-L843), [848](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L848-L848), [852](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L852-L852), [856](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L856-L856), [862](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L862-L862), [866](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L866-L866), [870](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L870-L870), [874](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L874-L874), [878](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L878-L878), [883](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L883-L883), [887](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L887-L887), [891](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L891-L891), [895](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L895-L895), [899](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L899-L899), [903](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L903-L903), [907](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L907-L907), [911](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L911-L911), [915](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L915-L915), [920](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L920-L920), [924](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L924-L924), [928](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L928-L928), [932](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L932-L932), [936](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L936-L936), [940](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L940-L940), [944](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L944-L944), [948](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L948-L948), [952](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L952-L952), [956](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L956-L956), [963](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L963-L963), [966](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L966-L966), [970](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L970-L970), [971](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L971-L971), [983](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L983-L983), [987](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L987-L987), [991](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L991-L991), [995](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L995-L995), [1001](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1001-L1001), [1005](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1005-L1005), [1009](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1009-L1009), [1013](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1013-L1013), [1019](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1019-L1019), [1025](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1025-L1025), [1031](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1031-L1031), [1037](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1037-L1037), [1041](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1041-L1041), [1047](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1047-L1047), [1051](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1051-L1051)

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

468:         if (metaData.claimedSize() == 0) revert NotInitialized();

599:         if (keccak256(abi.encode(_stateMatrix)) == _postState.stateCommitment) revert PostStateMatches();

630:         if (keccak256(abi.encode(stateMatrix)) == _postState.stateCommitment) revert PostStateMatches();

699:             if ((size & 1) == 1) {

727:         if (offset < 8 && currentSize == 0) {

```


*GitHub* : [468](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L468-L468), [599](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L599-L599), [630](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L630-L630), [699](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L699-L699), [727](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L727-L727)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol

97:         if (address(impl) == address(0)) revert NoImplementation(_gameType);

158:         if (_start >= _disputeGameList.length || _n == 0) return games_;

172:             if (gameType.raw() == _gameType.raw()) {

```


*GitHub* : [97](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L97-L97), [158](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L158-L158), [172](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L172-L172)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

176:         if (root.raw() == bytes32(0)) revert AnchorRootNotFound();

269:             preStateClaim = (stepPos.indexAtDepth() % (1 << (MAX_GAME_DEPTH - SPLIT_DEPTH))) == 0

302:         bool validStep = VM.step(_stateData, _proof, uuid.raw()) == postState.claim.raw();

303:         bool parentPostAgree = (parentPos.depth() - postState.position.depth()) % 2 == 0;

304:         if (parentPostAgree == validStep) revert ValidStep();

339:         if ((_challengeIndex == 0 || nextPositionDepth == SPLIT_DEPTH + 2) && !_isAttack) {

345:         if (l2BlockNumberChallenged && _challengeIndex == 0) revert L2BlockNumberChallenged();

355:         if (nextPositionDepth == SPLIT_DEPTH + 1) {

369:         if (nextDuration.raw() == MAX_CLOCK_DURATION.raw()) revert ClockTimeExceeded();

380:                 nextPositionDepth == SPLIT_DEPTH - 1 ? CLOCK_EXTENSION.raw() * 2 : CLOCK_EXTENSION.raw();

438:         if (_ident == LocalPreimageKey.L1_HEAD_HASH) {

441:         } else if (_ident == LocalPreimageKey.STARTING_OUTPUT_ROOT) {

444:         } else if (_ident == LocalPreimageKey.DISPUTED_OUTPUT_ROOT) {

447:         } else if (_ident == LocalPreimageKey.DISPUTED_L2_BLOCK_NUMBER) {

456:         } else if (_ident == LocalPreimageKey.CHAIN_ID) {

528:         if (blockNumber == l2BlockNumber()) revert BlockNumberMatches();

549:         status_ = claimData[0].counteredBy == address(0) ? GameStatus.DEFENDER_WINS : GameStatus.CHALLENGER_WINS;

580:         if (challengeIndicesLen == 0 && _claimIndex != 0) {

586:             address recipient = counteredBy == address(0) ? subgameRootClaim.claimant : counteredBy;

601:             if (_numToResolve == 0) _numToResolve = challengeIndicesLen;

621:             if (claim.counteredBy == address(0) && checkpoint.leftmostPosition.raw() > claim.position.raw()) {

636:         if (checkpoint.subgameIndex == challengeIndicesLen) {

643:             if (_claimIndex == 0 && l2BlockNumberChallenged) {

652:                 _distributeBond(countered == address(0) ? subgameRootClaim.claimant : countered, subgameRootClaim);

753:         if (recipientCredit == 0) revert NoCreditToClaim();

883:         if (_isAttack || disputed.position.depth() % 2 == SPLIT_DEPTH % 2) {

888:             if (!(vmStatus == VMStatuses.INVALID.raw() || vmStatus == VMStatuses.PANIC.raw())) {

957:             if (currentDepth == SPLIT_DEPTH + 1) execRootClaim = claim;

967:         bool wasAttack = execRootPos.parent().raw() == outputPos.raw();

1019:         uuid_ = _startingPos.raw() == 0

```


*GitHub* : [176](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L176-L176), [269](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L269-L269), [302](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L302-L302), [303](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L303-L303), [304](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L304-L304), [339](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L339-L339), [345](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L345-L345), [355](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L355-L355), [369](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L369-L369), [380](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L380-L380), [438](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L438-L438), [441](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L441-L441), [444](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L444-L444), [447](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L447-L447), [456](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L456-L456), [528](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L528-L528), [549](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L549-L549), [580](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L580-L580), [586](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L586-L586), [601](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L601-L601), [621](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L621-L621), [636](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L636-L636), [643](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L643-L643), [652](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L652-L652), [753](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L753-L753), [883](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L883-L883), [888](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L888-L888), [957](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L957-L957), [967](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L967-L967), [1019](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L1019-L1019)
