# QA Report

## Summary

### Low Issues

Total **35 instances** over **8 issues**:

|ID|Issue|Instances|
|:--:|:---|:--:|
| [[L-01]](#l-01-variables-shadowing-other-definitions) | Variables shadowing other definitions | 1 |
| [[L-02]](#l-02-missing-zero-address-check-in-initializer) | Missing zero address check in initializer | 1 |
| [[L-03]](#l-03-array-is-pushed-but-not-poped) | Array is `push()`ed but not `pop()`ed | 6 |
| [[L-04]](#l-04-vulnerable-versions-of-packages-are-being-used) | Vulnerable versions of packages are being used | 4 |
| [[L-05]](#l-05-constructor--initialization-function-lacks-parameter-validation) | Constructor / initialization function lacks parameter validation | 5 |
| [[L-06]](#l-06-unsafe-solidity-low-level-call-can-cause-gas-grief-attack) | Unsafe solidity low-level call can cause gas grief attack | 2 |
| [[L-07]](#l-07-critical-functions-should-be-controlled-by-time-locks) | Critical functions should be controlled by time locks | 1 |
| [[L-08]](#l-08-code-does-not-follow-the-best-practice-of-check-effects-interaction) | Code does not follow the best practice of check-effects-interaction | 15 |

### Non Critical Issues

Total **551 instances** over **58 issues**:

|ID|Issue|Instances|
|:--:|:---|:--:|
| [[N-01]](#n-01-import-declarations-should-import-specific-identifiers-rather-than-the-whole-file) | Import declarations should import specific identifiers, rather than the whole file | 6 |
| [[N-02]](#n-02-names-of-constants-should-use-the-upper_case_with_underscores-style) | Names of constants should use the UPPER_CASE_WITH_UNDERSCORES style | 4 |
| [[N-03]](#n-03-names-of-privateinternal-functions-should-be-prefixed-with-an-underscore) | Names of `private`/`internal` functions should be prefixed with an underscore | 26 |
| [[N-04]](#n-04-names-of-privateinternal-state-variables-should-be-prefixed-with-an-underscore) | Names of `private`/`internal` state variables should be prefixed with an underscore | 25 |
| [[N-05]](#n-05-use-of-override-is-unnecessary) | Use of `override` is unnecessary | 1 |
| [[N-06]](#n-06-consider-providing-a-ranged-getter-for-array-state-variables) | Consider providing a ranged getter for array state variables | 6 |
| [[N-07]](#n-07-assembly-blocks-should-have-extensive-comments) | Assembly blocks should have extensive comments | 30 |
| [[N-08]](#n-08-consider-splitting-complex-checks-into-multiple-steps) | Consider splitting complex checks into multiple steps | 1 |
| [[N-09]](#n-09-complex-casting) | Complex casting | 5 |
| [[N-10]](#n-10-complex-math-should-be-split-into-multiple-steps) | Complex math should be split into multiple steps | 12 |
| [[N-11]](#n-11-consider-adding-a-blockdeny-list) | Consider adding a block/deny-list | 4 |
| [[N-12]](#n-12-constantsimmutables-redefined-elsewhere) | Constants/Immutables redefined elsewhere | 4 |
| [[N-13]](#n-13-convert-simple-if-statements-to-ternary-expressions) | Convert simple `if`-statements to ternary expressions | 4 |
| [[N-14]](#n-14-contract-name-does-not-match-its-filename) | Contract name does not match its filename | 1 |
| [[N-15]](#n-15-duplicate-imports) | Duplicate imports | 1 |
| [[N-16]](#n-16-consider-emitting-an-event-at-the-end-of-the-constructor) | Consider emitting an event at the end of the constructor | 4 |
| [[N-17]](#n-17-events-are-emitted-without-the-sender-information) | Events are emitted without the sender information | 2 |
| [[N-18]](#n-18-imports-could-be-organized-more-systematically) | Imports could be organized more systematically | 3 |
| [[N-19]](#n-19-there-is-no-need-to-initialize-bool-variables-with-false) | There is no need to initialize bool variables with `false` | 1 |
| [[N-20]](#n-20-openzeppelincontracts-should-be-upgraded-to-a-newer-version) | @openzeppelin/contracts should be upgraded to a newer version | 1 |
| [[N-21]](#n-21-safe-contracts-should-be-upgraded-to-a-newer-version) | safe-contracts should be upgraded to a newer version | 1 |
| [[N-22]](#n-22-lib-solady-should-be-upgraded-to-a-newer-version) | Lib solady should be upgraded to a newer version | 1 |
| [[N-23]](#n-23-expressions-for-constant-values-should-use-immutable-rather-than-constant) | Expressions for constant values should use `immutable` rather than `constant` | 1 |
| [[N-24]](#n-24-consider-moving-duplicated-strings-to-constants) | Consider moving duplicated strings to constants | 5 |
| [[N-25]](#n-25-memory-safe-annotation-preferred-over-comment-variant) | Memory-safe annotation preferred over comment variant | 1 |
| [[N-26]](#n-26-contract-uses-both-requirerevert-as-well-as-custom-errors) | Contract uses both `require()`/`revert()` as well as custom errors | 1 |
| [[N-27]](#n-27-functions-should-be-named-in-mixedcase-style) | Functions should be named in mixedCase style | 8 |
| [[N-28]](#n-28-constants-should-be-put-on-the-left-side-of-comparisons) | Constants should be put on the left side of comparisons | 143 |
| [[N-29]](#n-29-else-block-not-required) | `else`-block not required | 47 |
| [[N-30]](#n-30-large-multiples-of-ten-should-use-scientific-notation) | Large multiples of ten should use scientific notation | 2 |
| [[N-31]](#n-31-high-cyclomatic-complexity) | High cyclomatic complexity | 2 |
| [[N-32]](#n-32-typos) | Typos | 8 |
| [[N-33]](#n-33-unnecessary-casting) | Unnecessary casting | 1 |
| [[N-34]](#n-34-unused-contract-variables) | Unused contract variables | 4 |
| [[N-35]](#n-35-unused-named-return) | Unused named return | 2 |
| [[N-36]](#n-36-consider-using-delete-rather-than-assigning-zero-to-clear-values) | Consider using `delete` rather than assigning zero to clear values | 5 |
| [[N-37]](#n-37-use-the-latest-solidity-version) | Use the latest Solidity version | 6 |
| [[N-38]](#n-38-consider-using-named-function-arguments) | Consider using named function arguments | 6 |
| [[N-39]](#n-39-named-returns-are-recommended) | Named returns are recommended | 1 |
| [[N-40]](#n-40-use-transfer-libraries-instead-of-low-level-calls-to-transfer-native-tokens) | Use transfer libraries instead of low level calls to transfer native tokens | 2 |
| [[N-41]](#n-41-using-underscore-at-the-end-of-variable-name) | Using underscore at the end of variable name | 76 |
| [[N-42]](#n-42-use-a-struct-to-encapsulate-multiple-function-parameters) | Use a struct to encapsulate multiple function parameters | 6 |
| [[N-43]](#n-43-returning-a-struct-instead-of-a-bunch-of-variables-is-better) | Returning a struct instead of a bunch of variables is better | 5 |
| [[N-44]](#n-44-contract-variables-should-have-comments) | Contract variables should have comments | 11 |
| [[N-45]](#n-45-missing-event-when-a-state-variables-is-set-in-constructor) | Missing event when a state variables is set in constructor | 1 |
| [[N-46]](#n-46-empty-bytes-check-is-missing) | Empty bytes check is missing | 12 |
| [[N-47]](#n-47-assembly-block-creates-dirty-bits) | Assembly block creates dirty bits | 6 |
| [[N-48]](#n-48-multiple-mappings-with-same-keys-can-be-combined-into-a-single-struct-mapping-for-readability) | Multiple mappings with same keys can be combined into a single struct mapping for readability | 13 |
| [[N-49]](#n-49-missing-event-for-critical-changes) | Missing event for critical changes | 1 |
| [[N-50]](#n-50-non-assembly-method-available) | Non-assembly method available | 3 |
| [[N-51]](#n-51-consider-adding-emergency-stop-functionality) | Consider adding emergency-stop functionality | 1 |
| [[N-52]](#n-52-avoid-the-use-of-sensitive-terms) | Avoid the use of sensitive terms | 2 |
| [[N-53]](#n-53-missing-checks-for-uint-state-variable-assignments) | Missing checks for uint state variable assignments | 2 |
| [[N-54]](#n-54-use-the-modern-upgradeable-contract-paradigm) | Use the Modern Upgradeable Contract Paradigm | 2 |
| [[N-55]](#n-55-large-or-complicated-code-bases-should-implement-invariant-tests) | Large or complicated code bases should implement invariant tests | 1 |
| [[N-56]](#n-56-the-default-value-is-manually-set-when-it-is-declared) | The default value is manually set when it is declared | 7 |
| [[N-57]](#n-57-contracts-should-have-all-publicexternal-functions-exposed-by-interfaces) | Contracts should have all `public`/`external` functions exposed by `interface`s | 4 |
| [[N-58]](#n-58-numeric-values-having-to-do-with-time-should-use-time-units-for-readability) | Numeric values having to do with time should use time units for readability | 10 |

## Low Issues

### [L-01] Variables shadowing other definitions

A variable declaration shadowing any other existing definition is error-prone. It can cause confusion for developers, potentially introduce bugs, and reduce the readability and maintainability.

There is 1 instance:

- *DisputeGameFactory.sol* ( [48-48](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L48-L48) ):

```solidity
/// @audit Shadows `state variable _owner`
48:     function initialize(address _owner) public initializer {
```

### [L-02] Missing zero address check in initializer

Consider adding a zero address check for each address type parameter in initializer.

There is 1 instance:

- *DisputeGameFactory.sol* ( [48-48](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L48-L48) ):

```solidity
/// @audit `_owner not checked`
48:     function initialize(address _owner) public initializer {
```

### [L-03] Array is `push()`ed but not `pop()`ed

Array entries are added but are never removed. Consider whether this should be the case, or whether there should be a maximum, or whether old entries should be removed. Cases where there are specific potential problems will be flagged separately under a different issue.

<details>
<summary>There are 6 instances (click to show):</summary>

- *PreimageOracle.sol* ( [433-433](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L433-L433), [554-554](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L554-L554) ):

```solidity
433:         proposals.push(LargePreimageProposalKeys(msg.sender, _uuid));

554:         proposalBlocks[msg.sender][_uuid].push(uint64(block.number));
```

- *DisputeGameFactory.sol* ( [130-130](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L130-L130) ):

```solidity
130:         _disputeGameList.push(id);
```

- *FaultDisputeGame.sol* ( [207-217](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L207-L217), [395-406](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L395-L406), [409-409](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L409-L409) ):

```solidity
207:         claimData.push(
208:             ClaimData({
209:                 parentIndex: type(uint32).max,
210:                 counteredBy: address(0),
211:                 claimant: gameCreator(),
212:                 bond: uint128(msg.value),
213:                 claim: rootClaim(),
214:                 position: ROOT_POSITION,
215:                 clock: LibClock.wrap(Duration.wrap(0), Timestamp.wrap(uint64(block.timestamp)))
216:             })
217:         );

395:         claimData.push(
396:             ClaimData({
397:                 parentIndex: uint32(_challengeIndex),
398:                 // This is updated during subgame resolution
399:                 counteredBy: address(0),
400:                 claimant: msg.sender,
401:                 bond: uint128(msg.value),
402:                 claim: _claim,
403:                 position: nextPosition,
404:                 clock: nextClock
405:             })
406:         );

409:         subgames[_challengeIndex].push(claimData.length - 1);
```

</details>

### [L-04] Vulnerable versions of packages are being used

This project is using specific package versions which are vulnerable to the specific CVEs listed below. Consider switching to more recent versions of these packages that don't have these vulnerabilities.
- [CVE-2023-40014](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2023-40014) - **MEDIUM** - (`openzeppelin-solidity >=4.0.0 <4.9.3`): OpenZeppelin Contracts is a library for secure smart contract development. Starting in version 4.0.0 and prior to version 4.9.3, contracts using `ERC2771Context` along with a custom trusted forwarder may see `_msgSender` return `address(0)` in calls that originate from the forwarder with calldata shorter than 20 bytes. This combination of circumstances does not appear to be common, in particular it is not the case for `MinimalForwarder` from OpenZeppelin Contracts, or any deployed forwarder the team is aware of, given that the signer address is appended to all calls that originate from these forwarders. The problem has been patched in v4.9.3.

- [CVE-2023-34459](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2023-34459) - **MEDIUM** - (`openzeppelin-solidity >=4.7.0 <4.9.2`): OpenZeppelin Contracts is a library for smart contract development. Starting in version 4.7.0 and prior to version 4.9.2, when the `verifyMultiProof`, `verifyMultiProofCalldata`, `procesprocessMultiProof`, or `processMultiProofCalldat` functions are in use, it is possible to construct merkle trees that allow forging a valid multiproof for an arbitrary set of leaves. A contract may be vulnerable if it uses multiproofs for verification and the merkle tree that is processed includes a node with value 0 at depth 1 (just under the root). This could happen inadvertedly for balanced trees with 3 leaves or less, if the leaves are not hashed. This could happen deliberately if a malicious tree builder includes such a node in the tree. A contract is not vulnerable if it uses single-leaf proving (`verify`, `verifyCalldata`, `processProof`, or `processProofCalldata`), or if it uses multiproofs with a known tree that has hashed leaves. Standard merkle trees produced or validated with the @openzeppelin/merkle-tree library are safe. The problem has been patched in version 4.9.2. Some workarounds are available. For those using multiproofs: When constructing merkle trees hash the leaves and do not insert empty nodes in your trees. Using the @openzeppelin/merkle-tree package eliminates this issue. Do not accept user-provided merkle roots without reconstructing at least the first level of the tree. Verify the merkle tree structure by reconstructing it from the leaves.

- [CVE-2023-30542](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2023-30542) - **HIGH** - (`openzeppelin-solidity >=4.7.0 <4.8.2`): OpenZeppelin Contracts is a library for secure smart contract development. The proposal creation entrypoint (`propose`) in `GovernorCompatibilityBravo` allows the creation of proposals with a `signatures` array shorter than the `calldatas` array. This causes the additional elements of the latter to be ignored, and if the proposal succeeds the corresponding actions would eventually execute without any calldata. The `ProposalCreated` event correctly represents what will eventually execute, but the proposal parameters as queried through `getActions` appear to respect the original intended calldata. This issue has been patched in 4.8.3. As a workaround, ensure that all proposals that pass through governance have equal length `signatures` and `calldatas` parameters.

- [CVE-2023-30541](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2023-30541) - **MEDIUM** - (`openzeppelin-solidity >=3.2.0 <4.8.3`): OpenZeppelin Contracts is a library for secure smart contract development. A function in the implementation contract may be inaccessible if its selector clashes with one of the proxy's own selectors. Specifically, if the clashing function has a different signature with incompatible ABI encoding, the proxy could revert while attempting to decode the arguments from calldata. The probability of an accidental clash is negligible, but one could be caused deliberately and could cause a reduction in availability. The issue has been fixed in version 4.8.3. As a workaround if a function appears to be inaccessible for this reason, it may be possible to craft the calldata such that ABI decoding does not fail at the proxy and the function is properly proxied through.

There are 4 instances:

- Global finding

### [L-05] Constructor / initialization function lacks parameter validation

Constructors and initialization functions play a critical role in contracts by setting important initial states when the contract is first deployed before the system starts. The parameters passed to the constructor and initialization functions directly affect the behavior of the contract / protocol. If incorrect parameters are provided, the system may fail to run, behave abnormally, be unstable, or lack security. Therefore, it's crucial to carefully check each parameter in the constructor and initialization functions. If an exception is found, the transaction should be rolled back.

There are 5 instances:

- *PreimageOracle.sol* ( [89-89](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L89-L89) ):

```solidity
/// @audit `_minProposalSize`
/// @audit `_challengePeriod`
89:     constructor(uint256 _minProposalSize, uint256 _challengePeriod) {
```

- *FaultDisputeGame.sol* ( [125-136](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L125-L136) ):

```solidity
/// @audit `_gameType`
/// @audit `_absolutePrestate`
/// @audit `_l2ChainId`
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

### [L-06] Unsafe solidity low-level call can cause gas grief attack

Using the low-level calls of a solidity address can leave the contract open to gas grief attacks. These attacks occur when the called contract returns a large amount of data.
So when calling an external contract, it is necessary to check the length of the return data before reading/copying it (using `returndatasize()`).

There are 2 instances:

- *PreimageOracle.sol* ( [792-792](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L792-L792) ):

```solidity
792:         (bool success,) = _to.call{ value: bond }("");
```

- *FaultDisputeGame.sol* ( [759-759](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L759-L759) ):

```solidity
759:         (bool success,) = _recipient.call{ value: recipientCredit }(hex"");
```

### [L-07] Critical functions should be controlled by time locks

It is a good practice to give time for users to react and adjust to critical changes. A timelock provides more guarantees and reduces the level of trust required, thus decreasing risk for users. It also indicates that the project is legitimate (less risk of a malicious owner making a sandwich attack on a user).

There is 1 instance:

- *DisputeGameFactory.sol* ( [199-199](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L199-L199) ):

```solidity
199:     function setImplementation(GameType _gameType, IDisputeGame _impl) external onlyOwner {
```

### [L-08] Code does not follow the best practice of check-effects-interaction

Code should follow the best-practice of [check-effects-interaction](https://blockchain-academy.hs-mittweida.de/courses/solidity-coding-beginners-to-intermediate/lessons/solidity-11-coding-patterns/topic/checks-effects-interactions/), where state variables are updated before any external calls are made. Doing so prevents a large class of reentrancy bugs.

<details>
<summary>There are 15 instances (click to show):</summary>

- *DisputeGameFactory.sol* ( [129-129](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L129-L129) ):

```solidity
/// @audit `initialize()` is called on line 117
129:         _disputeGames[uuid] = id;
```

- *FaultDisputeGame.sol* ( [226-226](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L226-L226), [311-311](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L311-L311), [588-588](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L588-L588), [597-597](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L597-L597), [598-598](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L598-L598), [601-601](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L601-L601), [607-607](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L607-L607), [622-622](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L622-L622), [623-623](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L623-L623), [628-628](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L628-L628), [632-632](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L632-L632), [640-640](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L640-L640), [648-648](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L648-L648), [656-656](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L656-L656) ):

```solidity
/// @audit `anchors()` is called on line 173
226:         createdAt = Timestamp.wrap(uint64(block.timestamp));

/// @audit `step()` is called on line 302
311:         parent.counteredBy = msg.sender;

/// @audit `_distributeBond()` is called on line 587, triggering an external call - `unlock()`
588:             resolvedSubgames[_claimIndex] = true;

/// @audit `_distributeBond()` is called on line 587, triggering an external call - `unlock()`
597:             checkpoint.leftmostPosition = Position.wrap(type(uint128).max);

/// @audit `_distributeBond()` is called on line 587, triggering an external call - `unlock()`
598:             checkpoint.initialCheckpointComplete = true;

/// @audit `_distributeBond()` is called on line 587, triggering an external call - `unlock()`
601:             if (_numToResolve == 0) _numToResolve = challengeIndicesLen;

/// @audit `_distributeBond()` is called on line 587, triggering an external call - `unlock()`
607:         for (uint256 i = checkpoint.subgameIndex; i < finalCursor; i++) {

/// @audit `_distributeBond()` is called on line 587, triggering an external call - `unlock()`
622:                 checkpoint.counteredBy = claim.claimant;

/// @audit `_distributeBond()` is called on line 587, triggering an external call - `unlock()`
623:                 checkpoint.leftmostPosition = claim.position;

/// @audit `_distributeBond()` is called on line 587, triggering an external call - `unlock()`
628:         checkpoint.subgameIndex = uint32(finalCursor);

/// @audit `_distributeBond()` is called on line 587, triggering an external call - `unlock()`
632:         resolutionCheckpoints[_claimIndex] = checkpoint;

/// @audit `_distributeBond()` is called on line 587, triggering an external call - `unlock()`
640:             resolvedSubgames[_claimIndex] = true;

/// @audit `_distributeBond()` is called on line 587, triggering an external call - `unlock()`
648:                 subgameRootClaim.counteredBy = challenger;

/// @audit `_distributeBond()` is called on line 587, triggering an external call - `unlock()`
656:                 subgameRootClaim.counteredBy = countered;
```

</details>

## Non Critical Issues

### [N-01] Import declarations should import specific identifiers, rather than the whole file

Using import declarations of the form `import {<identifier_name>} from "some/file.sol"` avoids polluting the symbol namespace making flattened files smaller, and speeds up compilation (but does not save any gas).

There are 6 instances:

- *PreimageOracle.sol* ( [8](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L8), [9](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L9) ):

```solidity
8: import "src/cannon/libraries/CannonErrors.sol";

9: import "src/cannon/libraries/CannonTypes.sol";
```

- *DisputeGameFactory.sol* ( [11](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L11), [12](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L12) ):

```solidity
11: import "src/dispute/lib/Types.sol";

12: import "src/dispute/lib/Errors.sol";
```

- *FaultDisputeGame.sol* ( [20](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L20), [21](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L21) ):

```solidity
20: import "src/dispute/lib/Types.sol";

21: import "src/dispute/lib/Errors.sol";
```

### [N-02] Names of constants should use the UPPER_CASE_WITH_UNDERSCORES style

It is recommended by the [Solidity Style Guide](https://docs.soliditylang.org/en/latest/style-guide.html)

There are 4 instances:

- *MIPS.sol* ( [47-47](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L47-L47) ):

```solidity
47:     string public constant version = "1.0.1";
```

- *PreimageOracle.sol* ( [33-33](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L33-L33) ):

```solidity
33:     string public constant version = "1.0.0";
```

- *DisputeGameFactory.sol* ( [25-25](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L25-L25) ):

```solidity
25:     string public constant version = "1.0.0";
```

- *FaultDisputeGame.sol* ( [73-73](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L73-L73) ):

```solidity
73:     string public constant version = "1.2.0";
```

### [N-03] Names of `private`/`internal` functions should be prefixed with an underscore

It is recommended by the [Solidity Style Guide](https://docs.soliditylang.org/en/v0.8.20/style-guide.html#underscore-prefix-for-non-external-functions-and-variables).

<details>
<summary>There are 26 instances (click to show):</summary>

- *MIPS.sol* ( [75-75](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L75-L75), [86-86](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L86-L86), [151-151](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L151-L151), [319-319](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L319-L319), [383-383](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L383-L383), [460-460](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L460-L460), [492-492](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L492-L492), [520-520](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L520-L520), [538-538](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L538-L538), [592-592](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L592-L592), [809-809](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L809-L809) ):

```solidity
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

- *PreimageKeyLib.sol* ( [12-12](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageKeyLib.sol#L12-L12), [29-29](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageKeyLib.sol#L29-L29), [47-47](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageKeyLib.sol#L47-L47) ):

```solidity
12:     function localizeIdent(uint256 _ident, bytes32 _localContext) internal view returns (bytes32 key_) {

29:     function localize(bytes32 _key, bytes32 _localContext) internal view returns (bytes32 localizedKey_) {

47:     function keccak256PreimageKey(bytes memory _preimage) internal pure returns (bytes32 key_) {
```

- *CannonTypes.sol* ( [24-24](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L24-L24), [30-30](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L30-L30), [36-36](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L36-L36), [42-42](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L42-L42), [48-48](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L48-L48), [54-54](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L54-L54), [60-60](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L60-L60), [66-66](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L66-L66), [72-72](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L72-L72), [78-78](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L78-L78), [84-84](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L84-L84), [90-90](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L90-L90) ):

```solidity
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

</details>

### [N-04] Names of `private`/`internal` state variables should be prefixed with an underscore

It is recommended by the [Solidity Style Guide](https://docs.soliditylang.org/en/v0.8.20/style-guide.html#underscore-prefix-for-non-external-functions-and-variables).

<details>
<summary>There are 25 instances (click to show):</summary>

- *MIPS.sol* ( [49-49](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L49-L49), [50-50](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L50-L50), [51-51](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L51-L51), [52-52](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L52-L52), [53-53](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L53-L53), [54-54](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L54-L54), [55-55](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L55-L55), [57-57](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L57-L57), [58-58](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L58-L58), [61-61](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L61-L61) ):

```solidity
49:     uint32 internal constant FD_STDIN = 0;

50:     uint32 internal constant FD_STDOUT = 1;

51:     uint32 internal constant FD_STDERR = 2;

52:     uint32 internal constant FD_HINT_READ = 3;

53:     uint32 internal constant FD_HINT_WRITE = 4;

54:     uint32 internal constant FD_PREIMAGE_READ = 5;

55:     uint32 internal constant FD_PREIMAGE_WRITE = 6;

57:     uint32 internal constant EBADF = 0x9;

58:     uint32 internal constant EINVAL = 0x16;

61:     IPreimageOracle internal immutable ORACLE;
```

- *PreimageOracle.sol* ( [21-21](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L21-L21), [23-23](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L23-L23) ):

```solidity
21:     uint256 internal immutable CHALLENGE_PERIOD;

23:     uint256 internal immutable MIN_LPP_SIZE_BYTES;
```

- *FaultDisputeGame.sol* ( [32-32](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L32-L32), [35-35](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L35-L35), [39-39](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L39-L39), [42-42](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L42-L42), [45-45](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L45-L45), [48-48](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L48-L48), [51-51](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L51-L51), [54-54](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L54-L54), [57-57](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L57-L57), [61-61](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L61-L61), [64-64](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L64-L64), [69-69](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L69-L69), [85-85](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L85-L85) ):

```solidity
32:     Claim internal immutable ABSOLUTE_PRESTATE;

35:     uint256 internal immutable MAX_GAME_DEPTH;

39:     uint256 internal immutable SPLIT_DEPTH;

42:     Duration internal immutable MAX_CLOCK_DURATION;

45:     IBigStepper internal immutable VM;

48:     GameType internal immutable GAME_TYPE;

51:     IDelayedWETH internal immutable WETH;

54:     IAnchorStateRegistry internal immutable ANCHOR_STATE_REGISTRY;

57:     uint256 internal immutable L2_CHAIN_ID;

61:     Duration internal immutable CLOCK_EXTENSION;

64:     Position internal constant ROOT_POSITION = Position.wrap(1);

69:     uint256 internal constant HEADER_BLOCK_NUMBER_INDEX = 8;

85:     bool internal initialized;
```

</details>

### [N-05] Use of `override` is unnecessary

[Starting from Solidity 0.8.8](https://docs.soliditylang.org/en/v0.8.20/contracts.html#function-overriding), the override keyword is not required when overriding an interface function, except for the case where the function is defined in multiple bases.

There is 1 instance:

- *FaultDisputeGame.sol* ( [662-662](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L662-L662) ):

```solidity
662:     function gameType() public view override returns (GameType gameType_) {
```

### [N-06] Consider providing a ranged getter for array state variables

While the compiler automatically provides a getter for accessing single elements within a public state variable array, it doesn't provide a way to fetch the whole array, or subsets thereof. Consider adding a function to allow the fetching of slices of the array, especially if the contract doesn't already have multicall functionality.

There are 6 instances:

- *PreimageOracle.sol* ( [70-70](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L70-L70), [72-72](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L72-L72), [74-74](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L74-L74), [83-83](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L83-L83) ):

```solidity
70:     bytes32[KECCAK_TREE_DEPTH] public zeroHashes;

72:     LargePreimageProposalKeys[] public proposals;

74:     mapping(address => mapping(uint256 => bytes32[KECCAK_TREE_DEPTH])) public proposalBranches;

83:     mapping(address => mapping(uint256 => uint64[])) public proposalBlocks;
```

- *FaultDisputeGame.sol* ( [95-95](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L95-L95), [104-104](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L104-L104) ):

```solidity
95:     ClaimData[] public claimData;

104:     mapping(uint256 => uint256[]) public subgames;
```

### [N-07] Assembly blocks should have extensive comments

Assembly blocks take a lot more time to audit than normal Solidity code, and often have gotchas and side-effects that the Solidity versions of the same code do not. Consider adding more comments explaining what is being done in every step of the assembly code, and describe why assembly is being used instead of Solidity.

<details>
<summary>There are 30 instances (click to show):</summary>

- *MIPS.sol* ( [155-157](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L155-L157), [323-325](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L323-L325), [387-389](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L387-L389), [464-466](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L464-L466), [496-498](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L496-L498), [526-528](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L526-L528), [543](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L543), [597](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L597) ):

```solidity
155:             assembly {
156:                 state := 0x80
157:             }

323:             assembly {
324:                 state := 0x80
325:             }

387:             assembly {
388:                 state := 0x80
389:             }

464:             assembly {
465:                 state := 0x80
466:             }

496:             assembly {
497:                 state := 0x80
498:             }

526:             assembly {
527:                 s := calldatasize()
528:             }

543:             assembly {

597:             assembly {
```

- *PreimageOracle.sol* ( [482](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L482), [560-564](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L560-L564), [681-683](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L681-L683), [729-733](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L729-L733), [747-749](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L747-L749), [767](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L767) ):

```solidity
482:         assembly {

560:         assembly {
561:             mstore(0x00, shl(96, caller()))
562:             calldatacopy(0x14, 0x00, calldatasize())
563:             log0(0x00, add(0x14, calldatasize()))
564:         }

681:         assembly {
682:             finalDigest := or(and(finalDigest, not(shl(248, 0xFF))), shl(248, 0x02))
683:         }

729:             assembly {
730:                 mstore(0x00, shl(192, claimedSize))
731:                 mstore(0x08, calldataload(_input.offset))
732:                 preimagePart := mload(offset)
733:             }

747:             assembly {
748:                 preimagePart := calldataload(add(_input.offset, relativeOffset))
749:             }

767:         assembly {
```

- *CannonTypes.sol* ( [25-27](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L25-L27), [31-33](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L31-L33), [37-39](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L37-L39), [43-45](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L43-L45), [49-51](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L49-L51), [55-57](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L55-L57), [61-63](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L61-L63), [67-69](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L67-L69), [73-75](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L73-L75), [79-81](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L79-L81), [85-87](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L85-L87), [91-93](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L91-L93) ):

```solidity
25:         assembly {
26:             self_ := or(shl(192, _timestamp), and(_self, not(shl(192, U64_MASK))))
27:         }

31:         assembly {
32:             self_ := or(shl(160, _partOffset), and(_self, not(shl(160, U32_MASK))))
33:         }

37:         assembly {
38:             self_ := or(shl(128, _claimedSize), and(_self, not(shl(128, U32_MASK))))
39:         }

43:         assembly {
44:             self_ := or(shl(96, _blocksProcessed), and(_self, not(shl(96, U32_MASK))))
45:         }

49:         assembly {
50:             self_ := or(shl(64, _bytesProcessed), and(_self, not(shl(64, U32_MASK))))
51:         }

55:         assembly {
56:             self_ := or(_countered, and(_self, not(U64_MASK)))
57:         }

61:         assembly {
62:             timestamp_ := shr(192, _self)
63:         }

67:         assembly {
68:             partOffset_ := and(shr(160, _self), U32_MASK)
69:         }

73:         assembly {
74:             claimedSize_ := and(shr(128, _self), U32_MASK)
75:         }

79:         assembly {
80:             blocksProcessed_ := and(shr(96, _self), U32_MASK)
81:         }

85:         assembly {
86:             bytesProcessed_ := and(shr(64, _self), U32_MASK)
87:         }

91:         assembly {
92:             countered_ := and(_self, U64_MASK)
93:         }
```

- *DisputeGameFactory.sol* ( [162-165](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L162-L165), [176-178](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L176-L178) ):

```solidity
162:         assembly {
163:             games_ := mload(0x40)
164:             mstore(0x40, add(games_, add(0x20, shl(0x05, _n))))
165:         }

176:                 assembly {
177:                     mstore(games_, add(mload(games_), 0x01))
178:                 }
```

- *FaultDisputeGame.sol* ( [194-200](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L194-L200), [523-525](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L523-L525) ):

```solidity
194:         assembly {
195:             if iszero(eq(calldatasize(), 0x7A)) {
196:                 // Store the selector for `BadExtraData()` & revert
197:                 mstore(0x00, 0x9824bdab)
198:                 revert(0x1C, 0x04)
199:             }
200:         }

523:         assembly {
524:             blockNumber := shr(shl(0x03, sub(0x20, mload(rawBlockNumber))), mload(add(rawBlockNumber, 0x20)))
525:         }
```

</details>

### [N-08] Consider splitting complex checks into multiple steps

Assign the expression's parts to intermediate local variables, and check against those instead.

There is 1 instance:

- *PreimageOracle.sol* ( [735-735](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L735-L735) ):

```solidity
735:         } else if (offset >= 8 && (offset = offset - 8) >= currentSize && offset < currentSize + _input.length) {
```

### [N-09] Complex casting

Consider whether the number of casts is really necessary, or whether using a different type would be more appropriate. Alternatively, add comments to explain in detail why the casts are necessary, and any implicit reasons why the cast does not introduce an overflow.

There are 5 instances:

- *MIPS.sol* ( [411-411](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L411-L411), [417-417](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L417-L417), [428-428](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L428-L428), [429-429](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L429-L429), [967-967](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L967-L967) ):

```solidity
411:                 uint64 acc = uint64(int64(int32(_rs)) * int64(int32(_rt)));

417:                 uint64 acc = uint64(uint64(_rs) * uint64(_rt));

428:                 state.hi = uint32(int32(_rs) % int32(_rt));

429:                 state.lo = uint32(int32(_rs) / int32(_rt));

967:                         return uint32(int32(rs) * int32(rt));
```

### [N-10] Complex math should be split into multiple steps

Consider splitting long arithmetic calculations into multiple steps to improve the code readability.

<details>
<summary>There are 12 instances (click to show):</summary>

- *MIPS.sol* ( [78-78](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L78-L78), [367-367](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L367-L367), [706-706](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L706-L706), [1014-1014](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1014-L1014), [1015-1015](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1015-L1015), [1020-1020](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1020-L1020), [1021-1021](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1021-L1021), [1026-1026](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1026-L1026), [1027-1027](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1027-L1027), [1042-1042](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1042-L1042), [1043-1043](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1043-L1043) ):

```solidity
78:             uint256 signed = ((1 << (32 - _idx)) - 1) << _idx;

367:                 state.nextPC = prevPC + 4 + (SE(_insn & 0xFFFF, 16) << 2);

706:                 uint32 target = (state.nextPC & 0xF0000000) | (insn & 0x03FFFFFF) << 2;

1014:                     uint32 val = mem >> (24 - (rs & 3) * 8);

1015:                     uint32 mask = uint32(0xFFFFFFFF) >> (24 - (rs & 3) * 8);

1020:                     uint32 val = (rt & 0xFF) << (24 - (rs & 3) * 8);

1021:                     uint32 mask = 0xFFFFFFFF ^ uint32(0xFF << (24 - (rs & 3) * 8));

1026:                     uint32 val = (rt & 0xFFFF) << (16 - (rs & 2) * 8);

1027:                     uint32 mask = 0xFFFFFFFF ^ uint32(0xFFFF << (16 - (rs & 2) * 8));

1042:                     uint32 val = rt << (24 - (rs & 3) * 8);

1043:                     uint32 mask = uint32(0xFFFFFFFF) << (24 - (rs & 3) * 8);
```

- *FaultDisputeGame.sol* ( [269-271](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L269-L271) ):

```solidity
269:             preStateClaim = (stepPos.indexAtDepth() % (1 << (MAX_GAME_DEPTH - SPLIT_DEPTH))) == 0
270:                 ? ABSOLUTE_PRESTATE
271:                 : _findTraceAncestor(Position.wrap(parentPos.raw() - 1), parent.parentIndex, false).claim;
```

</details>

### [N-11] Consider adding a block/deny-list

Doing so will significantly increase centralization, but will help to prevent hackers from using stolen tokens

There are 4 instances:

- *MIPS.sol* ( [23](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L23) ):

```solidity
23: contract MIPS is ISemver {
```

- *PreimageOracle.sol* ( [15](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L15) ):

```solidity
15: contract PreimageOracle is IPreimageOracle, ISemver {
```

- *DisputeGameFactory.sol* ( [19](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L19) ):

```solidity
19: contract DisputeGameFactory is OwnableUpgradeable, IDisputeGameFactory, ISemver {
```

- *FaultDisputeGame.sol* ( [25](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L25) ):

```solidity
25: contract FaultDisputeGame is IFaultDisputeGame, Clone, ISemver {
```

### [N-12] Constants/Immutables redefined elsewhere

Consider defining in only one contract so that values cannot become out of sync when only one location is updated. A [cheap way](https://medium.com/coinmonks/gas-cost-of-solidity-library-functions-dbe0cedd4678) to store constants/immutables in a single location is to create an `internal constant` in a `library`. If the variable is a local cache of another contract's value, consider making the cache variable internal or private, which will require external users to query the contract with the source of truth, so that callers don't get out of sync.

There are 4 instances:

- *MIPS.sol* ( [47-47](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L47-L47) ):

```solidity
/// @audit Seen in src/dispute/DisputeGameFactory.sol#25
47:     string public constant version = "1.0.1";
```

- *PreimageOracle.sol* ( [33-33](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L33-L33) ):

```solidity
/// @audit Seen in src/cannon/MIPS.sol#47
33:     string public constant version = "1.0.0";
```

- *DisputeGameFactory.sol* ( [25-25](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L25-L25) ):

```solidity
/// @audit Seen in src/dispute/FaultDisputeGame.sol#73
25:     string public constant version = "1.0.0";
```

- *FaultDisputeGame.sol* ( [73-73](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L73-L73) ):

```solidity
/// @audit Seen in src/dispute/DisputeGameFactory.sol#25
73:     string public constant version = "1.2.0";
```

### [N-13] Convert simple `if`-statements to ternary expressions

Converting some if statements to ternaries (such as `z = (a < b) ? x : y`) can make the code more concise and easier to read.

<details>
<summary>There are 4 instances (click to show):</summary>

- *MIPS.sol* ( [366-370](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L366-L370), [726-732](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L726-L732) ):

```solidity
366:             if (shouldBranch) {
367:                 state.nextPC = prevPC + 4 + (SE(_insn & 0xFFFF, 16) << 2);
368:             } else {
369:                 state.nextPC = state.nextPC + 4;
370:             }

726:                 if (opcode == 0xC || opcode == 0xD || opcode == 0xe) {
727:                     // ZeroExtImm
728:                     rt = insn & 0xFFFF;
729:                 } else {
730:                     // SignExtImm
731:                     rt = SE(insn & 0xFFFF, 16);
732:                 }
```

- *PreimageOracle.sol* ( [451-455](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L451-L455), [699-703](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L699-L703) ):

```solidity
451:         if (_finalize) {
452:             input = LibKeccak.pad(_input);
453:         } else {
454:             input = _input;
455:         }

699:             if ((size & 1) == 1) {
700:                 treeRoot_ = keccak256(abi.encode(proposalBranches[_owner][_uuid][height], treeRoot_));
701:             } else {
702:                 treeRoot_ = keccak256(abi.encode(treeRoot_, zeroHashes[height]));
703:             }
```

</details>

### [N-14] Contract name does not match its filename

According to the [Solidity Style Guide](https://docs.soliditylang.org/en/latest/style-guide.html#contract-and-library-names), contract and library names should also match their filenames.

There is 1 instance:

- *CannonTypes.sol* ( [20](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L20) ):

```solidity
/// @audit Not match filename `CannonTypes.sol`
20: library LPPMetadataLib {
```

### [N-15] Duplicate imports

Duplicate imports are redundant and should be avoided.

There is 1 instance:

- *FaultDisputeGame.sol* ( [17-17](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L17-L17) ):

```solidity
/// @audit Duplicate import `Types` on line 14 
17: import { Types } from "src/libraries/Types.sol";
```

### [N-16] Consider emitting an event at the end of the constructor

This will allow users to easily exactly pinpoint when and by whom a contract was constructed.

<details>
<summary>There are 4 instances (click to show):</summary>

- *MIPS.sol* ( [64-64](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L64-L64) ):

```solidity
64:     constructor(IPreimageOracle _oracle) {
```

- *PreimageOracle.sol* ( [89-89](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L89-L89) ):

```solidity
89:     constructor(uint256 _minProposalSize, uint256 _challengePeriod) {
```

- *DisputeGameFactory.sol* ( [42-42](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L42-L42) ):

```solidity
42:     constructor() OwnableUpgradeable() {
```

- *FaultDisputeGame.sol* ( [125-136](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L125-L136) ):

```solidity
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

</details>

### [N-17] Events are emitted without the sender information

When an action is triggered based on a user's action, not being able to filter based on who triggered the action makes event processing a lot more cumbersome. Including the `msg.sender` the events of these types of action will make events much more useful to end users, especially when `msg.sender` is not `tx.origin`.

There are 2 instances:

- *DisputeGameFactory.sol* ( [131-131](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L131-L131) ):

```solidity
131:         emit DisputeGameCreated(address(proxy_), _gameType, _rootClaim);
```

- *FaultDisputeGame.sol* ( [553-553](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L553-L553) ):

```solidity
553:         emit Resolved(status = status_);
```

### [N-18] Imports could be organized more systematically

The contract's interface should be imported first, followed by each of the interfaces it uses, followed by all other files. The examples below do not follow this layout.

There are 3 instances:

- *DisputeGameFactory.sol* ( [6-6](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L6-L6) ):

```solidity
/// @audit Out of order with the prev import️ ⬆
6: import { ISemver } from "src/universal/ISemver.sol";
```

- *FaultDisputeGame.sol* ( [6-6](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L6-L6), [15-15](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L15-L15) ):

```solidity
/// @audit Out of order with the prev import️ ⬆
6: import { IDelayedWETH } from "src/dispute/interfaces/IDelayedWETH.sol";

/// @audit Out of order with the prev import️ ⬆
15: import { ISemver } from "src/universal/ISemver.sol";
```

### [N-19] There is no need to initialize bool variables with `false`

Since the bool variables are automatically set to `false` when created, it is redundant to initialize it with `false` again.

There is 1 instance:

- *MIPS.sol* ( [327-327](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L327-L327) ):

```solidity
327:             bool shouldBranch = false;
```

### [N-20] @openzeppelin/contracts should be upgraded to a newer version

These contracts import contracts from `@openzeppelin/contracts`, but they are not using [the latest version](https://github.com/OpenZeppelin/openzeppelin-contracts/releases). Using the latest version ensures contract security with fixes for known vulnerabilities, access to the latest feature updates, and enhanced community support and engagement.

The imported version is `4.7.3`.

There is 1 instance:

- *DisputeGameFactory.sol* ( [5](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L5) ):

```solidity
5: import { OwnableUpgradeable } from "@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol";
```

### [N-21] safe-contracts should be upgraded to a newer version

These contracts import contracts from `@gnosis.pm/safe-contracts`, but they are not using [the latest version](https://github.com/safe-global/safe-contracts/releases). Using the latest version ensures contract security with fixes for known vulnerabilities, access to the latest feature updates, and enhanced community support and engagement.

The imported version is `1.3.0`.

There is 1 instance:

- Global finding

### [N-22] Lib solady should be upgraded to a newer version

These contracts import contracts from lib solady, but they are not using [the latest version](https://github.com/Vectorized/solady/tags). Using the latest version ensures contract security with fixes for known vulnerabilities, access to the latest feature updates, and enhanced community support and engagement.

The imported version is `0.0.158`.

There is 1 instance:

- Global finding

### [N-23] Expressions for constant values should use `immutable` rather than `constant`

While it doesn't save any gas because the compiler knows that developers often make this mistake, it's still best to use the right tool for the task at hand. There is a difference between `constant` variables and `immutable` variables, and they should each be used in their appropriate contexts. `constants` should be used for literal values written into the code, and `immutable` variables should be used for expressions, or values calculated in, or passed into the constructor.

There is 1 instance:

- *FaultDisputeGame.sol* ( [64-64](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L64-L64) ):

```solidity
64:     Position internal constant ROOT_POSITION = Position.wrap(1);
```

### [N-24] Consider moving duplicated strings to constants

Moving duplicate strings to constants can improve code maintainability and readability.

There are 5 instances:

- *MIPS.sol* ( [426-426](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L426-L426), [436-436](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L436-L436), [959-959](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L959-L959), [1054-1054](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1054-L1054), [1057-1057](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1057-L1057) ):

```solidity
426:                     revert("MIPS: division by zero");

436:                     revert("MIPS: division by zero");

959:                     revert("invalid instruction");

1054:                     revert("invalid instruction");

1057:             revert("invalid instruction");
```

### [N-25] Memory-safe annotation preferred over comment variant

The memory-safe annotation (`assembly ("memory-safe") { ... }`) is preferred over the comment variant, which will be removed in a future breaking [release](https://docs.soliditylang.org/en/v0.8.13/assembly.html#memory-safety).

There is 1 instance:

- *PreimageOracle.sol* ( [766](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L766) ):

```solidity
766:         /// @solidity memory-safe-assembly
```

### [N-26] Contract uses both `require()`/`revert()` as well as custom errors

Consider using just one method in a single file.

There is 1 instance:

- *PreimageOracle.sol* ( [15](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L15) ):

```solidity
15: contract PreimageOracle is IPreimageOracle, ISemver {
```

### [N-27] Functions should be named in mixedCase style

As the [Solidity Style Guide](https://docs.soliditylang.org/en/v0.8.21/style-guide.html#function-names) suggests: functions should be named in mixedCase style.

There are 8 instances:

- *MIPS.sol* ( [75](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L75) ):

```solidity
75:     function SE(uint32 _dat, uint32 _idx) internal pure returns (uint32 out_) {
```

- *PreimageOracle.sol* ( [417](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L417), [440](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L440), [568](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L568), [609](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L609), [640](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L640), [696](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L696) ):

```solidity
417:     function initLPP(uint256 _uuid, uint32 _partOffset, uint32 _claimedSize) external payable {

440:     function addLeavesLPP(

568:     function challengeLPP(

609:     function challengeFirstLPP(

640:     function squeezeLPP(

696:     function getTreeRootLPP(address _owner, uint256 _uuid) public view returns (bytes32 treeRoot_) {
```

- *DisputeGameFactory.sol* ( [135](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L135) ):

```solidity
135:     function getGameUUID(
```

### [N-28] Constants should be put on the left side of comparisons

Putting constants on the left side of comparison statements is a best practice known as [Yoda conditions](https://en.wikipedia.org/wiki/Yoda_conditions).
Although solidity's static typing system prevents accidental assignments within conditionals, adopting this practice can improve code readability and consistency, especially when working across multiple languages.

<details>
<summary>There are 143 instances (click to show):</summary>

- *MIPS.sol* ( [170](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L170), [176](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L176), [184](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L184), [188](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L188), [192](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L192), [198](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L198), [201](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L201), [205](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L205), [238](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L238), [248](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L248), [251](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L251), [251](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L251), [251](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L251), [255](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L255), [282](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L282), [285](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L285), [287](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L287), [287](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L287), [287](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L287), [289](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L289), [289](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L289), [289](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L289), [289](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L289), [334](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L334), [334](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L334), [336](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L336), [336](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L336), [339](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L339), [343](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L343), [347](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L347), [350](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L350), [353](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L353), [394](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L394), [398](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L398), [402](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L402), [406](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L406), [410](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L410), [416](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L416), [424](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L424), [434](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L434), [435](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L435), [443](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L443), [478](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L478), [504](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L504), [704](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L704), [704](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L704), [707](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L707), [719](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L719), [719](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L719), [726](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L726), [726](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L726), [726](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L726), [733](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L733), [733](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L733), [733](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L733), [741](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L741), [741](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L741), [749](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L749), [754](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L754), [754](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L754), [766](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L766), [766](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L766), [767](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L767), [767](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L767), [769](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L769), [772](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L772), [774](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L774), [776](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L776), [778](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L778), [782](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L782), [788](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L788), [794](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L794), [794](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L794), [799](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L799), [813](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L813), [813](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L813), [835](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L835), [839](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L839), [843](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L843), [848](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L848), [852](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L852), [856](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L856), [862](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L862), [866](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L866), [870](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L870), [874](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L874), [878](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L878), [883](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L883), [887](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L887), [891](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L891), [895](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L895), [899](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L899), [903](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L903), [907](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L907), [911](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L911), [915](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L915), [920](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L920), [924](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L924), [928](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L928), [932](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L932), [936](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L936), [940](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L940), [944](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L944), [948](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L948), [952](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L952), [956](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L956), [963](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L963), [966](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L966), [970](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L970), [970](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L970), [971](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L971), [983](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L983), [987](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L987), [991](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L991), [995](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L995), [1001](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1001), [1005](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1005), [1009](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1009), [1013](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1013), [1019](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1019), [1025](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1025), [1031](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1031), [1037](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1037), [1041](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1041), [1047](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1047), [1051](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1051) ):

```solidity
/// @audit put `4090` on the left
170:             if (syscall_no == 4090) {

/// @audit put `0` on the left
176:                 if (a0 == 0) {

/// @audit put `4045` on the left
184:             else if (syscall_no == 4045) {

/// @audit put `4120` on the left
188:             else if (syscall_no == 4120) {

/// @audit put `4246` on the left
192:             else if (syscall_no == 4246) {

/// @audit put `4003` on the left
198:             else if (syscall_no == 4003) {

/// @audit put `FD_STDIN` on the left
201:                 if (a0 == FD_STDIN) {

/// @audit put `FD_PREIMAGE_READ` on the left
205:                 else if (a0 == FD_PREIMAGE_READ) {

/// @audit put `FD_HINT_READ` on the left
238:                 else if (a0 == FD_HINT_READ) {

/// @audit put `4004` on the left
248:             else if (syscall_no == 4004) {

/// @audit put `FD_HINT_WRITE` on the left
251:                 if (a0 == FD_STDOUT || a0 == FD_STDERR || a0 == FD_HINT_WRITE) {

/// @audit put `FD_STDOUT` on the left
251:                 if (a0 == FD_STDOUT || a0 == FD_STDERR || a0 == FD_HINT_WRITE) {

/// @audit put `FD_STDERR` on the left
251:                 if (a0 == FD_STDOUT || a0 == FD_STDERR || a0 == FD_HINT_WRITE) {

/// @audit put `FD_PREIMAGE_WRITE` on the left
255:                 else if (a0 == FD_PREIMAGE_WRITE) {

/// @audit put `4055` on the left
282:             else if (syscall_no == 4055) {

/// @audit put `3` on the left
285:                 if (a1 == 3) {

/// @audit put `FD_HINT_READ` on the left
287:                     if (a0 == FD_STDIN || a0 == FD_PREIMAGE_READ || a0 == FD_HINT_READ) {

/// @audit put `FD_STDIN` on the left
287:                     if (a0 == FD_STDIN || a0 == FD_PREIMAGE_READ || a0 == FD_HINT_READ) {

/// @audit put `FD_PREIMAGE_READ` on the left
287:                     if (a0 == FD_STDIN || a0 == FD_PREIMAGE_READ || a0 == FD_HINT_READ) {

/// @audit put `FD_HINT_WRITE` on the left
289:                     } else if (a0 == FD_STDOUT || a0 == FD_STDERR || a0 == FD_PREIMAGE_WRITE || a0 == FD_HINT_WRITE) {

/// @audit put `FD_PREIMAGE_WRITE` on the left
289:                     } else if (a0 == FD_STDOUT || a0 == FD_STDERR || a0 == FD_PREIMAGE_WRITE || a0 == FD_HINT_WRITE) {

/// @audit put `FD_STDOUT` on the left
289:                     } else if (a0 == FD_STDOUT || a0 == FD_STDERR || a0 == FD_PREIMAGE_WRITE || a0 == FD_HINT_WRITE) {

/// @audit put `FD_STDERR` on the left
289:                     } else if (a0 == FD_STDOUT || a0 == FD_STDERR || a0 == FD_PREIMAGE_WRITE || a0 == FD_HINT_WRITE) {

/// @audit put `4` on the left
334:             if (_opcode == 4 || _opcode == 5) {

/// @audit put `5` on the left
334:             if (_opcode == 4 || _opcode == 5) {

/// @audit put `4` on the left
336:                 shouldBranch = (_rs == rt && _opcode == 4) || (_rs != rt && _opcode == 5);

/// @audit put `5` on the left
336:                 shouldBranch = (_rs == rt && _opcode == 4) || (_rs != rt && _opcode == 5);

/// @audit put `6` on the left
339:             else if (_opcode == 6) {

/// @audit put `7` on the left
343:             else if (_opcode == 7) {

/// @audit put `1` on the left
347:             else if (_opcode == 1) {

/// @audit put `0` on the left
350:                 if (rtv == 0) {

/// @audit put `1` on the left
353:                 if (rtv == 1) {

/// @audit put `0x10` on the left
394:             if (_func == 0x10) {

/// @audit put `0x11` on the left
398:             else if (_func == 0x11) {

/// @audit put `0x12` on the left
402:             else if (_func == 0x12) {

/// @audit put `0x13` on the left
406:             else if (_func == 0x13) {

/// @audit put `0x18` on the left
410:             else if (_func == 0x18) {

/// @audit put `0x19` on the left
416:             else if (_func == 0x19) {

/// @audit put `0x1a` on the left
424:             else if (_func == 0x1a) {

/// @audit put `0x1b` on the left
434:             else if (_func == 0x1b) {

/// @audit put `0` on the left
435:                 if (_rt == 0) {

/// @audit put `0` on the left
443:             if (_storeReg != 0) {

/// @audit put `0` on the left
478:             if (_linkReg != 0) {

/// @audit put `0` on the left
504:             if (_storeReg != 0 && _conditional) {

/// @audit put `2` on the left
704:             if (opcode == 2 || opcode == 3) {

/// @audit put `3` on the left
704:             if (opcode == 2 || opcode == 3) {

/// @audit put `2` on the left
707:                 return handleJump(opcode == 2 ? 0 : 31, target);

/// @audit put `0` on the left
719:             if (opcode == 0 || opcode == 0x1c) {

/// @audit put `0x1c` on the left
719:             if (opcode == 0 || opcode == 0x1c) {

/// @audit put `0xe` on the left
726:                 if (opcode == 0xC || opcode == 0xD || opcode == 0xe) {

/// @audit put `0xC` on the left
726:                 if (opcode == 0xC || opcode == 0xD || opcode == 0xe) {

/// @audit put `0xD` on the left
726:                 if (opcode == 0xC || opcode == 0xD || opcode == 0xe) {

/// @audit put `0x26` on the left
733:             } else if (opcode >= 0x28 || opcode == 0x22 || opcode == 0x26) {

/// @audit put `0x28` on the left
733:             } else if (opcode >= 0x28 || opcode == 0x22 || opcode == 0x26) {

/// @audit put `0x22` on the left
733:             } else if (opcode >= 0x28 || opcode == 0x22 || opcode == 0x26) {

/// @audit put `1` on the left
741:             if ((opcode >= 4 && opcode < 8) || opcode == 1) {

/// @audit put `4` on the left
741:             if ((opcode >= 4 && opcode < 8) || opcode == 1) {

/// @audit put `0x20` on the left
749:             if (opcode >= 0x20) {

/// @audit put `0x28` on the left
754:                 if (opcode >= 0x28 && opcode != 0x30) {

/// @audit put `0x30` on the left
754:                 if (opcode >= 0x28 && opcode != 0x30) {

/// @audit put `0` on the left
766:             if (opcode == 0 && func >= 8 && func < 0x1c) {

/// @audit put `8` on the left
766:             if (opcode == 0 && func >= 8 && func < 0x1c) {

/// @audit put `8` on the left
767:                 if (func == 8 || func == 9) {

/// @audit put `9` on the left
767:                 if (func == 8 || func == 9) {

/// @audit put `8` on the left
769:                     return handleJump(func == 8 ? 0 : rdReg, rs);

/// @audit put `0xa` on the left
772:                 if (func == 0xa) {

/// @audit put `0` on the left
774:                     return handleRd(rdReg, rs, rt == 0);

/// @audit put `0xb` on the left
776:                 if (func == 0xb) {

/// @audit put `0` on the left
778:                     return handleRd(rdReg, rs, rt != 0);

/// @audit put `0xC` on the left
782:                 if (func == 0xC) {

/// @audit put `0x10` on the left
788:                 if (func >= 0x10 && func < 0x1c) {

/// @audit put `0x38` on the left
794:             if (opcode == 0x38 && rtReg != 0) {

/// @audit put `0` on the left
794:             if (opcode == 0x38 && rtReg != 0) {

/// @audit put `0xFF_FF_FF_FF` on the left
799:             if (storeAddr != 0xFF_FF_FF_FF) {

/// @audit put `0` on the left
813:             if (opcode == 0 || (opcode >= 8 && opcode < 0xF)) {

/// @audit put `8` on the left
813:             if (opcode == 0 || (opcode >= 8 && opcode < 0xF)) {

/// @audit put `0x00` on the left
835:                 if (func == 0x00) {

/// @audit put `0x02` on the left
839:                 else if (func == 0x02) {

/// @audit put `0x03` on the left
843:                 else if (func == 0x03) {

/// @audit put `0x04` on the left
848:                 else if (func == 0x04) {

/// @audit put `0x6` on the left
852:                 else if (func == 0x6) {

/// @audit put `0x07` on the left
856:                 else if (func == 0x07) {

/// @audit put `0x08` on the left
862:                 else if (func == 0x08) {

/// @audit put `0x09` on the left
866:                 else if (func == 0x09) {

/// @audit put `0x0a` on the left
870:                 else if (func == 0x0a) {

/// @audit put `0x0b` on the left
874:                 else if (func == 0x0b) {

/// @audit put `0x0c` on the left
878:                 else if (func == 0x0c) {

/// @audit put `0x0f` on the left
883:                 else if (func == 0x0f) {

/// @audit put `0x10` on the left
887:                 else if (func == 0x10) {

/// @audit put `0x11` on the left
891:                 else if (func == 0x11) {

/// @audit put `0x12` on the left
895:                 else if (func == 0x12) {

/// @audit put `0x13` on the left
899:                 else if (func == 0x13) {

/// @audit put `0x18` on the left
903:                 else if (func == 0x18) {

/// @audit put `0x19` on the left
907:                 else if (func == 0x19) {

/// @audit put `0x1a` on the left
911:                 else if (func == 0x1a) {

/// @audit put `0x1b` on the left
915:                 else if (func == 0x1b) {

/// @audit put `0x20` on the left
920:                 else if (func == 0x20) {

/// @audit put `0x21` on the left
924:                 else if (func == 0x21) {

/// @audit put `0x22` on the left
928:                 else if (func == 0x22) {

/// @audit put `0x23` on the left
932:                 else if (func == 0x23) {

/// @audit put `0x24` on the left
936:                 else if (func == 0x24) {

/// @audit put `0x25` on the left
940:                 else if (func == 0x25) {

/// @audit put `0x26` on the left
944:                 else if (func == 0x26) {

/// @audit put `0x27` on the left
948:                 else if (func == 0x27) {

/// @audit put `0x2a` on the left
952:                 else if (func == 0x2a) {

/// @audit put `0x2b` on the left
956:                 else if (func == 0x2b) {

/// @audit put `0x1C` on the left
963:                 if (opcode == 0x1C) {

/// @audit put `0x2` on the left
966:                     if (func == 0x2) {

/// @audit put `0x20` on the left
970:                     else if (func == 0x20 || func == 0x21) {

/// @audit put `0x21` on the left
970:                     else if (func == 0x20 || func == 0x21) {

/// @audit put `0x20` on the left
971:                         if (func == 0x20) {

/// @audit put `0x0F` on the left
983:                 else if (opcode == 0x0F) {

/// @audit put `0x20` on the left
987:                 else if (opcode == 0x20) {

/// @audit put `0x21` on the left
991:                 else if (opcode == 0x21) {

/// @audit put `0x22` on the left
995:                 else if (opcode == 0x22) {

/// @audit put `0x23` on the left
1001:                 else if (opcode == 0x23) {

/// @audit put `0x24` on the left
1005:                 else if (opcode == 0x24) {

/// @audit put `0x25` on the left
1009:                 else if (opcode == 0x25) {

/// @audit put `0x26` on the left
1013:                 else if (opcode == 0x26) {

/// @audit put `0x28` on the left
1019:                 else if (opcode == 0x28) {

/// @audit put `0x29` on the left
1025:                 else if (opcode == 0x29) {

/// @audit put `0x2a` on the left
1031:                 else if (opcode == 0x2a) {

/// @audit put `0x2b` on the left
1037:                 else if (opcode == 0x2b) {

/// @audit put `0x2e` on the left
1041:                 else if (opcode == 0x2e) {

/// @audit put `0x30` on the left
1047:                 else if (opcode == 0x30) {

/// @audit put `0x38` on the left
1051:                 else if (opcode == 0x38) {
```

- *PreimageOracle.sol* ( [622](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L622), [727](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L727), [735](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L735) ):

```solidity
/// @audit put `0` on the left
622:         if (_postState.index != 0) revert StatesNotContiguous();

/// @audit put `0` on the left
727:         if (offset < 8 && currentSize == 0) {

/// @audit put `8` on the left
735:         } else if (offset >= 8 && (offset = offset - 8) >= currentSize && offset < currentSize + _input.length) {
```

- *DisputeGameFactory.sol* ( [158](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L158), [168](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L168) ):

```solidity
/// @audit put `0` on the left
158:         if (_start >= _disputeGameList.length || _n == 0) return games_;

/// @audit put `0` on the left
168:         for (uint256 i = _start; i >= 0 && i <= _start;) {
```

- *FaultDisputeGame.sol* ( [307](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L307), [339](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L339), [345](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L345), [549](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L549), [580](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L580), [580](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L580), [586](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L586), [601](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L601), [621](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L621), [643](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L643), [652](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L652), [753](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L753) ):

```solidity
/// @audit put `address(0)` on the left
307:         if (parent.counteredBy != address(0)) revert DuplicateStep();

/// @audit put `0` on the left
339:         if ((_challengeIndex == 0 || nextPositionDepth == SPLIT_DEPTH + 2) && !_isAttack) {

/// @audit put `0` on the left
345:         if (l2BlockNumberChallenged && _challengeIndex == 0) revert L2BlockNumberChallenged();

/// @audit put `address(0)` on the left
549:         status_ = claimData[0].counteredBy == address(0) ? GameStatus.DEFENDER_WINS : GameStatus.CHALLENGER_WINS;

/// @audit put `0` on the left
580:         if (challengeIndicesLen == 0 && _claimIndex != 0) {

/// @audit put `0` on the left
580:         if (challengeIndicesLen == 0 && _claimIndex != 0) {

/// @audit put `address(0)` on the left
586:             address recipient = counteredBy == address(0) ? subgameRootClaim.claimant : counteredBy;

/// @audit put `0` on the left
601:             if (_numToResolve == 0) _numToResolve = challengeIndicesLen;

/// @audit put `address(0)` on the left
621:             if (claim.counteredBy == address(0) && checkpoint.leftmostPosition.raw() > claim.position.raw()) {

/// @audit put `0` on the left
643:             if (_claimIndex == 0 && l2BlockNumberChallenged) {

/// @audit put `address(0)` on the left
652:                 _distributeBond(countered == address(0) ? subgameRootClaim.claimant : countered, subgameRootClaim);

/// @audit put `0` on the left
753:         if (recipientCredit == 0) revert NoCreditToClaim();
```

</details>

### [N-29] `else`-block not required

One level of nesting can be removed by not having an `else` block when the `if`-block always jumps at the end. For example:
```solidity
if (condition) {
    body1...
    return x;
} else {
    body2...
}
```
can be changed to:
```solidity
if (condition) {
    body1...
    return x;
}
body2...
```

<details>
<summary>There are 47 instances (click to show):</summary>

- *MIPS.sol* ( [192-198](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L192-L198), [835-839](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L835-L839), [839-843](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L839-L843), [843-848](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L843-L848), [848-852](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L848-L852), [852-856](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L852-L856), [856-862](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L856-L862), [862-866](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L862-L866), [866-870](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L866-L870), [870-874](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L870-L874), [874-878](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L874-L878), [878-883](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L878-L883), [883-887](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L883-L887), [887-891](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L887-L891), [891-895](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L891-L895), [895-899](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L895-L899), [899-903](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L899-L903), [903-907](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L903-L907), [907-911](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L907-L911), [911-915](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L911-L915), [915-920](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L915-L920), [920-924](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L920-L924), [924-928](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L924-L928), [928-932](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L928-L932), [932-936](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L932-L936), [936-940](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L936-L940), [940-944](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L940-L944), [944-948](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L944-L948), [948-952](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L948-L952), [952-956](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L952-L956), [956-958](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L956-L958), [966-970](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L966-L970), [983-987](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L983-L987), [987-991](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L987-L991), [991-995](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L991-L995), [995-1001](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L995-L1001), [1001-1005](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1001-L1005), [1005-1009](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1005-L1009), [1009-1013](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1009-L1013), [1013-1019](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1013-L1019), [1019-1025](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1019-L1025), [1025-1031](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1025-L1031), [1031-1037](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1031-L1037), [1037-1041](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1037-L1041), [1041-1047](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1041-L1047), [1047-1051](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1047-L1051), [1051-1053](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1051-L1053) ):

```solidity
192:             else if (syscall_no == 4246) {
193:                 state.exited = true;
194:                 state.exitCode = uint8(a0);
195:                 return outputState();
196:             }
197:             // read: Like Linux read syscall. Splits unaligned reads into aligned reads.
198:             else if (syscall_no == 4003) {

835:                 if (func == 0x00) {
836:                     return rt << ((insn >> 6) & 0x1F);
837:                 }
838:                 // srl
839:                 else if (func == 0x02) {

839:                 else if (func == 0x02) {
840:                     return rt >> ((insn >> 6) & 0x1F);
841:                 }
842:                 // sra
843:                 else if (func == 0x03) {

843:                 else if (func == 0x03) {
844:                     uint32 shamt = (insn >> 6) & 0x1F;
845:                     return SE(rt >> shamt, 32 - shamt);
846:                 }
847:                 // sllv
848:                 else if (func == 0x04) {

848:                 else if (func == 0x04) {
849:                     return rt << (rs & 0x1F);
850:                 }
851:                 // srlv
852:                 else if (func == 0x6) {

852:                 else if (func == 0x6) {
853:                     return rt >> (rs & 0x1F);
854:                 }
855:                 // srav
856:                 else if (func == 0x07) {

856:                 else if (func == 0x07) {
857:                     return SE(rt >> rs, 32 - rs);
858:                 }
859:                 // functs in range [0x8, 0x1b] are handled specially by other functions
860:                 // Explicitly enumerate each funct in range to reduce code diff against Go Vm
861:                 // jr
862:                 else if (func == 0x08) {

862:                 else if (func == 0x08) {
863:                     return rs;
864:                 }
865:                 // jalr
866:                 else if (func == 0x09) {

866:                 else if (func == 0x09) {
867:                     return rs;
868:                 }
869:                 // movz
870:                 else if (func == 0x0a) {

870:                 else if (func == 0x0a) {
871:                     return rs;
872:                 }
873:                 // movn
874:                 else if (func == 0x0b) {

874:                 else if (func == 0x0b) {
875:                     return rs;
876:                 }
877:                 // syscall
878:                 else if (func == 0x0c) {

878:                 else if (func == 0x0c) {
879:                     return rs;
880:                 }
881:                 // 0x0d - break not supported
882:                 // sync
883:                 else if (func == 0x0f) {

883:                 else if (func == 0x0f) {
884:                     return rs;
885:                 }
886:                 // mfhi
887:                 else if (func == 0x10) {

887:                 else if (func == 0x10) {
888:                     return rs;
889:                 }
890:                 // mthi
891:                 else if (func == 0x11) {

891:                 else if (func == 0x11) {
892:                     return rs;
893:                 }
894:                 // mflo
895:                 else if (func == 0x12) {

895:                 else if (func == 0x12) {
896:                     return rs;
897:                 }
898:                 // mtlo
899:                 else if (func == 0x13) {

899:                 else if (func == 0x13) {
900:                     return rs;
901:                 }
902:                 // mult
903:                 else if (func == 0x18) {

903:                 else if (func == 0x18) {
904:                     return rs;
905:                 }
906:                 // multu
907:                 else if (func == 0x19) {

907:                 else if (func == 0x19) {
908:                     return rs;
909:                 }
910:                 // div
911:                 else if (func == 0x1a) {

911:                 else if (func == 0x1a) {
912:                     return rs;
913:                 }
914:                 // divu
915:                 else if (func == 0x1b) {

915:                 else if (func == 0x1b) {
916:                     return rs;
917:                 }
918:                 // The rest includes transformed R-type arith imm instructions
919:                 // add
920:                 else if (func == 0x20) {

920:                 else if (func == 0x20) {
921:                     return (rs + rt);
922:                 }
923:                 // addu
924:                 else if (func == 0x21) {

924:                 else if (func == 0x21) {
925:                     return (rs + rt);
926:                 }
927:                 // sub
928:                 else if (func == 0x22) {

928:                 else if (func == 0x22) {
929:                     return (rs - rt);
930:                 }
931:                 // subu
932:                 else if (func == 0x23) {

932:                 else if (func == 0x23) {
933:                     return (rs - rt);
934:                 }
935:                 // and
936:                 else if (func == 0x24) {

936:                 else if (func == 0x24) {
937:                     return (rs & rt);
938:                 }
939:                 // or
940:                 else if (func == 0x25) {

940:                 else if (func == 0x25) {
941:                     return (rs | rt);
942:                 }
943:                 // xor
944:                 else if (func == 0x26) {

944:                 else if (func == 0x26) {
945:                     return (rs ^ rt);
946:                 }
947:                 // nor
948:                 else if (func == 0x27) {

948:                 else if (func == 0x27) {
949:                     return ~(rs | rt);
950:                 }
951:                 // slti
952:                 else if (func == 0x2a) {

952:                 else if (func == 0x2a) {
953:                     return int32(rs) < int32(rt) ? 1 : 0;
954:                 }
955:                 // sltiu
956:                 else if (func == 0x2b) {

956:                 else if (func == 0x2b) {
957:                     return rs < rt ? 1 : 0;
958:                 } else {

966:                     if (func == 0x2) {
967:                         return uint32(int32(rs) * int32(rt));
968:                     }
969:                     // clz, clo
970:                     else if (func == 0x20 || func == 0x21) {

983:                 else if (opcode == 0x0F) {
984:                     return rt << 16;
985:                 }
986:                 // lb
987:                 else if (opcode == 0x20) {

987:                 else if (opcode == 0x20) {
988:                     return SE((mem >> (24 - (rs & 3) * 8)) & 0xFF, 8);
989:                 }
990:                 // lh
991:                 else if (opcode == 0x21) {

991:                 else if (opcode == 0x21) {
992:                     return SE((mem >> (16 - (rs & 2) * 8)) & 0xFFFF, 16);
993:                 }
994:                 // lwl
995:                 else if (opcode == 0x22) {

995:                 else if (opcode == 0x22) {
996:                     uint32 val = mem << ((rs & 3) * 8);
997:                     uint32 mask = uint32(0xFFFFFFFF) << ((rs & 3) * 8);
998:                     return (rt & ~mask) | val;
999:                 }
1000:                 // lw
1001:                 else if (opcode == 0x23) {

1001:                 else if (opcode == 0x23) {
1002:                     return mem;
1003:                 }
1004:                 // lbu
1005:                 else if (opcode == 0x24) {

1005:                 else if (opcode == 0x24) {
1006:                     return (mem >> (24 - (rs & 3) * 8)) & 0xFF;
1007:                 }
1008:                 //  lhu
1009:                 else if (opcode == 0x25) {

1009:                 else if (opcode == 0x25) {
1010:                     return (mem >> (16 - (rs & 2) * 8)) & 0xFFFF;
1011:                 }
1012:                 //  lwr
1013:                 else if (opcode == 0x26) {

1013:                 else if (opcode == 0x26) {
1014:                     uint32 val = mem >> (24 - (rs & 3) * 8);
1015:                     uint32 mask = uint32(0xFFFFFFFF) >> (24 - (rs & 3) * 8);
1016:                     return (rt & ~mask) | val;
1017:                 }
1018:                 //  sb
1019:                 else if (opcode == 0x28) {

1019:                 else if (opcode == 0x28) {
1020:                     uint32 val = (rt & 0xFF) << (24 - (rs & 3) * 8);
1021:                     uint32 mask = 0xFFFFFFFF ^ uint32(0xFF << (24 - (rs & 3) * 8));
1022:                     return (mem & mask) | val;
1023:                 }
1024:                 //  sh
1025:                 else if (opcode == 0x29) {

1025:                 else if (opcode == 0x29) {
1026:                     uint32 val = (rt & 0xFFFF) << (16 - (rs & 2) * 8);
1027:                     uint32 mask = 0xFFFFFFFF ^ uint32(0xFFFF << (16 - (rs & 2) * 8));
1028:                     return (mem & mask) | val;
1029:                 }
1030:                 //  swl
1031:                 else if (opcode == 0x2a) {

1031:                 else if (opcode == 0x2a) {
1032:                     uint32 val = rt >> ((rs & 3) * 8);
1033:                     uint32 mask = uint32(0xFFFFFFFF) >> ((rs & 3) * 8);
1034:                     return (mem & ~mask) | val;
1035:                 }
1036:                 //  sw
1037:                 else if (opcode == 0x2b) {

1037:                 else if (opcode == 0x2b) {
1038:                     return rt;
1039:                 }
1040:                 //  swr
1041:                 else if (opcode == 0x2e) {

1041:                 else if (opcode == 0x2e) {
1042:                     uint32 val = rt << (24 - (rs & 3) * 8);
1043:                     uint32 mask = uint32(0xFFFFFFFF) << (24 - (rs & 3) * 8);
1044:                     return (mem & ~mask) | val;
1045:                 }
1046:                 // ll
1047:                 else if (opcode == 0x30) {

1047:                 else if (opcode == 0x30) {
1048:                     return mem;
1049:                 }
1050:                 // sc
1051:                 else if (opcode == 0x38) {

1051:                 else if (opcode == 0x38) {
1052:                     return rt;
1053:                 } else {
```

</details>

### [N-30] Large multiples of ten should use scientific notation

Use a scientific notation rather than decimal literals (e.g. `1e6` instead of `1000000`), for better code readability.

There are 2 instances:

- *FaultDisputeGame.sol* ( [708-708](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L708-L708), [709-709](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L709-L709) ):

```solidity
/// @audit 400_000 -> 4e5
708:         uint256 baseGasCharged = 400_000;

/// @audit 300_000_000 -> 3e8
709:         uint256 highGasCharged = 300_000_000;
```

### [N-31] High cyclomatic complexity

Consider breaking down these blocks into more manageable units, by splitting things into utility functions, by reducing nesting, and by using early returns.

<details>
<summary>There are 2 instances (click to show):</summary>

- *PreimageOracle.sol* ( [440-565](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L440-L565) ):

```solidity
440:     function addLeavesLPP(
441:         uint256 _uuid,
442:         uint256 _inputStartBlock,
443:         bytes calldata _input,
444:         bytes32[] calldata _stateCommitments,
445:         bool _finalize
446:     )
447:         external
448:     {
449:         // If we're finalizing, pad the input for the submitter. If not, copy the input into memory verbatim.
450:         bytes memory input;
451:         if (_finalize) {
452:             input = LibKeccak.pad(_input);
453:         } else {
454:             input = _input;
455:         }
456: 
457:         // Pull storage variables onto the stack / into memory for operations.
458:         bytes32[KECCAK_TREE_DEPTH] memory branch = proposalBranches[msg.sender][_uuid];
459:         LPPMetaData metaData = proposalMetadata[msg.sender][_uuid];
460:         uint256 blocksProcessed = metaData.blocksProcessed();
461: 
462:         // The caller of `addLeavesLPP` must be an EOA.
463:         // Note: This check may break if EIPs like EIP-3074 are introduced. We may query the data in the logs if this
464:         // is the case.
...... OMITTED ......
541:         // If the proposal is being finalized, set the timestamp to the current block timestamp. This begins the
542:         // challenge period, which must be waited out before the proposal can be finalized.
543:         if (_finalize) {
544:             metaData = metaData.setTimestamp(uint64(block.timestamp));
545: 
546:             // If the number of bytes processed is not equal to the claimed size, the proposal cannot be finalized.
547:             if (metaData.bytesProcessed() != metaData.claimedSize()) revert InvalidInputSize();
548:         }
549: 
550:         // Perist the latest branch to storage.
551:         proposalBranches[msg.sender][_uuid] = branch;
552:         // Persist the block number that these leaves were added in. This assists off-chain observers in reconstructing
553:         // the proposal merkle tree by querying block bodies.
554:         proposalBlocks[msg.sender][_uuid].push(uint64(block.number));
555:         // Persist the updated metadata to storage.
556:         proposalMetadata[msg.sender][_uuid] = metaData;
557: 
558:         // Clobber memory and `log0` all calldata. This is safe because there is no execution afterwards within
559:         // this callframe.
560:         assembly {
561:             mstore(0x00, shl(96, caller()))
562:             calldatacopy(0x14, 0x00, calldatasize())
563:             log0(0x00, add(0x14, calldatasize()))
564:         }
565:     }
```

- *FaultDisputeGame.sol* ( [234-312](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L234-L312) ):

```solidity
234:     function step(
235:         uint256 _claimIndex,
236:         bool _isAttack,
237:         bytes calldata _stateData,
238:         bytes calldata _proof
239:     )
240:         public
241:         virtual
242:     {
243:         // INVARIANT: Steps cannot be made unless the game is currently in progress.
244:         if (status != GameStatus.IN_PROGRESS) revert GameNotInProgress();
245: 
246:         // Get the parent. If it does not exist, the call will revert with OOB.
247:         ClaimData storage parent = claimData[_claimIndex];
248: 
249:         // Pull the parent position out of storage.
250:         Position parentPos = parent.position;
251:         // Determine the position of the step.
252:         Position stepPos = parentPos.move(_isAttack);
253: 
254:         // INVARIANT: A step cannot be made unless the move position is 1 below the `MAX_GAME_DEPTH`
255:         if (stepPos.depth() != MAX_GAME_DEPTH + 1) revert InvalidParent();
256: 
257:         // Determine the expected pre & post states of the step.
258:         Claim preStateClaim;
...... OMITTED ......
288:         Hash uuid = _findLocalContext(_claimIndex);
289: 
290:         // INVARIANT: If a step is an attack, the poststate is valid if the step produces
291:         //            the same poststate hash as the parent claim's value.
292:         //            If a step is a defense:
293:         //              1. If the parent claim and the found post state agree with each other
294:         //                 (depth diff % 2 == 0), the step is valid if it produces the same
295:         //                 state hash as the post state's claim.
296:         //              2. If the parent claim and the found post state disagree with each other
297:         //                 (depth diff % 2 != 0), the parent cannot be countered unless the step
298:         //                 produces the same state hash as `postState.claim`.
299:         // SAFETY:    While the `attack` path does not need an extra check for the post
300:         //            state's depth in relation to the parent, we don't need another
301:         //            branch because (n - n) % 2 == 0.
302:         bool validStep = VM.step(_stateData, _proof, uuid.raw()) == postState.claim.raw();
303:         bool parentPostAgree = (parentPos.depth() - postState.position.depth()) % 2 == 0;
304:         if (parentPostAgree == validStep) revert ValidStep();
305: 
306:         // INVARIANT: A step cannot be made against a claim for a second time.
307:         if (parent.counteredBy != address(0)) revert DuplicateStep();
308: 
309:         // Set the parent claim as countered. We do not need to append a new claim to the game;
310:         // instead, we can just set the existing parent as countered.
311:         parent.counteredBy = msg.sender;
312:     }
```

</details>

### [N-32] Typos

All typos should be corrected.

<details>
<summary>There are 8 instances (click to show):</summary>

- *MIPS.sol* ( [544](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L544), [598](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L598) ):

```solidity
/// @audit alignement
544:                 // Validate the address alignement.

/// @audit alignement
598:                 // Validate the address alignement.
```

- *PreimageOracle.sol* ( [80](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L80), [259](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L259), [320](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L320), [383](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L383), [487](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L487) ):

```solidity
/// @audit absorbtion
80:     /// @notice Mapping of claimants to proposal UUIDs to the preimage part picked up during the absorbtion process.

/// @audit btyes
259:             //         free memory ptr region. Since the exact number of btyes that is copied into scratch space is

/// @audit btyes
320:             // Compute the key: `keccak256(commitment ++ z)`. Since the exact number of btyes that is copied into

/// @audit ofset
383:             // compute part given ofset

/// @audit lenth
487:             // The input lenth / 136 must be equal to the number of state commitments.
```

- *FaultDisputeGame.sol* ( [620](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L620) ):

```solidity
/// @audit claimes
620:             // Note that correctly positioned defense, but invalid claimes can still be successfully countered.
```

</details>

### [N-33] Unnecessary casting

Unnecessary castings can be removed.

There is 1 instance:

- *MIPS.sol* ( [417-417](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L417-L417) ):

```solidity
/// @audit uint64(uint64(_rs) * uint64(_rt))
417:                 uint64 acc = uint64(uint64(_rs) * uint64(_rt));
```

### [N-34] Unused contract variables

The following state variables are defined but not used.
It is recommended to check the code for logical omissions that cause them not to be used. If it's determined that they are not needed anywhere, it's best to remove them from the codebase to improve code clarity and minimize confusion.

There are 4 instances:

- *MIPS.sol* ( [47-47](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L47-L47) ):

```solidity
47:     string public constant version = "1.0.1";
```

- *PreimageOracle.sol* ( [33-33](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L33-L33) ):

```solidity
33:     string public constant version = "1.0.0";
```

- *DisputeGameFactory.sol* ( [25-25](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L25-L25) ):

```solidity
25:     string public constant version = "1.0.0";
```

- *FaultDisputeGame.sol* ( [73-73](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L73-L73) ):

```solidity
73:     string public constant version = "1.2.0";
```

### [N-35] Unused named return

Declaring named returns, but not using them, is confusing to the reader. Consider either completely removing them (by declaring just the type without a name), or remove the return statement and do a variable assignment. This would improve the readability of the code, and it may also help reduce regressions during future code refactors.

There are 2 instances:

- *MIPS.sol* ( [75-75](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L75-L75), [809-809](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L809-L809) ):

```solidity
/// @audit out_
75:     function SE(uint32 _dat, uint32 _idx) internal pure returns (uint32 out_) {

/// @audit out
809:     function execute(uint32 insn, uint32 rs, uint32 rt, uint32 mem) internal pure returns (uint32 out) {
```

### [N-36] Consider using `delete` rather than assigning zero to clear values

The `delete` keyword more closely matches the semantics of what is being done, and draws more attention to the changing of state, which may lead to a more thorough audit of its associated logic.

There are 5 instances:

- *MIPS.sol* ( [273-273](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L273-L273), [288-288](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L288-L288), [758-758](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L758-L758) ):

```solidity
273:                     state.preimageOffset = 0; // reset offset, to read new pre-image data from the start

288:                         v0 = 0; // O_RDONLY

758:                     rdReg = 0;
```

- *PreimageOracle.sol* ( [791-791](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L791-L791) ):

```solidity
791:         proposalBonds[_claimant][_uuid] = 0;
```

- *FaultDisputeGame.sol* ( [750-750](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L750-L750) ):

```solidity
750:         credit[_recipient] = 0;
```

### [N-37] Use the latest Solidity version

Upgrading to the [latest solidity version](https://github.com/ethereum/solc-js/tags) (0.8.19 for L2s) can optimize gas usage, take advantage of new features and improve overall contract efficiency. Where possible, based on compatibility requirements, it is recommended to use newer/latest solidity version to take advantage of the latest optimizations and features.

<details>
<summary>There are 6 instances (click to show):</summary>

- *MIPS.sol* ( [2](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L2) ):

```solidity
2: pragma solidity 0.8.15;
```

- *PreimageKeyLib.sol* ( [2](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageKeyLib.sol#L2) ):

```solidity
2: pragma solidity 0.8.15;
```

- *PreimageOracle.sol* ( [2](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L2) ):

```solidity
2: pragma solidity 0.8.15;
```

- *CannonTypes.sol* ( [2](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L2) ):

```solidity
2: pragma solidity 0.8.15;
```

- *DisputeGameFactory.sol* ( [2](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L2) ):

```solidity
2: pragma solidity 0.8.15;
```

- *FaultDisputeGame.sol* ( [2](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L2) ):

```solidity
2: pragma solidity 0.8.15;
```

</details>

### [N-38] Consider using named function arguments

When calling functions with multiple arguments, consider using [named function parameters](https://docs.soliditylang.org/en/latest/control-structures.html#function-calls-with-named-parameters), rather than positional ones.

There are 6 instances:

- *FaultDisputeGame.sol* ( [302-302](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L302-L302), [440-440](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L440-L440), [443-443](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L443-L443), [446-446](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L446-L446), [455-455](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L455-L455), [458-458](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L458-L458) ):

```solidity
302:         bool validStep = VM.step(_stateData, _proof, uuid.raw()) == postState.claim.raw();

440:             oracle.loadLocalData(_ident, uuid.raw(), l1Head().raw(), 32, _partOffset);

443:             oracle.loadLocalData(_ident, uuid.raw(), starting.raw(), 32, _partOffset);

446:             oracle.loadLocalData(_ident, uuid.raw(), disputed.raw(), 32, _partOffset);

455:             oracle.loadLocalData(_ident, uuid.raw(), bytes32(l2Number << 0xC0), 8, _partOffset);

458:             oracle.loadLocalData(_ident, uuid.raw(), bytes32(L2_CHAIN_ID << 0xC0), 8, _partOffset);
```

### [N-39] Named returns are recommended

Using named returns makes the code more self-documenting, makes it easier to fill out NatSpec, and in some cases can save gas. The cases below are where there currently is at most one return statement, which is ideal for named returns.

There is 1 instance:

- *MIPS.sol* ( [640-640](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L640-L640) ):

```solidity
640:     function step(bytes calldata _stateData, bytes calldata _proof, bytes32 _localContext) public returns (bytes32) {
```

### [N-40] Use transfer libraries instead of low level calls to transfer native tokens

Consider using [`SafeTransferLib.safeTransferETH`](https://github.com/transmissions11/solmate/blob/fadb2e2778adbf01c80275bfb99e5c14969d964b/src/utils/SafeTransferLib.sol#L15) or [`Address.sendValue`](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/9e3f4d60c581010c4a3979480e07cc7752f124cc/contracts/utils/Address.sol#L41) for clearer semantic meaning instead of using a low level `call`.

There are 2 instances:

- *PreimageOracle.sol* ( [792-792](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L792-L792) ):

```solidity
792:         (bool success,) = _to.call{ value: bond }("");
```

- *FaultDisputeGame.sol* ( [759-759](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L759-L759) ):

```solidity
759:         (bool success,) = _recipient.call{ value: recipientCredit }(hex"");
```

### [N-41] Using underscore at the end of variable name

The use of the underscore at the end of the variable name is unusual, consider refactoring it.

<details>
<summary>There are 76 instances (click to show):</summary>

- *MIPS.sol* ( [70-70](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L70-L70), [75-75](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L75-L75), [86-86](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L86-L86), [151-151](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L151-L151), [319-319](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L319-L319), [383-383](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L383-L383), [460-460](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L460-L460), [492-492](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L492-L492), [520-520](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L520-L520), [538-538](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L538-L538) ):

```solidity
/// @audit oracle_
70:     function oracle() external view returns (IPreimageOracle oracle_) {

/// @audit out_
75:     function SE(uint32 _dat, uint32 _idx) internal pure returns (uint32 out_) {

/// @audit out_
86:     function outputState() internal returns (bytes32 out_) {

/// @audit out_
151:     function handleSyscall(bytes32 _localContext) internal returns (bytes32 out_) {

/// @audit out_
319:     function handleBranch(uint32 _opcode, uint32 _insn, uint32 _rtReg, uint32 _rs) internal returns (bytes32 out_) {

/// @audit out_
383:     function handleHiLo(uint32 _func, uint32 _rs, uint32 _rt, uint32 _storeReg) internal returns (bytes32 out_) {

/// @audit out_
460:     function handleJump(uint32 _linkReg, uint32 _dest) internal returns (bytes32 out_) {

/// @audit out_
492:     function handleRd(uint32 _storeReg, uint32 _val, bool _conditional) internal returns (bytes32 out_) {

/// @audit offset_
520:     function proofOffset(uint8 _proofIndex) internal pure returns (uint256 offset_) {

/// @audit out_
538:     function readMem(uint32 _addr, uint8 _proofIndex) internal pure returns (uint32 out_) {
```

- *PreimageKeyLib.sol* ( [12-12](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageKeyLib.sol#L12-L12), [29-29](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageKeyLib.sol#L29-L29), [47-47](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageKeyLib.sol#L47-L47) ):

```solidity
/// @audit key_
12:     function localizeIdent(uint256 _ident, bytes32 _localContext) internal view returns (bytes32 key_) {

/// @audit localizedKey_
29:     function localize(bytes32 _key, bytes32 _localContext) internal view returns (bytes32 localizedKey_) {

/// @audit key_
47:     function keccak256PreimageKey(bytes memory _preimage) internal pure returns (bytes32 key_) {
```

- *PreimageOracle.sol* ( [104-104](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L104-L104), [104-104](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L104-L104), [128-128](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L128-L128), [396-396](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L396-L396), [402-402](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L402-L402), [407-407](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L407-L407), [412-412](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L412-L412), [696-696](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L696-L696), [764-764](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L764-L764), [797-797](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L797-L797) ):

```solidity
/// @audit dat_
104:     function readPreimage(bytes32 _key, uint256 _offset) external view returns (bytes32 dat_, uint256 datLen_) {

/// @audit datLen_
104:     function readPreimage(bytes32 _key, uint256 _offset) external view returns (bytes32 dat_, uint256 datLen_) {

/// @audit key_
128:         returns (bytes32 key_)

/// @audit count_
396:     function proposalCount() external view returns (uint256 count_) {

/// @audit len_
402:     function proposalBlocksLen(address _claimant, uint256 _uuid) external view returns (uint256 len_) {

/// @audit challengePeriod_
407:     function challengePeriod() external view returns (uint256 challengePeriod_) {

/// @audit minProposalSize_
412:     function minProposalSize() external view returns (uint256 minProposalSize_) {

/// @audit treeRoot_
696:     function getTreeRootLPP(address _owner, uint256 _uuid) public view returns (bytes32 treeRoot_) {

/// @audit isValid_
764:         returns (bool isValid_)

/// @audit leaf_
797:     function _hashLeaf(Leaf memory _leaf) internal pure returns (bytes32 leaf_) {
```

- *CannonTypes.sol* ( [24-24](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L24-L24), [30-30](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L30-L30), [36-36](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L36-L36), [42-42](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L42-L42), [48-48](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L48-L48), [54-54](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L54-L54), [60-60](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L60-L60), [66-66](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L66-L66), [72-72](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L72-L72), [78-78](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L78-L78), [84-84](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L84-L84), [90-90](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L90-L90) ):

```solidity
/// @audit self_
24:     function setTimestamp(LPPMetaData _self, uint64 _timestamp) internal pure returns (LPPMetaData self_) {

/// @audit self_
30:     function setPartOffset(LPPMetaData _self, uint32 _partOffset) internal pure returns (LPPMetaData self_) {

/// @audit self_
36:     function setClaimedSize(LPPMetaData _self, uint32 _claimedSize) internal pure returns (LPPMetaData self_) {

/// @audit self_
42:     function setBlocksProcessed(LPPMetaData _self, uint32 _blocksProcessed) internal pure returns (LPPMetaData self_) {

/// @audit self_
48:     function setBytesProcessed(LPPMetaData _self, uint32 _bytesProcessed) internal pure returns (LPPMetaData self_) {

/// @audit self_
54:     function setCountered(LPPMetaData _self, bool _countered) internal pure returns (LPPMetaData self_) {

/// @audit timestamp_
60:     function timestamp(LPPMetaData _self) internal pure returns (uint64 timestamp_) {

/// @audit partOffset_
66:     function partOffset(LPPMetaData _self) internal pure returns (uint64 partOffset_) {

/// @audit claimedSize_
72:     function claimedSize(LPPMetaData _self) internal pure returns (uint32 claimedSize_) {

/// @audit blocksProcessed_
78:     function blocksProcessed(LPPMetaData _self) internal pure returns (uint32 blocksProcessed_) {

/// @audit bytesProcessed_
84:     function bytesProcessed(LPPMetaData _self) internal pure returns (uint32 bytesProcessed_) {

/// @audit countered_
90:     function countered(LPPMetaData _self) internal pure returns (bool countered_) {
```

- *DisputeGameFactory.sol* ( [54-54](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L54-L54), [66-66](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L66-L66), [66-66](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L66-L66), [77-77](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L77-L77), [77-77](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L77-L77), [77-77](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L77-L77), [91-91](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L91-L91), [142-142](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L142-L142), [155-155](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L155-L155) ):

```solidity
/// @audit gameCount_
54:     function gameCount() external view returns (uint256 gameCount_) {

/// @audit proxy_
66:         returns (IDisputeGame proxy_, Timestamp timestamp_)

/// @audit timestamp_
66:         returns (IDisputeGame proxy_, Timestamp timestamp_)

/// @audit gameType_
77:         returns (GameType gameType_, Timestamp timestamp_, IDisputeGame proxy_)

/// @audit timestamp_
77:         returns (GameType gameType_, Timestamp timestamp_, IDisputeGame proxy_)

/// @audit proxy_
77:         returns (GameType gameType_, Timestamp timestamp_, IDisputeGame proxy_)

/// @audit proxy_
91:         returns (IDisputeGame proxy_)

/// @audit uuid_
142:         returns (Hash uuid_)

/// @audit games_
155:         returns (GameSearchResult[] memory games_)
```

- *FaultDisputeGame.sol* ( [465-465](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L465-L465), [474-474](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L474-L474), [479-479](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L479-L479), [484-484](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L484-L484), [541-541](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L541-L541), [662-662](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L662-L662), [667-667](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L667-L667), [672-672](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L672-L672), [677-677](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L677-L677), [682-682](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L682-L682), [689-689](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L689-L689), [689-689](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L689-L689), [689-689](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L689-L689), [702-702](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L702-L702), [767-767](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L767-L767), [789-789](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L789-L789), [798-798](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L798-L798), [803-803](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L803-L803), [808-808](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L808-L808), [813-813](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L813-L813), [818-818](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L818-L818), [823-823](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L823-L823), [828-828](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L828-L828), [833-833](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L833-L833), [838-838](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L838-L838), [911-911](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L911-L911), [934-934](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L934-L934), [934-934](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L934-L934), [934-934](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L934-L934), [934-934](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L934-L934), [995-995](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L995-L995), [1015-1015](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L1015-L1015) ):

```solidity
/// @audit numRemainingChildren_
465:     function getNumToResolve(uint256 _claimIndex) public view returns (uint256 numRemainingChildren_) {

/// @audit l2BlockNumber_
474:     function l2BlockNumber() public pure returns (uint256 l2BlockNumber_) {

/// @audit startingBlockNumber_
479:     function startingBlockNumber() external view returns (uint256 startingBlockNumber_) {

/// @audit startingRootHash_
484:     function startingRootHash() external view returns (Hash startingRootHash_) {

/// @audit status_
541:     function resolve() external returns (GameStatus status_) {

/// @audit gameType_
662:     function gameType() public view override returns (GameType gameType_) {

/// @audit creator_
667:     function gameCreator() public pure returns (address creator_) {

/// @audit rootClaim_
672:     function rootClaim() public pure returns (Claim rootClaim_) {

/// @audit l1Head_
677:     function l1Head() public pure returns (Hash l1Head_) {

/// @audit extraData_
682:     function extraData() public pure returns (bytes memory extraData_) {

/// @audit gameType_
689:     function gameData() external view returns (GameType gameType_, Claim rootClaim_, bytes memory extraData_) {

/// @audit rootClaim_
689:     function gameData() external view returns (GameType gameType_, Claim rootClaim_, bytes memory extraData_) {

/// @audit extraData_
689:     function gameData() external view returns (GameType gameType_, Claim rootClaim_, bytes memory extraData_) {

/// @audit requiredBond_
702:     function getRequiredBond(Position _position) public view returns (uint256 requiredBond_) {

/// @audit duration_
767:     function getChallengerDuration(uint256 _claimIndex) public view returns (Duration duration_) {

/// @audit len_
789:     function claimDataLen() external view returns (uint256 len_) {

/// @audit absolutePrestate_
798:     function absolutePrestate() external view returns (Claim absolutePrestate_) {

/// @audit maxGameDepth_
803:     function maxGameDepth() external view returns (uint256 maxGameDepth_) {

/// @audit splitDepth_
808:     function splitDepth() external view returns (uint256 splitDepth_) {

/// @audit maxClockDuration_
813:     function maxClockDuration() external view returns (Duration maxClockDuration_) {

/// @audit clockExtension_
818:     function clockExtension() external view returns (Duration clockExtension_) {

/// @audit vm_
823:     function vm() external view returns (IBigStepper vm_) {

/// @audit weth_
828:     function weth() external view returns (IDelayedWETH weth_) {

/// @audit registry_
833:     function anchorStateRegistry() external view returns (IAnchorStateRegistry registry_) {

/// @audit l2ChainId_
838:     function l2ChainId() external view returns (uint256 l2ChainId_) {

/// @audit ancestor_
911:         returns (ClaimData storage ancestor_)

/// @audit startingClaim_
934:         returns (Claim startingClaim_, Position startingPos_, Claim disputedClaim_, Position disputedPos_)

/// @audit startingPos_
934:         returns (Claim startingClaim_, Position startingPos_, Claim disputedClaim_, Position disputedPos_)

/// @audit disputedClaim_
934:         returns (Claim startingClaim_, Position startingPos_, Claim disputedClaim_, Position disputedPos_)

/// @audit disputedPos_
934:         returns (Claim startingClaim_, Position startingPos_, Claim disputedClaim_, Position disputedPos_)

/// @audit uuid_
995:     function _findLocalContext(uint256 _claimIndex) internal view returns (Hash uuid_) {

/// @audit uuid_
1015:         returns (Hash uuid_)
```

</details>

### [N-42] Use a struct to encapsulate multiple function parameters

If a function has too many parameters, replacing them with a struct can improve code readability and maintainability, increase reusability, and reduce the likelihood of errors when passing the parameters.

<details>
<summary>There are 6 instances (click to show):</summary>

- *PreimageOracle.sol* ( [120-128](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L120-L128), [244-252](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L244-L252), [440-448](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L440-L448), [568-578](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L568-L578), [640-650](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L640-L650) ):

```solidity
120:     function loadLocalData(
121:         uint256 _ident,
122:         bytes32 _localContext,
123:         bytes32 _word,
124:         uint256 _size,
125:         uint256 _partOffset
126:     )
127:         external
128:         returns (bytes32 key_)

244:     function loadBlobPreimagePart(
245:         uint256 _z,
246:         uint256 _y,
247:         bytes calldata _commitment,
248:         bytes calldata _proof,
249:         uint256 _partOffset
250:     )
251:         external
252:     {

440:     function addLeavesLPP(
441:         uint256 _uuid,
442:         uint256 _inputStartBlock,
443:         bytes calldata _input,
444:         bytes32[] calldata _stateCommitments,
445:         bool _finalize
446:     )
447:         external
448:     {

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
```

- *FaultDisputeGame.sol* ( [125-136](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L125-L136) ):

```solidity
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

</details>

### [N-43] Returning a struct instead of a bunch of variables is better

If a function returns [too many variables](https://docs.soliditylang.org/en/v0.8.21/contracts.html#returning-multiple-values), replacing them with a struct can improve code readability, maintainability and reusability.

<details>
<summary>There are 5 instances (click to show):</summary>

- *PreimageOracle.sol* ( [104-104](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L104-L104) ):

```solidity
104:     function readPreimage(bytes32 _key, uint256 _offset) external view returns (bytes32 dat_, uint256 datLen_) {
```

- *DisputeGameFactory.sol* ( [59-66](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L59-L66), [74-77](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L74-L77) ):

```solidity
59:     function games(
60:         GameType _gameType,
61:         Claim _rootClaim,
62:         bytes calldata _extraData
63:     )
64:         external
65:         view
66:         returns (IDisputeGame proxy_, Timestamp timestamp_)

74:     function gameAtIndex(uint256 _index)
75:         external
76:         view
77:         returns (GameType gameType_, Timestamp timestamp_, IDisputeGame proxy_)
```

- *FaultDisputeGame.sol* ( [689-689](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L689-L689), [931-934](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L931-L934) ):

```solidity
689:     function gameData() external view returns (GameType gameType_, Claim rootClaim_, bytes memory extraData_) {

931:     function _findStartingAndDisputedOutputs(uint256 _start)
932:         internal
933:         view
934:         returns (Claim startingClaim_, Position startingPos_, Claim disputedClaim_, Position disputedPos_)
```

</details>

### [N-44] Contract variables should have comments

Consider adding some comments on non-public contract variables to explain what they are supposed to do. This will help for future code reviews.

There are 11 instances:

- *MIPS.sol* ( [49-49](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L49-L49), [50-50](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L50-L50), [51-51](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L51-L51), [52-52](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L52-L52), [53-53](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L53-L53), [54-54](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L54-L54), [55-55](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L55-L55), [57-57](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L57-L57), [58-58](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L58-L58) ):

```solidity
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

- *CannonTypes.sol* ( [21-21](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L21-L21), [22-22](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L22-L22) ):

```solidity
21:     uint256 private constant U64_MASK = 0xFFFFFFFFFFFFFFFF;

22:     uint256 private constant U32_MASK = 0xFFFFFFFF;
```

### [N-45] Missing event when a state variables is set in constructor

The initial states set in a constructor are important and should be recorded in the event.

There is 1 instance:

- *PreimageOracle.sol* ( [95](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L95) ):

```solidity
95:             zeroHashes[height + 1] = keccak256(abi.encodePacked(zeroHashes[height], zeroHashes[height]));
```

### [N-46] Empty bytes check is missing

Passing empty bytes to a function can cause unexpected behavior, such as certain operations failing, producing incorrect results, or wasting gas. It is recommended to check that all byte parameters are not empty.

<details>
<summary>There are 12 instances (click to show):</summary>

- *MIPS.sol* ( [640-640](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L640-L640) ):

```solidity
/// @audit _stateData
/// @audit _proof
640:     function step(bytes calldata _stateData, bytes calldata _proof, bytes32 _localContext) public returns (bytes32) {
```

- *PreimageOracle.sol* ( [160-160](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L160-L160), [195-195](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L195-L195), [244-252](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L244-L252), [335-335](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L335-L335), [440-448](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L440-L448) ):

```solidity
/// @audit _preimage
160:     function loadKeccak256PreimagePart(uint256 _partOffset, bytes calldata _preimage) external {

/// @audit _preimage
195:     function loadSha256PreimagePart(uint256 _partOffset, bytes calldata _preimage) external {

/// @audit _commitment
/// @audit _proof
244:     function loadBlobPreimagePart(
245:         uint256 _z,
246:         uint256 _y,
247:         bytes calldata _commitment,
248:         bytes calldata _proof,
249:         uint256 _partOffset
250:     )
251:         external
252:     {

/// @audit _input
335:     function loadPrecompilePreimagePart(uint256 _partOffset, address _precompile, bytes calldata _input) external {

/// @audit _input
440:     function addLeavesLPP(
441:         uint256 _uuid,
442:         uint256 _inputStartBlock,
443:         bytes calldata _input,
444:         bytes32[] calldata _stateCommitments,
445:         bool _finalize
446:     )
447:         external
448:     {
```

- *DisputeGameFactory.sol* ( [84-92](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L84-L92) ):

```solidity
/// @audit _extraData
84:     function create(
85:         GameType _gameType,
86:         Claim _rootClaim,
87:         bytes calldata _extraData
88:     )
89:         external
90:         payable
91:         returns (IDisputeGame proxy_)
92:     {
```

- *FaultDisputeGame.sol* ( [234-242](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L234-L242), [492-497](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L492-L497) ):

```solidity
/// @audit _stateData
/// @audit _proof
234:     function step(
235:         uint256 _claimIndex,
236:         bool _isAttack,
237:         bytes calldata _stateData,
238:         bytes calldata _proof
239:     )
240:         public
241:         virtual
242:     {

/// @audit _headerRLP
492:     function challengeRootL2Block(
493:         Types.OutputRootProof calldata _outputRootProof,
494:         bytes calldata _headerRLP
495:     )
496:         external
497:     {
```

</details>

### [N-47] Assembly block creates dirty bits

Writing data to the free memory pointer without later updating the free memory pointer will cause there to be dirty bits at that memory location. Not updating the free memory pointer will make it [harder](https://docs.soliditylang.org/en/latest/ir-breaking-changes.html#cleanup) for the optimizer to reason about whether the memory needs to be cleaned before use, which will lead to worse optimizations. Update the free memory pointer and annotate the block (`assembly ("memory-safe") { ... }`) to avoid this issue.

<details>
<summary>There are 6 instances (click to show):</summary>

- *MIPS.sol* ( [87-145](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L87-L145), [645-690](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L645-L690) ):

```solidity
87:         assembly {
88:             // copies 'size' bytes, right-aligned in word at 'from', to 'to', incl. trailing data
89:             function copyMem(from, to, size) -> fromOut, toOut {
90:                 mstore(to, mload(add(from, sub(32, size))))
91:                 fromOut := add(from, 32)
92:                 toOut := add(to, size)
93:             }
94: 
95:             // From points to the MIPS State
96:             let from := 0x80
97: 
98:             // Copy to the free memory pointer
99:             let start := mload(0x40)
100:             let to := start
101: 
102:             // Copy state to free memory
103:             from, to := copyMem(from, to, 32) // memRoot
104:             from, to := copyMem(from, to, 32) // preimageKey
105:             from, to := copyMem(from, to, 4) // preimageOffset
106:             from, to := copyMem(from, to, 4) // pc
107:             from, to := copyMem(from, to, 4) // nextPC
108:             from, to := copyMem(from, to, 4) // lo
109:             from, to := copyMem(from, to, 4) // hi
110:             from, to := copyMem(from, to, 4) // heap
111:             let exitCode := mload(from)
...... OMITTED ......
121:             // Clean up end of memory
122:             mstore(to, 0)
123: 
124:             // Log the resulting MIPS state, for debugging
125:             log0(start, sub(to, start))
126: 
127:             // Determine the VM status
128:             let status := 0
129:             switch exited
130:             case 1 {
131:                 switch exitCode
132:                 // VMStatusValid
133:                 case 0 { status := 0 }
134:                 // VMStatusInvalid
135:                 case 1 { status := 1 }
136:                 // VMStatusPanic
137:                 default { status := 2 }
138:             }
139:             // VMStatusUnfinished
140:             default { status := 3 }
141: 
142:             // Compute the hash of the resulting MIPS state and set the status byte
143:             out_ := keccak256(start, sub(to, start))
144:             out_ := or(and(not(shl(248, 0xFF)), out_), shl(248, status))
145:         }

645:             assembly {
646:                 if iszero(eq(state, 0x80)) {
647:                     // expected state mem offset check
648:                     revert(0, 0)
649:                 }
650:                 if iszero(eq(mload(0x40), shl(5, 48))) {
651:                     // expected memory check
652:                     revert(0, 0)
653:                 }
654:                 if iszero(eq(_stateData.offset, 132)) {
655:                     // 32*4+4=132 expected state data offset
656:                     revert(0, 0)
657:                 }
658:                 if iszero(eq(_proof.offset, 420)) {
659:                     // 132+32+256=420 expected proof offset
660:                     revert(0, 0)
661:                 }
662: 
663:                 function putField(callOffset, memOffset, size) -> callOffsetOut, memOffsetOut {
664:                     // calldata is packed, thus starting left-aligned, shift-right to pad and right-align
665:                     let w := shr(shl(3, sub(32, size)), calldataload(callOffset))
666:                     mstore(memOffset, w)
667:                     callOffsetOut := add(callOffset, size)
668:                     memOffsetOut := add(memOffset, 32)
669:                 }
670: 
671:                 // Unpack state from calldata into memory
672:                 let c := _stateData.offset // calldata offset
673:                 let m := 0x80 // mem offset
674:                 c, m := putField(c, m, 32) // memRoot
675:                 c, m := putField(c, m, 32) // preimageKey
676:                 c, m := putField(c, m, 4) // preimageOffset
677:                 c, m := putField(c, m, 4) // pc
678:                 c, m := putField(c, m, 4) // nextPC
679:                 c, m := putField(c, m, 4) // lo
680:                 c, m := putField(c, m, 4) // hi
681:                 c, m := putField(c, m, 4) // heap
682:                 c, m := putField(c, m, 1) // exitCode
683:                 c, m := putField(c, m, 1) // exited
684:                 c, m := putField(c, m, 8) // step
685: 
686:                 // Unpack register calldata into memory
687:                 mstore(m, add(m, 32)) // offset to registers
688:                 m := add(m, 32)
689:                 for { let i := 0 } lt(i, 32) { i := add(i, 1) } { c, m := putField(c, m, 4) }
690:             }
```

- *PreimageKeyLib.sol* ( [30-41](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageKeyLib.sol#L30-L41) ):

```solidity
30:         assembly {
31:             // Grab the current free memory pointer to restore later.
32:             let ptr := mload(0x40)
33:             // Store the local data key and caller next to each other in memory for hashing.
34:             mstore(0, _key)
35:             mstore(0x20, caller())
36:             mstore(0x40, _localContext)
37:             // Localize the key with the above `localize` operation.
38:             localizedKey_ := or(and(keccak256(0, 0x60), not(shl(248, 0xFF))), shl(248, 1))
39:             // Restore the free memory pointer.
40:             mstore(0x40, ptr)
41:         }
```

- *PreimageOracle.sol* ( [482-530](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L482-L530), [498-529](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L498-L529) ):

```solidity
482:         assembly {
483:             let inputLen := mload(input)
484:             let inputPtr := add(input, 0x20)
485: 
486:             // The input length must be a multiple of 136 bytes
487:             // The input lenth / 136 must be equal to the number of state commitments.
488:             if or(mod(inputLen, 136), iszero(eq(_stateCommitments.length, div(inputLen, 136)))) {
489:                 // Store "InvalidInputSize()" error selector
490:                 mstore(0x00, 0x7b1daf1)
491:                 revert(0x1C, 0x04)
492:             }
493: 
494:             // Allocate a hashing buffer the size of the leaf preimage.
495:             let hashBuf := mload(0x40)
496:             mstore(0x40, add(hashBuf, 0xC8))
497: 
498:             for { let i := 0 } lt(i, inputLen) { i := add(i, 136) } {
499:                 // Copy the leaf preimage into the hashing buffer.
500:                 let inputStartPtr := add(inputPtr, i)
501:                 mstore(hashBuf, mload(inputStartPtr))
502:                 mstore(add(hashBuf, 0x20), mload(add(inputStartPtr, 0x20)))
503:                 mstore(add(hashBuf, 0x40), mload(add(inputStartPtr, 0x40)))
504:                 mstore(add(hashBuf, 0x60), mload(add(inputStartPtr, 0x60)))
505:                 mstore(add(hashBuf, 0x80), mload(add(inputStartPtr, 0x80)))
506:                 mstore(add(hashBuf, 136), blocksProcessed)
507:                 mstore(add(hashBuf, 168), calldataload(add(_stateCommitments.offset, shl(0x05, div(i, 136)))))
508: 
509:                 // Hash the leaf preimage to get the node to add.
510:                 let node := keccak256(hashBuf, 0xC8)
511: 
512:                 // Increment the number of blocks processed.
513:                 blocksProcessed := add(blocksProcessed, 0x01)
514: 
515:                 // Add the node to the tree.
516:                 let size := blocksProcessed
517:                 for { let height := 0x00 } lt(height, shl(0x05, KECCAK_TREE_DEPTH)) { height := add(height, 0x20) } {
518:                     if and(size, 0x01) {
519:                         mstore(add(branch, height), node)
520:                         break
521:                     }
522: 
523:                     // Hash the node at `height` in the branch and the node together.
524:                     mstore(0x00, mload(add(branch, height)))
525:                     mstore(0x20, node)
526:                     node := keccak256(0x00, 0x40)
527:                     size := shr(0x01, size)
528:                 }
529:             }
530:         }

498:             for { let i := 0 } lt(i, inputLen) { i := add(i, 136) } {
499:                 // Copy the leaf preimage into the hashing buffer.
500:                 let inputStartPtr := add(inputPtr, i)
501:                 mstore(hashBuf, mload(inputStartPtr))
502:                 mstore(add(hashBuf, 0x20), mload(add(inputStartPtr, 0x20)))
503:                 mstore(add(hashBuf, 0x40), mload(add(inputStartPtr, 0x40)))
504:                 mstore(add(hashBuf, 0x60), mload(add(inputStartPtr, 0x60)))
505:                 mstore(add(hashBuf, 0x80), mload(add(inputStartPtr, 0x80)))
506:                 mstore(add(hashBuf, 136), blocksProcessed)
507:                 mstore(add(hashBuf, 168), calldataload(add(_stateCommitments.offset, shl(0x05, div(i, 136)))))
508: 
509:                 // Hash the leaf preimage to get the node to add.
510:                 let node := keccak256(hashBuf, 0xC8)
511: 
512:                 // Increment the number of blocks processed.
513:                 blocksProcessed := add(blocksProcessed, 0x01)
514: 
515:                 // Add the node to the tree.
516:                 let size := blocksProcessed
517:                 for { let height := 0x00 } lt(height, shl(0x05, KECCAK_TREE_DEPTH)) { height := add(height, 0x20) } {
518:                     if and(size, 0x01) {
519:                         mstore(add(branch, height), node)
520:                         break
521:                     }
522: 
523:                     // Hash the node at `height` in the branch and the node together.
524:                     mstore(0x00, mload(add(branch, height)))
525:                     mstore(0x20, node)
526:                     node := keccak256(0x00, 0x40)
527:                     size := shr(0x01, size)
528:                 }
529:             }
```

- *DisputeGameFactory.sol* ( [162-165](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L162-L165) ):

```solidity
162:         assembly {
163:             games_ := mload(0x40)
164:             mstore(0x40, add(games_, add(0x20, shl(0x05, _n))))
165:         }
```

</details>

### [N-48] Multiple mappings with same keys can be combined into a single struct mapping for readability

Well-organized data structures make code reviews easier, which may lead to fewer bugs. Consider combining related mappings into mappings to structs, so it's clear what data is related.

<details>
<summary>There are 13 instances (click to show):</summary>

- *PreimageOracle.sol* ( [40-40](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L40-L40), [42-42](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L42-L42), [44-44](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L44-L44), [74-74](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L74-L74), [77-77](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L77-L77), [79-79](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L79-L79), [81-81](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L81-L81), [83-83](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L83-L83) ):

```solidity
40:     mapping(bytes32 => uint256) public preimageLengths;

42:     mapping(bytes32 => mapping(uint256 => bytes32)) public preimageParts;

44:     mapping(bytes32 => mapping(uint256 => bool)) public preimagePartOk;

74:     mapping(address => mapping(uint256 => bytes32[KECCAK_TREE_DEPTH])) public proposalBranches;

77:     mapping(address => mapping(uint256 => LPPMetaData)) public proposalMetadata;

79:     mapping(address => mapping(uint256 => uint256)) public proposalBonds;

81:     mapping(address => mapping(uint256 => bytes32)) public proposalParts;

83:     mapping(address => mapping(uint256 => uint64[])) public proposalBlocks;
```

- *DisputeGameFactory.sol* ( [28-28](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L28-L28), [31-31](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L31-L31) ):

```solidity
28:     mapping(GameType => IDisputeGame) public gameImpls;

31:     mapping(GameType => uint256) public initBonds;
```

- *FaultDisputeGame.sol* ( [104-104](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L104-L104), [107-107](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L107-L107), [110-110](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L110-L110) ):

```solidity
104:     mapping(uint256 => uint256[]) public subgames;

107:     mapping(uint256 => bool) public resolvedSubgames;

110:     mapping(uint256 => ResolutionCheckpoint) public resolutionCheckpoints;
```

</details>

### [N-49] Missing event for critical changes

Events should be emitted when critical changes are made to the contracts.

There is 1 instance:

- *PreimageOracle.sol* ( [440-448](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L440-L448) ):

```solidity
440:     function addLeavesLPP(
441:         uint256 _uuid,
442:         uint256 _inputStartBlock,
443:         bytes calldata _input,
444:         bytes32[] calldata _stateCommitments,
445:         bool _finalize
446:     )
447:         external
448:     {
```

### [N-50] Non-assembly method available

There are some automated tools that will flag a project as having higher complexity if there is inline-assembly, so it's best to avoid using it where it's not necessary. In addition, most assembly methods can be replaced by non-assembly methods, for example:
- `assembly{ g := gas() }` => `uint256 g = gasleft()`
- `assembly{ id := chainid() }` => `uint256 id = block.chainid`
- `assembly { r := mulmod(a, b, d) }` => `uint256 m = mulmod(x, y, k)`
- `assembly { size := extcodesize() }` => `uint256 size = address(a).code.length`
- etc.

There are 3 instances:

- *MIPS.sol* ( [606-606](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L606-L606), [607-607](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L607-L607), [629-629](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L629-L629) ):

```solidity
606:                 leaf := or(and(leaf, not(shl(shamt, 0xFFffFFff))), shl(shamt, _val))

607:                 offset := add(offset, 32)

629:                 mstore(0x80, node)
```

### [N-51] Consider adding emergency-stop functionality

Adding a way to quickly halt protocol functionality in an emergency, rather than having to pause individual contracts one-by-one, will make in-progress hack mitigation faster and much less stressful.

There is 1 instance:

- *DisputeGameFactory.sol* ( [19-19](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L19-L19) ):

```solidity
19: contract DisputeGameFactory is OwnableUpgradeable, IDisputeGameFactory, ISemver {
```

### [N-52] Avoid the use of sensitive terms

Use [alternative variants](https://www.zdnet.com/article/mysql-drops-master-slave-and-blacklist-whitelist-terminology/), e.g. allowlist/denylist instead of whitelist/blacklist.

There are 2 instances:

- *MIPS.sol* ( [21](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L21) ):

```solidity
21: /// @dev https://github.com/golang/go/blob/master/src/syscall/zerrors_linux_mips.go
```

- *FaultDisputeGame.sol* ( [515](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L515) ):

```solidity
515:         // Sanity check the block number string length.
```

### [N-53] Missing checks for uint state variable assignments

Consider whether reasonable bounds checks for variables would be useful.

There are 2 instances:

- *FaultDisputeGame.sol* ( [226-226](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L226-L226), [550-550](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L550-L550) ):

```solidity
226:         createdAt = Timestamp.wrap(uint64(block.timestamp));

550:         resolvedAt = Timestamp.wrap(uint64(block.timestamp));
```

### [N-54] Use the Modern Upgradeable Contract Paradigm

Modern smart contract development often employs upgradeable contract structures, utilizing proxy patterns like OpenZeppelin’s Upgradeable Contracts. This paradigm separates logic and state, allowing developers to amend and enhance the contract's functionality without altering its state or the deployed contract address. Transitioning to this approach enhances long-term maintainability.
Resolution: Adopt a well-established proxy pattern for upgradeability, ensuring proper initialization and employing transparent proxies to mitigate potential risks. Embrace comprehensive testing and audit practices, particularly when updating contract logic, to ensure state consistency and security are preserved across upgrades. This ensures your contract remains robust and adaptable to future requirements.

There are 2 instances:

- *MIPS.sol* ( [23](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L23) ):

```solidity
23: contract MIPS is ISemver {
```

- *PreimageOracle.sol* ( [15](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L15) ):

```solidity
15: contract PreimageOracle is IPreimageOracle, ISemver {
```

### [N-55] Large or complicated code bases should implement invariant tests

This includes: large code bases, or code with lots of inline-assembly, complicated math, or complicated interactions between multiple contracts.
Invariant fuzzers such as Echidna require the test writer to come up with invariants which should not be violated under any circumstances, and the fuzzer tests various inputs and function calls to ensure that the invariants always hold.
Even code with 100% code coverage can still have bugs due to the order of the operations a user performs, and invariant fuzzers may help significantly.

There is 1 instance:

- Global finding

### [N-56] The default value is manually set when it is declared

In instances where a new variable is defined, there is no need to set it to it's default value.

There are 7 instances:

- *MIPS.sol* ( [49-49](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L49-L49), [161-161](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L161-L161), [162-162](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L162-L162), [525-525](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L525-L525), [974-974](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L974-L974) ):

```solidity
49:     uint32 internal constant FD_STDIN = 0;

161:             uint32 v0 = 0;

162:             uint32 v1 = 0;

525:             uint256 s = 0;

974:                         uint32 i = 0;
```

- *PreimageOracle.sol* ( [94-94](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L94-L94), [698-698](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L698-L698) ):

```solidity
94:         for (uint256 height = 0; height < KECCAK_TREE_DEPTH - 1; height++) {

698:         for (uint256 height = 0; height < KECCAK_TREE_DEPTH; height++) {
```

### [N-57] Contracts should have all `public`/`external` functions exposed by `interface`s

All `external`/`public` functions should extend an `interface`. This is useful to ensure that the whole API is extracted and can be more easily integrated by other projects.

There are 4 instances:

- *MIPS.sol* ( [23](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L23) ):

```solidity
23: contract MIPS is ISemver {
```

- *PreimageOracle.sol* ( [15](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L15) ):

```solidity
15: contract PreimageOracle is IPreimageOracle, ISemver {
```

- *DisputeGameFactory.sol* ( [19](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L19) ):

```solidity
19: contract DisputeGameFactory is OwnableUpgradeable, IDisputeGameFactory, ISemver {
```

- *FaultDisputeGame.sol* ( [25](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L25) ):

```solidity
25: contract FaultDisputeGame is IFaultDisputeGame, Clone, ISemver {
```

### [N-58] Numeric values having to do with time should use time units for readability

There are [time units](https://docs.soliditylang.org/en/latest/units-and-global-variables.html#time-units) for seconds, minutes, hours, days, and weeks, and since they're defined, they should be used.

<details>
<summary>There are 10 instances (click to show):</summary>

- *MIPS.sol* ( [303-303](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L303-L303), [343-343](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L343-L343), [988-988](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L988-L988), [1006-1006](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1006-L1006), [1014-1014](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1014-L1014), [1015-1015](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1015-L1015), [1020-1020](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1020-L1020), [1021-1021](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1021-L1021), [1042-1042](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1042-L1042), [1043-1043](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L1043-L1043) ):

```solidity
/// @audit 7
303:             state.registers[7] = v1;

/// @audit 7
343:             else if (_opcode == 7) {

/// @audit 24
988:                     return SE((mem >> (24 - (rs & 3) * 8)) & 0xFF, 8);

/// @audit 24
1006:                     return (mem >> (24 - (rs & 3) * 8)) & 0xFF;

/// @audit 24
1014:                     uint32 val = mem >> (24 - (rs & 3) * 8);

/// @audit 24
1015:                     uint32 mask = uint32(0xFFFFFFFF) >> (24 - (rs & 3) * 8);

/// @audit 24
1020:                     uint32 val = (rt & 0xFF) << (24 - (rs & 3) * 8);

/// @audit 24
1021:                     uint32 mask = 0xFFFFFFFF ^ uint32(0xFF << (24 - (rs & 3) * 8));

/// @audit 24
1042:                     uint32 val = rt << (24 - (rs & 3) * 8);

/// @audit 24
1043:                     uint32 mask = uint32(0xFFFFFFFF) << (24 - (rs & 3) * 8);
```

</details>
