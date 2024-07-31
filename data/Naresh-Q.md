## Summary

| |Issue|Instances| 
|-|:-|:-:|
| [[L-01](#l-01)] | Consider adding validation of user inputs | 18| 
| [[L-02](#l-02)] | Array is `push()`ed but not `pop()`ed | 4| 
| [[L-03](#l-03)] | Consider disabling `renounceOwnership()` | 1| 
| [[L-04](#l-04)] | Events may be emitted out of order due to reentrancy | 2| 
| [[L-05](#l-05)] | Functions calling contracts/addresses with transfer hooks are missing reentrancy guards | 1| 
| [[L-06](#l-06)] | Gas griefing/theft is possible on an unsafe external call | 1| 
| [[L-07](#l-07)] | Loss of precision on division | 1| 
| [[L-08](#l-08)] | Mapping arrays can grow in size without a way to shrink them | 2| 
| [[L-09](#l-09)] | Missing zero address check in constructor | 2| 
| [[L-10](#l-10)] | Missing contract-existence checks before low-level calls | 2| 
| [[L-11](#l-11)] | `onlyOwner` functions not accessible if owner renounces ownership | 2| 
| [[L-12](#l-12)] | State variables not capped at reasonable values | 3| 
| [[L-13](#l-13)] | Using zero as a parameter | 2| 

### Low Risk Issues

### [L-01]<a name="l-01"></a> Consider adding validation of user inputs

There are no validations done on the arguments below. Consider that the Solidity [documentation](https://docs.soliditylang.org/en/latest/control-structures.html#panic-via-assert-and-error-via-require) states that `Properly functioning code should never create a Panic, not even on invalid external input. If this happens, then there is a bug in your contract which you should fix`. This means that there should be explicit checks for expected ranges of inputs. Underflows/overflows result in panics should not be used as range checks, and allowing funds to be sent to `0x0`, which is the default value of address variables and has many gotchas, should be avoided.

*There are 18 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

// @audit missing checks for -->  _precompile
335:     function loadPrecompilePreimagePart(uint256 _partOffset, address _precompile, bytes calldata _input) external {

// @audit missing checks for -->  _claimant
402:     function proposalBlocksLen(address _claimant, uint256 _uuid) external view returns (uint256 len_) {

// @audit missing checks for -->  _claimant
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

// @audit missing checks for -->  _claimant
609:     function challengeFirstLPP(
610:         address _claimant,
611:         uint256 _uuid,
612:         Leaf calldata _postState,
613:         bytes32[] calldata _postStateProof
614:     )
615:         external
616:     {

// @audit missing checks for -->  _claimant
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

// @audit missing checks for -->  _owner
696:     function getTreeRootLPP(address _owner, uint256 _uuid) public view returns (bytes32 treeRoot_) {

```


*GitHub* : [335](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L335-L335), [402](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L402-L402), [568](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L568-L578), [609](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L609-L616), [640](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L640-L650), [696](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L696-L696)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol

// @audit missing checks for -->  _owner
48:     function initialize(address _owner) public initializer {

// @audit missing checks for -->  _gameType, _rootClaim
59:     function games(
60:         GameType _gameType,
61:         Claim _rootClaim,
62:         bytes calldata _extraData
63:     )
64:         external
65:         view
66:         returns (IDisputeGame proxy_, Timestamp timestamp_)
67:     {

// @audit missing checks for -->  _gameType, _rootClaim
84:     function create(
85:         GameType _gameType,
86:         Claim _rootClaim,
87:         bytes calldata _extraData
88:     )
89:         external
90:         payable
91:         returns (IDisputeGame proxy_)
92:     {

// @audit missing checks for -->  _gameType, _rootClaim
135:     function getGameUUID(
136:         GameType _gameType,
137:         Claim _rootClaim,
138:         bytes calldata _extraData
139:     )
140:         public
141:         pure
142:         returns (Hash uuid_)
143:     {

// @audit missing checks for -->  _gameType
148:     function findLatestGames(
149:         GameType _gameType,
150:         uint256 _start,
151:         uint256 _n
152:     )
153:         external
154:         view
155:         returns (GameSearchResult[] memory games_)
156:     {

// @audit missing checks for -->  _gameType, _impl
199:     function setImplementation(GameType _gameType, IDisputeGame _impl) external onlyOwner {

// @audit missing checks for -->  _gameType
205:     function setInitBond(GameType _gameType, uint256 _initBond) external onlyOwner {

```


*GitHub* : [48](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L48-L48), [59](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L59-L67), [84](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L84-L92), [135](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L135-L143), [148](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L148-L156), [199](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L199-L199), [205](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L205-L205)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

// @audit missing checks for -->  _disputed, _claim
319:     function move(Claim _disputed, uint256 _challengeIndex, Claim _claim, bool _isAttack) public payable virtual {

// @audit missing checks for -->  _disputed, _claim
419:     function attack(Claim _disputed, uint256 _parentIndex, Claim _claim) external payable {

// @audit missing checks for -->  _disputed, _claim
424:     function defend(Claim _disputed, uint256 _parentIndex, Claim _claim) external payable {

// @audit missing checks for -->  _position
702:     function getRequiredBond(Position _position) public view returns (uint256 requiredBond_) {

// @audit missing checks for -->  _recipient
747:     function claimCredit(address _recipient) external {

```


*GitHub* : [319](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L319-L319), [419](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L419-L419), [424](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L424-L424), [702](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L702-L702), [747](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L747-L747)

### [L-02]<a name="l-02"></a> Array is `push()`ed but not `pop()`ed

Array entries are added but are never removed. Consider whether this should be the case, or whether there should be a maximum, or whether old entries should be removed. Cases where there are specific potential problems will be flagged separately under a different issue.

*There are 4 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

433:         proposals.push(LargePreimageProposalKeys(msg.sender, _uuid));

```


*GitHub* : [433](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L433-L433)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol

130:         _disputeGameList.push(id);

```


*GitHub* : [130](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L130-L130)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

207:         claimData.push(

395:         claimData.push(

```


*GitHub* : [207](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L207-L207), [395](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L395-L395)

### [L-03]<a name="l-03"></a> Consider disabling `renounceOwnership()`

Typically, the contract's owner is the account that deploys the contract. As a result, the owner is able to perform certain privileged activities. The OpenZeppelin's `Ownable` is used in this project contract implements `renounceOwnership`. This can represent a certain risk if the ownership is renounced for any other reason than by design. Renouncing ownership will leave the contract without an owner, thereby removing any functionality that is only available to the owner.

*There are 1 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol

19: contract DisputeGameFactory is OwnableUpgradeable, IDisputeGameFactory, ISemver {

```


*GitHub* : [19](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L19-L19)

### [L-04]<a name="l-04"></a> Events may be emitted out of order due to reentrancy

If a reentrancy occurs, some events may be emitted in an unexpected order, and this may be a problem if a third party expects a specific order for these events. Ensure that events are emitted before external calls and follow the best practice of CEI.

*There are 2 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol

131:         emit DisputeGameCreated(address(proxy_), _gameType, _rootClaim);

```


*GitHub* : [131](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L131-L131)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

415:         emit Move(_challengeIndex, _claim, msg.sender);

```


*GitHub* : [415](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L415-L415)

### [L-05]<a name="l-05"></a> Functions calling contracts/addresses with transfer hooks are missing reentrancy guards

Even if the function follows the best practice of check-effects-interaction, not using a reentrancy guard when there may be transfer hooks will open the users of this protocol up to [read-only reentrancies](https://chainsecurity.com/curve-lp-oracle-manipulation-post-mortem/) with no way to protect against it, except by block-listing the whole protocol.

*There are 1 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

// @audit function 'claimCredit()' is missing Reentrancy guard
756:         WETH.withdraw(_recipient, recipientCredit);

```


*GitHub* : [756](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L756-L756)

### [L-06]<a name="l-06"></a> Gas griefing/theft is possible on an unsafe external call

A low-level call will copy any amount of bytes to local memory. When bytes are copied from returndata to memory, the memory expansion cost is paid.This means that when using a standard solidity call, the callee can 'returnbomb' the caller, imposing an arbitrary gas cost.Because this gas is paid by the caller and in the caller's context, it can cause the caller to run out of gas and halt execution.Consider replacing all unsafe `call` with `excessivelySafeCall` from this [repository](https://github.com/nomad-xyz/ExcessivelySafeCall).

*There are 1 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

792:         (bool success,) = _to.call{ value: bond }("");

```


*GitHub* : [792](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L792-L792)

### [L-07]<a name="l-07"></a> Loss of precision on division

Solidity doesn't support fractions, so divisions by large numbers could result in the quotient being zero. To avoid this, it's recommended to require a minimum numerator amount to ensure that it is always greater than the denominator.

*There are 1 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/MIPS.sol

439:                 state.lo = _rs / _rt;

```


*GitHub* : [439](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L439-L439)

### [L-08]<a name="l-08"></a> Mapping arrays can grow in size without a way to shrink them

It's a good practice to maintain control over the size of array mappings in Solidity, especially if they are dynamically updated. If a contract includes a mechanism to push items into an array, it should ideally also provide a mechanism to remove items. This is because Solidity arrays don't automatically shrink when items are deleted - their length needs to be manually adjusted.

Ignoring this can lead to bloated and inefficient contracts. For instance, iterating over a large array can cause your contract to hit the block gas limit. Additionally, if entries are only marked for deletion but never actually removed, you may end up dealing with stale or irrelevant data, which can cause logical errors.

Therefore, implementing a method to 'pop' items from mapping arrays helps manage contract's state, improve efficiency and prevent potential issues related to gas limits or stale data. Always ensure to handle potential underflow conditions when popping elements from the mapping array.

*There are 2 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

83:     mapping(address => mapping(uint256 => uint64[])) public proposalBlocks;

```


*GitHub* : [83](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L83-L83)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

104:     mapping(uint256 => uint256[]) public subgames;

```


*GitHub* : [104](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L104-L104)

### [L-09]<a name="l-09"></a> Missing zero address check in constructor

Constructors often take address parameters to initialize important components of a contract, such as owner or linked contracts. However, without a checking, there's a risk that an address parameter could be mistakenly set to the zero address `(0x0)`. This could be due to an error or oversight during contract deployment. A zero address in a crucial role can cause serious issues, as it cannot perform actions like a normal address, and any funds sent to it will be irretrievable. It's therefore crucial to include a zero address check in constructors to prevent such potential problems. If a zero address is detected, the constructor should revert the transaction.

*There are 2 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/MIPS.sol

// @audit missing checks for -->  _oracle
64:     constructor(IPreimageOracle _oracle) {

```


*GitHub* : [64](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L64-L64)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

// @audit missing checks for -->  _gameType, _absolutePrestate, _clockExtension, _maxClockDuration, _vm, _weth, _anchorStateRegistry
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

### [L-10]<a name="l-10"></a> Missing contract-existence checks before low-level calls

Low-level calls return success if there is no code present at the specified address.

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

### [L-11]<a name="l-11"></a> `onlyOwner` functions not accessible if owner renounces ownership

The owner is able to perform certain privileged activities, but it's possible to set the owner to address(0). This can represent a certain risk if the ownership is renounced for any other reason than by design.

Renouncing ownership will leave the contract without an owner, therefore limiting any functionality that needs authority.

*There are 2 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol

199:     function setImplementation(GameType _gameType, IDisputeGame _impl) external onlyOwner {

205:     function setInitBond(GameType _gameType, uint256 _initBond) external onlyOwner {

```


*GitHub* : [199](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L199-L199), [205](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L205-L205)

### [L-12]<a name="l-12"></a> State variables not capped at reasonable values

Consider adding minimum/maximum value checks to ensure that the state variables below can never be used to excessively harm users, including via griefing

*There are 3 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/PreimageOracle.sol

// @audit _minProposalSize
90:         MIN_LPP_SIZE_BYTES = _minProposalSize;

// @audit _challengePeriod
91:         CHALLENGE_PERIOD = _challengePeriod;

```


*GitHub* : [90](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L90-L90), [91](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L91-L91)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol

// @audit _l2ChainId
153:         L2_CHAIN_ID = _l2ChainId;

```


*GitHub* : [153](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L153-L153)

### [L-13]<a name="l-13"></a> Using zero as a parameter

Passing `0` or `0x0` as a function argument can sometimes result in a security issue(e.g. passing zero as the slippage parameter). A historical example is the infamous 0x0 address bug where numerous tokens were lost. This happens because `0` can be interpreted as an uninitialized address, leading to transfers to the `0x0` address, effectively burning tokens. Moreover, `0` as a denominator in division operations would cause a runtime exception. It's also often indicative of a logical error in the caller's code.

Consider using a constant variable with a descriptive name, so it's clear that the argument is intentionally being used, and for the right reasons.

*There are 2 instance(s) of this issue:*

```solidity
📁 File: packages/contracts-bedrock/src/cannon/MIPS.sol

700:             uint32 insn = readMem(state.pc, 0);

```


*GitHub* : [700](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L700-L700)

```solidity
📁 File: packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol

43:         initialize(address(0));

```


*GitHub* : [43](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L43-L43)

