### Low-01 Incorrect comment - The actual revert condition is when `_global` is set to false.
**Instances(1)**
The code comment for _findTraceAncestor() states that if `_global` set to true, and `_pos` is at or above the split depth, this function will revert. This is incorrect.

It should be if _global set to false, and `_pos` is at or above the split depth, this function. Because the internal library method `traceAncestorBounded()`'s revert will only be triggered if `_global` is false and `_position.depth() <= SPLIT_DEPTH`.

```solidity
//packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol
    /// @notice Finds the trace ancestor of a given position within the DAG.
    /// @param _pos The position to find the trace ancestor claim of.
    /// @param _start The index to start searching from.
|>  /// @param _global Whether or not to search the entire dag or just within an execution trace subgame. If set to
    ///                `true`, and `_pos` is at or above the split depth, this function will revert.
    /// @return ancestor_ The ancestor claim that commits to the same trace index as `_pos`.
    function _findTraceAncestor(
        Position _pos,
        uint256 _start,
        bool _global
    )
        internal
        view
        returns (ClaimData storage ancestor_)
    {
        // Grab the trace ancestor's expected position.
        Position traceAncestorPos = _global ? _pos.traceAncestor() : _pos.traceAncestorBounded(SPLIT_DEPTH); //@audit-info note: only when _global() is false, _pos.traceAncestorBounded(SPLIT_DEPTH) will run which contains the revert condition.
...
```
(https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L901)
```solidity
//packages/contracts-bedrock/src/dispute/lib/LibPosition.sol
    function traceAncestorBounded(
        Position _position,
        uint256 _upperBoundExclusive
    )
        internal
        pure
        returns (Position ancestor_)
    {
        // This function only works for positions that are below the upper bound.
        if (_position.depth() <= _upperBoundExclusive) {
            assembly {
                // Revert with `ClaimAboveSplit()`
                mstore(0x00, 0xb34b5c22)
                revert(0x1C, 0x04)
            }
        }
...
```
(https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/lib/LibPosition.sol#L165-L169)

Recommendations:
Change the _global parameter comment into `If set to false, and _pos is at or above the split depth, this function will revert.`

### Low-02 Incorrect comment in FaultDisputeGame::step
**Instances(1)**

FaultDisputeGame::step has an incorrect comment regarding the defence scenario. See incorrect comment:
```soliditity
        // INVARIANT: If a step is an attack, the poststate is valid if the step produces
        //            the same poststate hash as the parent claim's value.
        //            If a step is a defense:
        //              1. If the parent claim and the found post state agree with each other
|>      //                 (depth diff % 2 == 0), the step is valid if it produces the same
        //                 state hash as the post state's claim.
        //              2. If the parent claim and the found post state disagree with each other
        //                 (depth diff % 2 != 0), the parent cannot be countered unless the step
        //                 produces the same state hash as `postState.claim`.
...
        bool validStep = VM.step(_stateData, _proof, uuid.raw()) == postState.claim.raw();
        bool parentPostAgree = (parentPos.depth() - postState.position.depth()) % 2 == 0;
        if (parentPostAgree == validStep) revert ValidStep();
...
```
(https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L294)

Based on the implementation, when parent claim and the found post state agree with each other (`depth diff % 2 != 0`), the step is only valid when (parentPostAgree != validStep), this means validStep needs to be false, which means the stateHash from VM.step should be different from postState.

Recommendations:
Change the highlighted comment into `If the parent claim and the found post state agree with each other(depth diff % 2 == 0), the step is valid if it doesn't produce the same state hash as postState.claim`.

### Low-03 Possible racing condition between step() and resolveClaim(), may result in dishonest claimant wins.
**Instances(1)**
Unlike move(), challengeDuration is not checked in step(). As a result, `step()` can be called at any time even after the game lock expires. 

`resolveClaim()` can only be called after the challenger's game lock expires, which means that `step()` and `resolveClaim()` can both be called after the challenger's game lock expires. This opens up the risk of racing between step() and resolveClaim().

POC: 
In the time window where both `step()` and `resolveClaim()` are allowed:
(1) An honest challenger challenge attacks an execution tract claim with `step()`.

(2) A malicious actor(e.g. claimant of the claim that is about to be challenged) can front-run the `step()` tx with `resolveClaim()`. `resolveClaim()` will succeed due to counterdBy is address(0) and the original claimant get the bonds.

(3) The honest challenger's `step()` tx settles after `resolveClaim()` and set the `parent.counteredBy` to the challenger's address. However, the challenger will not get bond rewards because the parent claim is already resolved.

Recommendations:
(1) Currently `resolveClaim()` can resolve a claim minimally CLOCK_EXTENSION after the target parent claim is posted. Consider adding additional delay for `resolveClaim()` when the parent claim is at max depth, to allow more time for `step()` challenges. 
(2) In `step()`, add a check on the status of the `resolvedSubgames[_claimIndex]`, and if `resolvedSubgames[_claimIndex]== true`, revert `step()` because it has no effects but wasting honest challenger's gas.













