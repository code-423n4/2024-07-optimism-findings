## L1 `addLeavesLPP`  don't check  `_input` length , possible cause proposal can not finalize 

https://github.com/code-423n4/2024-07-optimism/blob/0f8027921e621a9ac75d8eb20cbda873965e3b8a/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L543-L548

in function `addLeavesLPP`, when finalize, function have limit `bytesProcessed()` equals `claimedSize` 
 
```solidity
      function addLeavesLPP(
        uint256 _uuid,
        uint256 _inputStartBlock,
        bytes calldata _input,
        bytes32[] calldata _stateCommitments,
        bool _finalize
    )
        ... 
         metaData = metaData.setBlocksProcessed(uint32(blocksProcessed)).setBytesProcessed(
                uint32(_input.length + metaData.bytesProcessed())
            );
        // If the proposal is being finalized, set the timestamp to the current block timestamp. This begins the
        // challenge period, which must be waited out before the proposal can be finalized.
        if (_finalize) {
            metaData = metaData.setTimestamp(uint64(block.timestamp));
            // If the number of bytes processed is not equal to the claimed size, the proposal cannot be finalized.
            if (metaData.bytesProcessed() != metaData.claimedSize()) revert InvalidInputSize();
        }

```
the issue is `_inputs` don't have input length check,  once input length +  `bytesProcessed` > `claimedSize`  and status is not `finalized`, 
`bytesProcessed` is possible larger than `claimedSize` ,if this situation happens, proposal can never not be finalized



## L2  `_inputStartBlock`  should be  `blocksProcessed` +1

https://github.com/code-423n4/2024-07-optimism/blob/0f8027921e621a9ac75d8eb20cbda873965e3b8a/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L475

In function `addLeavesLPP`, we have check to make sure  starting block is  the next block to be added,  but this check seems to mark sure `_inputStartBlock` equals
the blocks processed,not the next block to be processed. 

```solidity

    function addLeavesLPP(
...
      // Revert if the starting block is not the next block to be added. This is to aid submitters in ensuring that
        // they don't corrupt an in-progress proposal by submitting input out of order.
        if (blocksProcessed != _inputStartBlock) revert WrongStartingBlock();

```
if we want to make sure `_inputStartBlock` in next block to be processed, it should be 

```diff

- if (blocksProcessed != _inputStartBlock) revert WrongStartingBlock();
+ if (blocksProcessed != _inputStartBlock + 1) revert WrongStartingBlock();

```


  
