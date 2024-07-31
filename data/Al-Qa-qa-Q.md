## Summary

|ID|title|severity|
|:-|:----|:------:|
|[L-01](#l-01-no-checking-for-the-max_clock_duration-in-fdgstep)|No checking for the `MAX_CLOCK_DURATION` in `FDG::step()`|Low|
|[L-02](#l-02-unchecking-that-the-l2-lies-in-the-boundaries-of-the-available-blocks)|Unchecking that the L2 lies in the boundaries of the available Blocks|Low|
|[L-03](#l-03-withdrawal-operations-in-a-short-time-will-be-more-difficult-to-challenge-than-others)|Withdrawal operations in a short time will be more difficult to challenge than others|Low|
|[L-04](#l-04-race-condition-for-extracting-value-in-fgdstep)|Race Condition for Extracting Value in `FGD::Step()`|Low|
||||
|[I-01](#i-01-mismatching-between-variable-type-in-fdg_findtraceancestor)|Mismatching between variable type in `FDG::_findTraceAncestor()`|Info|
|[I-02](#i-02-incorrect-comment-in-ifaultdisputegame)|Incorrect Comment in `IFaultDisputeGame`|Info|
|[I-03](#i-03-there-is-no-receive-function-and-the-contract-will-be-used-to-receive-eth)|There is no receive() function and the contract will be used to receive ETH|Info|

---

## [L-01] No checking for the `MAX_CLOCK_DURATION` in `FDG::step()`

In order for the `move()` to take place, the node Clock should not exceeds the `MAX_CLOCK_DURATION`, which is like the game is still running for that node.

[FaultDisputeGame.sol#L365-L369](https://github.com/code-423n4/2024-07-optimism/blob/main/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L365-L369)
```solidity
        Duration nextDuration = getChallengerDuration(_challengeIndex);

        // INVARIANT: A move can never be made once its clock has exceeded `MAX_CLOCK_DURATION`
        //            seconds of time.
        if (nextDuration.raw() == MAX_CLOCK_DURATION.raw()) revert ClockTimeExceeded();
```

But this check is not implemented in `FDG::step()`, so this will allow users to step over the node even it the duration Duration has been passed.

### Recommendations
Check that the Duration for that claimIndex is still has enough time to modify it.

```diff
diff --git a/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol b/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol
index ab55914..09b9ab2 100644
--- a/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol
+++ b/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol
@@ -254,6 +254,9 @@ contract FaultDisputeGame is IFaultDisputeGame, Clone, ISemver {
         // INVARIANT: A step cannot be made unless the move position is 1 below the `MAX_GAME_DEPTH`
         if (stepPos.depth() != MAX_GAME_DEPTH + 1) revert InvalidParent();
 
+        Duration nextDuration = getChallengerDuration(_claimIndex);
+        if (nextDuration.raw() == MAX_CLOCK_DURATION.raw()) revert ClockTimeExceeded();
+
         // Determine the expected pre & post states of the step.
         Claim preStateClaim;
         ClaimData storage postState;
```

---

## [L-02] Unchecking that the L2 lies in the boundaries of the available Blocks

The Number of Available Blocks that are available are determined by the `SPLIT_DEPTH` number, where if `SPLIT_DEPTH` equals 10 then we can provide an `l2BlockNumber` from that in the `AnchorStateRegistry`.

But what if the L2 chain did not receive a Withdrawal (L2 -> L1) request for a long time? L2 blocks are proposed every 1 to 2 seconds in Optimism, so we have `~50K` block each day, and we don't know how long it will take to receive a new withdrawal from L2 -> L1 to update the L2 block Number in `AnchorStateRegistry`.

So if the `SPLIT_DEPTH` is set to a number that will not satisfy the amount of blocks proposed in L2 from the last withdrawal to the new withdrawal, the game process can get broken.

This is not a user error or a malicious L2BlockNumber proposed by the user, let's illustrate by example.

- If we say `SPLIT_DEPTH` = `10` i.e. 1024 blocks
- the last block is `10`
- Optimism has minted `10000` blocks
- The user made a withdraw
- The problem here is that the available Blocks to play against will be only `1024` instead of `10K` blocks, that have been passed.

### Recommendations
We can check in the `FDG::initialize()` that the l2BlockNumber is bounded between the anchor BlockNumber and the about of Blocks that the Game can satisfy.

---

## [L-03] Withdrawal operations in a short time will be more difficult to challenge than others

In order for the challenge to take place, we should reach a block we are disputing against, But as the Number of blocks the the tree can occupy depends on `SPLIT_DEPTH` it will be hard to challenge a proposal the difference between its L2BlockNumber and `ANCHOR_STATE_REGISTRY` l2 blockNumber is too small.

If the  `ANCHOR_STATE_REGISTRY` just finalized a game and updated the l2blockNumber, then another FDG just deployed.

- The difference between each two block numbers will be small, may be just the `3.5 days` or a little more.
- If the amount of blocks that the game can occupy is too large (SPLIT_DEPTH is set with relativliy large value), a large percentage of the tree will be an invalid to get challenged as they are either future blocks after that Withdrawal or not even created on L2
- If we said that `SPLIT_DEPTH` occuby `10M` blocks for example, and the distance between `ANCHOR_STATE_REGISTRY` l2block and the submitted block is `100,000` the challenger will have to make attack after attack to reach to the the blocks that are existed in that challenge `10M -> 5M -> 2.5M -> 1.25M -> 0.625M -> 312,500 -> 156,250 -> 78,125` in order to reach to the point to challenge him. 

---

## [L-04] Race Condition for Extracting Value in `FGD::Step()`

`FDG::step()` is the process which if the challenger wins it, he will claim the Bonds of the claimant. Since These Bonds are not small `~60 ETH` it can be Attractive for MEVs to track the calling of this function which will result in valid stepping over the Node and FrontRun the Challenger to Claim the Bonds themselves.

This case will introduce race conditions where there may be a lot of Front-Running processes and each of them wants to claim the Bonds for himself.

---
---
---

## [I-01] Mismatching between variable type in `FDG::_findTraceAncestor()`

This function is used to get the `Ancestor` of a given node, It takes its Position, start index, and the `global` which will bound searching. The problem is that the function takes the `start index` as `uint256`, and this variable will be the index of the parent of the Node, like in this example.

[FaultDisputeGame.sol#L880](https://github.com/code-423n4/2024-07-optimism/blob/main/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L880)
```solidity
        ClaimData storage disputed = _findTraceAncestor({ _pos: disputedLeafPos, _start: _parentIdx, _global: true });
```

The second variable `_parentIdx` is of type `uint32` according to the ClaimData interface.

[IFaultDisputeGame.sol#L13](https://github.com/code-423n4/2024-07-optimism/blob/main/packages/contracts-bedrock/src/dispute/interfaces/IFaultDisputeGame.sol#L13)
```solidity
    struct ClaimData {
@>      uint32 parentIndex;
        address counteredBy;
        address claimant;
        uint128 bond;
        Claim claim;
        Position position;
        Clock clock;
    }
```


---

## [I-02] Incorrect Comment in `IFaultDisputeGame`

The comment for `addLocalData` is written wrongly where it is write `PreimageOralce` instead of `PreimageOracle`.

[IFaultDisputeGame.sol#L63-L67](https://github.com/code-423n4/2024-07-optimism/blob/main/packages/contracts-bedrock/src/dispute/interfaces/IFaultDisputeGame.sol#L63-L67)
```solidity
    /// @notice Posts the requested local data to the VM's `PreimageOralce`.
```

### Recommendations
Change it to `PreimageOracle`

---

## [I-03] There is no receive() function and the contract will be used to receive ETH

In the current implementation of FDG, there is no receive function, Although this doesn't mean the contract will not be able to receive ETH, as it is implemented as a proxy with fallback, having `receive()` function is good for code readability.

This will make the proxy delegates to the `receive()` function in the implementation, and will not affect its usage.

## Recommendations
Add `receive()` function to the contract