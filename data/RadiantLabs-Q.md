## [QA-01] MIPS: The MIPS VM does not throw on overflow exceptions for signed arithmetic `add`, `addi`, `sub` instructions which deviates from MIPS spec

For the `add`, `addi`, `sub` instructions which perform signed arithmetic, the MIPS VM does not handle signed overflows appropriately. A signed overflow occurs when:

1) The sign bit of the operands are the same AND
2) The sign bit of the result is different from the operands

```solidity
	// add
	else if (func == 0x20) {
		return (rs + rt);
	}
```
```solidity
	// sub
	else if (func == 0x22) {
		return (rs - rt);
	}
```
As seen in the above snippets, the MIPS VM performs unsigned arithmetic for signed arithmetic instructions where the overflow does not lead to an exception. For example, consider the following operation: 0b0100 + 0b0100.

Performing unsigned addition will lead to a result 0b1000, but according to MIPS specification - please check the relevant information on the MIPS ISA [opcodes](https://www.cs.cmu.edu/afs/cs/academic/class/15740-f97/public/doc/mips-isa.pdf), the signed addition must throw an exception for this result because the sign bit is 0 for both operands while the sign bit is 1 for the result. The only impact of this is that it deviates from the MIPS specification, as the offchain MIPS VM has the same bug. 

## [QA-02] MIPS: The MIPS VM does not throw when an unaligned effective memory address is used for certain instructions

As per the [MIPS spec](https://www.cs.cmu.edu/afs/cs/academic/class/15740-f97/public/doc/mips-isa.pdf), certain instructions are supposed to throw if the effective memory address `M[R[rs]+SignExtImm]` is unaligned, but they currently don't.

One such example is the `sw` instruction that copies 4 bytes of data from a source register to destination memory address. If an unaligned effective memory address is used, the operation proceeds as normal instead of throwing an exception as per MIPS spec:
```solidity
	//  sw 
	else if (opcode == 0x2b) {
		return rt;
	}
```
There are many instructions that throw an exception on unaligned memory, search `AddressError` in the MIPS [document](https://www.cs.cmu.edu/afs/cs/academic/class/15740-f97/public/doc/mips-isa.pdf) for more details. The only impact of this is that it deviates from the MIPS specification, as the offchain MIPS VM has the same bug. 

## [QA-03] MIPS: `sc` does not write a 0 to `rt` if the write is non-atomic

 As per the MIPS ISA, `sc` will write a 1 to `rt` if a write is atomic and a 0 to `rt` is non-atomic. A write is atomic if the value of the memory address pointed to by the `sc` instruction has not been changed since the value of the same memory address has been loaded by the `ll` instruction. However, the `sc` implementation in the MIPS code will always write a 1 regardless of whether the write is atomic or non-atomic. 
 ```solidity 
	// stupid sc, write a 1 to rt
	if (opcode == 0x38 && rtReg != 0) {
		state.registers[rtReg] = 1;
	}
```
Since the VM is likely to not be implementing concurrency, the only impact of this is that it deviates from the MIPS specification, as the offchain MIPS VM has the same bug. 

## [QA-04] MIPS: `state.step` can overflow if there are more than `2^64` MIPS instructions

`state.step` is incremented every MIPS instruction executed, it is a `uint64` variable so it can overflow if there are more than `2^64` MIPS instructions when `MAX_GAME_DEPTH - SPLIT_DEPTH - 2 = 64`

```solidity
	state.step += 1;
```

## [QA-05] MIPS: `_storeReg` should be zeroed for `mult`, `multu`, `div`, `divu`

The `mult`, `multu`, `div`, `divu` instructions are only supposed to write to `HI`, `LO` special registers. However, since they are R-type instructions, they can still have a non-zero `rd` value, though this would be an invalid instruction and lead to undefined behaviour. The MIPS VM do not zero out the `rd` value if the `rd` value for the `mult`, `multu`, `div`, `divu` instructions.

Therefore if the `rd` value is non-zero, the register referenced by `rd` which is passed into `_storeReg` variable will be overwritten by `val` which will be 0, as seen in the snippet below.

```solidity
	// Store the result in the destination register, if applicable
	if (_storeReg != 0) {
		state.registers[_storeReg] = val;
	}
```

Since this is undefined behaviour, it does not contradict any MIPS spec and the onchain and offchain MIPS VM have the same behaviour but it might probably be best to resolve this.

## [QA-06] MIPS: The wrong variable name `v1` is used to denote register 7 instead of `a3`

In `handleSyscall` the variable `v1` is used to refer to register 7 which is where the error code of the syscall is stored. However, the correct variable name of register 7 is `a3`. 

```solidity
	// Write the results back to the state registers
	state.registers[2] = v0;
	state.registers[7] = v1;
```
## [QA-07] MIPS: Redundant assignment of `rdReg = rtReg`

The code pointed by the arrow is redundant because `rdReg` was assigned `rtReg` earlier in the function and has not been changed in the function flow.

```solidity
	// R-type or I-type (stores rt)
	rs = state.registers[(insn >> 21) & 0x1F];
	uint32 rdReg = rtReg; 

	if (opcode == 0 || opcode == 0x1c) { 
	...
	} else if (opcode >= 0x28 || opcode == 0x22 || opcode == 0x26) {
		// store rt value with store
		rt = state.registers[rtReg];

		// store actual rt with lwl and lwr
=>              rdReg = rtReg; // REDUNDANT
	}
```

## [QA-08] MIPS: `(uint32) & 0xffFFffFF` bit-masking is redundant

`execute` will return a `uint32` value. Therefore, performing `(uint32) & 0xffFFffFF` is redundant. The comment is also wrong and the bit-masking is not required.
```solidity
	// ALU
	uint32 val = execute(insn, rs, rt, mem) & 0xffFFffFF; // swr outputs more than 4 bytes without the mask
```
The offchain MIPS VM also does not have this redundant bitmask.

## [QA-09] PREIMAGE: `read` syscalls that read EOF might not be able to be executed via `step()` if `op-program` is compiled to MIPS in a certain way

A common way to detect an EOF is to perform a `read` syscall until the return value (the count of the number of bytes to read) is 0. When this happens programs know that the end-of-file has been reached and will stop calling the `read()` syscall

We believe that the op-program when compiled to MIPS will instead read the first 8 bytes of the preimage to determine the length and then perform the exact number of `read` syscalls to obtain the correct number of bytes from the preimage data. But because we don't actually know how it will be compiled, we are submitting this just in case it actually does the former.

If the MIPS program uses the former method to read the preimage data, the `read` syscall will revert because the part offset will be out of bounds and therefore cannot be inserted into the preimage oracle. Thus, this line will always revert because the `_offset` cannot be inserted into the preimage oracle because of OOB error.
```solidity
	require(preimagePartOk[_key][_offset], "pre-image must exist");
```
For instance, consider a preimage of length 100 (92 bytes of data and 8 bytes to store the length). To read the entire preimage you will need 25 syscalls to read the entire preimage, and 1 syscall to detect the EOF by returning 0.

The MIPS `read` syscall that returns 0 will have a `_partOffset` of 100, but this can't be inserted into the preimage oracle because in many of the functions, OOB error will occur because the `_partOffset` will be 100 which will equal to `size + 8` which is 100 and the function will revert.
```solidity
	// revert if part offset >= size+8 (i.e. parts must be within bounds)
	if iszero(lt(_partOffset, add(size, 8))) {
		// Store "PartOffsetOOB()"
		mstore(0x00, 0xfe254987)
		// Revert with "PartOffsetOOB()"
		revert(0x1c, 0x04)
	}
```

## [QA-10] PREIMAGE: Non-zero bytes can end up in `preimageParts` after the tail end of the preimage of an LPP

In `_extractPreimagePart`, it is possible that non-zero bytes can be stored in the `proposalParts` and then the `preimageParts` because `calldataload` is used to load the next 32 bytes from the offset which will may overshoot the end of the `_input` calldata. Someone can craft calldata such that there can be non-zero bytes in the padding between the `_input` calldata and the `_stateCommitments` to store it in the `preimagePart`
```solidity
	bytes32 preimagePart;
	assembly {
		preimagePart := calldataload(add(_input.offset, relativeOffset))
	}
	proposalParts[msg.sender][_uuid] = preimagePart;
```

This is not exploitable because the MIPS VM will use the `preimageLengths` to only read bytes up to the tail end of the preimage but it might be good defense-in-depth to fix this.

## [QA-11] PREIMAGE: Non-zero bytes can end up in `preimageParts` after the tail end of the part from `loadPrecompilePreimagePart`

Since the `ptr` is reused, it is possible that non-zero bytes can end up after the tail end of the part if the `input` is larger than the `returndata`
```solidity
	// Reuse the `ptr` to store the preimage part: <sizePrefix ++ precompileStatus ++ returrnData>
	// put size as big-endian uint64 at start of pre-image
	mstore(ptr, shl(192, size))
	ptr := add(ptr, 0x08)

	// write precompile result status to the first byte of `ptr`
	mstore8(ptr, res)
	// write precompile return data to the rest of `ptr`
	returndatacopy(add(ptr, 0x01), 0x0, returndatasize())

	// compute part given ofset
	part := mload(add(sub(ptr, 0x08), _partOffset))
```

This is also non-exploitable for the same reason as above.

## [QA-12] PREIMAGE: Wrong comparison operator in `relativeOffset` may cause unintentional reverts

In the below snippet, the case `relativeOffset + 32 == _input.length` is fine because only calldata up to `relativeOffset + 31` will be loaded. This can cause unnecessary reverts when adding leaves in chunks that lead to equality in the below condition, but it is always possible to add them s.t. the last leaf in the call that would have failed is accompanied by the next one, and then the call succeeds.
```solidity
	// Revert if the full preimage part is not available in the data we're absorbing. The submitter must
	// supply data that contains the full preimage part so that no partial preimage parts are stored in the
	// oracle. Partial parts are *only* allowed at the tail end of the preimage, where no more data is available
	// to be absorbed.
	if (relativeOffset + 32 >= _input.length && !_finalize) revert PartOffsetOOB();
```

## [QA-13] PREIMAGE: `.length` should be used instead of `calldataload` to determine `size`

For both `loadKeccak256PreimagePart` and `loadSha256PreimagePart`, it is better to use `.length` instead of using `calldataload` at a fixed offset to determine the length of calldata, because calldata offsets can be manipulated.
```solidity
            size := calldataload(0x44)
```

## [QA-14] PREIMAGE: It is possible to call `addLeavesLPP()` with 0-length `_input`

There is no zero value check on the length of `_input` in `addLeavesLPP()`. The only side effects this has is that:
- If `offset < 8 && currentSize == 0`, the wrong preimage part will be written to `proposalParts`, but this will be overwritten in the next call actually passing the input.
- The block number is added to `proposalBlocks`.

## [QA-15] PREIMAGE: Absorption and permutation are redundant in `squeezeLPP`
The `squeezeLPP` function performs unnecessary absorption and permutation operations on the state matrix. These operations are redundant because the state commitments already contain this information. The function could be simplified by directly using the post-state matrix.

Instead of absorbing and permuting the input bytes, the function should take the state matrix after absorption of the post-state as an input. It should then verify this matrix against the `_postState.stateCommitment` before performing the squeeze operation. 

As per the inline comments:

> We perform no final verification on the state matrix here, since the proposal has passed the challenge period and is considered valid.

So this also goes for the final state commitment which could be used directly.
```solidity
        LibKeccak.absorb(_stateMatrix, _postState.input);
        LibKeccak.permutation(_stateMatrix);
        bytes32 finalDigest = LibKeccak.squeeze(_stateMatrix);
```

## [QA-16] PREIMAGE: `addLeavesLPP` not required to `finalize` on last leaf

When the last leaf is added, it is not actually required to set `_finalize` to true. This only works if `claimedSize % BLOCK_SIZE_BYTES == 0`, as otherwise the input will not be padded and the following check will fail:

```solidity
            if or(mod(inputLen, 136), iszero(eq(_stateCommitments.length, div(inputLen, 136)))) {
```

For the call to succeed, one must pass all but the last state commitment, which corresponds to the empty block which is added as padding at the end in the case `_input % BLOCK_SIZE_BYTES == 0`.

The LPP can still be finalized later by passing no input and the last state commitment.

## [QA-17] PREIMAGE: Off-by-one error in `readPreimage`

In `readPreimage`, when `_offset + 32 == length + 8` then `datLen_` will be 32 and we don't need to enter the `if` statement but still do because of the `>=` comparison operator.
```solidity
  datLen_ = 32;
  if (_offset + 32 >= length + 8) {
      datLen_ = length + 8 - _offset;
  }
```

## [QA-18] PREIMAGE: Return type of `partOffset_` in `CannonTypes` should be `uint32`

The return type of `partOffset_` in `CannonTypes` should be `uint32`.
```solidity
    function partOffset(LPPMetaData _self) internal pure returns (uint64 partOffset_) {
        assembly {
            partOffset_ := and(shr(160, _self), U32_MASK)
        }
    }
```

## [QA-19] PREIMAGE: If a keccak256 preimage is larger than the max size of an LPP, a valid state referencing it cannot be proven

Keccak256 hashes of data larger than `136 * MAX_LEAF_COUNT` bytes are possible on L2s with a higher block gas limit.

We think only hashes originated on L1 will need to be provided in LPPs, hence we are only submitting this as QA. However, if it is necessary to provide such a preimage, the `MAX_LEAF_COUNT` in the current implementation of LPPs will not be sufficient. It will not be possible to use `loadKeccak256PreimagePart()` either due to exceeding calldata limitation and block size limit, which could potentially prevent a valid state form being proven.

## [QA-20] PREIMAGE: If a sha256 preimage extremely large, a valid state consisting it cannot be proven

We don't know how large a sha256 preimage can be, but if its extremely large, it cannot be added to the oracle via the `loadSha256PreimagePart` due to exceeding block gas limits and the lack of a streaming option for it, and a valid state potentially cannot be proven.

## [QA-21] FDG: LPP cost may not cover bonds at `MAX_GAME_DEPTH` when gas is expensive.

The bond for a `MAX_GAME_DEPTH` claim is fixed at `300_000_000 * 200 Gwei = 60 ETH` which is meant to cover double the estimated cost of a passing a maximum size LPP. However, the gas price is actually dynamic and dependent on factors such as network demand for gas (EIP-1559).

According to our calculations, the costs of submitting a max-size LPP can be optimized to slightly below `100_000_000` gas. Hence the estimated gas cost will generally be well above the expected cost, as well as the 200 gas price being a very generous estimate.

Nevertheless, it is technically possible that the actual gas costs surpass the bond if the gas price rises above 600 gwei. In that case, the total cost for passing a maximum size LPP will exceed the bond reimbursed to the user, leading to less incentives to pass the LPP. Consider adapting the bond based on the current `tx.gasprice` at the time a move at `MAX_GAME_DEPTH - 1` is made.

## [QA-22] FDG: The new `CLOCK_EXTENSION` feature can be abused to extend the dispute game above 7 days.

The `CLOCK_EXTENSION` feature is a newly added feature that extends the remaining time left of a team to 3 hours if they have less than 3 hours left on the clock. 

```solidity
        // If the remaining clock time has less than `CLOCK_EXTENSION` seconds remaining, grant the potential
        // grandchild's clock `CLOCK_EXTENSION` seconds. This is to ensure that, even if a player has to inherit another
        // team's clock to counter a freeloader claim, they will always have enough time to to respond. This extension
        // is bounded by the depth of the tree. If the potential grandchild is an execution trace bisection root, the
        // clock extension is doubled. This is to allow for extra time for the off-chain challenge agent to generate
        // the initial instruction trace on the native FPVM.
        if (nextDuration.raw() > MAX_CLOCK_DURATION.raw() - CLOCK_EXTENSION.raw()) {
            // If the potential grandchild is an execution trace bisection root, double the clock extension.
            uint64 extensionPeriod =
                nextPositionDepth == SPLIT_DEPTH - 1 ? CLOCK_EXTENSION.raw() * 2 : CLOCK_EXTENSION.raw();
            nextDuration = Duration.wrap(MAX_CLOCK_DURATION.raw() - extensionPeriod);
        }
```
It is possible for an attacker to always extend a game by creating a long chain of games in the dispute game and making a move below the 3 hour duration in order to extend it, although they will likely lose their bonds for doing so. This is bounded by the `MAX_GAME_DEPTH` which is 73 on mainnet, and therefore the maximum time the game can be extended by will be 73 * 3 hours = 219 hours = ~9 days.

Although this is intentional as acknowledged by the NatSpec, it is still a possible attack and so that is why we filed it here.

## [QA-23] FDG: Honest challengers can lose their bonds when proposing valid root claims that will become invalid in the event of chain reorgs.

In the event of a chain reorg, valid root claims can become invalid because the L2 state and correct output can change. The honest challenger can lose their bonds as a result of this because `DisputeGameFactory.create()` does not have any protections against this.
