## [MIPS-01]: The MIPS VM do not handle overflow exceptions for signed arithmetic `add`, `addi`, `sub` instructions which deviates from MIPS spec.

For the `add`, `addi`, `sub` instructions which perform signed arithmetic, the MIPS  VM do not handle signed overflows appropriately. A signed overflow occurs when:

1) The sign bit of the operands are the same AND
2) The sign bit of the result is different from the operands

[MIPS.sol#L918-L922](https://github.com/code-423n4/2024-07-optimism/blob/main/packages/contracts-bedrock/src/cannon/MIPS.sol#L918-L922)
```solidity
                // add
                else if (func == 0x20) {
                    return (rs + rt);
                }
```
[MIPS.sol#L927-L930](https://github.com/code-423n4/2024-07-optimism/blob/main/packages/contracts-bedrock/src/cannon/MIPS.sol#L927-L930)
```solidity
                // sub
                else if (func == 0x22) {
                    return (rs - rt);
                }
```
As seen in the above snippets, the MIPS VM performs unsigned arithmetic for signed arithmetic instructions where the overflow does not lead to an exception. For example, consider the following operation: 0b0100 + 0b0100,

Performing unsigned addition will lead to a result 0b1000, but according to MIPS specification - please check the relevant information on the MIPS ISA opcodes in [link](https://www.cs.cmu.edu/afs/cs/academic/class/15740-f97/public/doc/mips-isa.pdf), the signed addition will throw an exception for this result because the sign bit is 0 for both operands while the sign bit is 1 for the result. The only impact of this is that it deviates from the MIPS specification as the offchain MIPS VM has the same bug. 

## [MIPS-02]: `_storeReg` should be zeroed for `mult`, `multu`, `div`, `divu`

The `mult`, `multu`, `div`, `divu` instructions are only supposed to write to `HI`, `LO` special registers. However, since they are R-type instructions, they can still have a non-zero `rd` value, though this would be an invalid instruction. The MIPS VM do not zero out the `rd` value if the `rd` value for the `mult`, `multu`, `div`, `divu` instructions.

Therefore if the `rd` value is non-zero, the register referenced by `rd` which is passed into `_storeReg` variable will be overwritten by `val` which will be 0, as seen in the snippet below.

[MIPS.sol#L442-L445](https://github.com/code-423n4/2024-07-optimism/blob/main/packages/contracts-bedrock/src/cannon/MIPS.sol#L442-L445)
```solidity
            // Store the result in the destination register, if applicable
            if (_storeReg != 0) {
                state.registers[_storeReg] = val;
            }
```

Since this would be an invalid instruction, the only impact of this is that it deviates from the MIPS specification as the offchain MIPS VM has the same bug. 

## [MIPS-03] Redundant assignment of `rdReg = rtReg`

The code pointed by the arrow is redundant because `rdReg` was assigned `rtReg` earlier in the function and has not been changed in the function flow.

[MIPS.sol#L738](https://github.com/code-423n4/2024-07-optimism/blob/main/packages/contracts-bedrock/src/cannon/MIPS.sol#L738)
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

## [MIPS-04]: `(uint32) & 0xffFFffFF` bit-masking is redundant

`execute` will return a `uint32` value. Therefore, performing `(uint32) & 0xffFFffFF` is redundant. The comment is also wrong and the bit-masking is not required.

[MIPS.sol#L762-L763](https://github.com/code-423n4/2024-07-optimism/blob/main/packages/contracts-bedrock/src/cannon/MIPS.sol#L762-L763)
```solidity
            // ALU
            uint32 val = execute(insn, rs, rt, mem) & 0xffFFffFF; // swr outputs 
more than 4 bytes without the mask 
```

## [FDG-01]: The new `CLOCK_EXTENSION` feature can be abused to extend the dispute game above 7 days.

The `CLOCK_EXTENSION` feature is a newly added feature that extends the remaining time left of a team to 3 hours if they have less than 3 hours left on the clock. 

[FaultDisputeGame.sol#L371-L382](https://github.com/code-423n4/2024-07-optimism/blob/main/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L371-L382)
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
It is possible for an attacker to always extend a game by creating a long chain of games in the dispute game and making a move below the 3 hour duration in order to extend it, although they will likely lose their bonds for doing so. This is bounded by the `MAX_DEPTH` which is 73 on mainnet, and therefore the maximum time the game can be extended by will be 73 * 3 hours = 219 hours = ~9 days.

Although this is intentional as acknowledged by the NatSpec, it is still a possible attack and so that is why I filed it here.
