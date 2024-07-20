## [QA-01]: The MIPS VM do not handle overflow exceptions for signed arithmetic `add`, `addi`, `sub` instructions which deviates from MIPS spec.

For the `add`, `addi`, `sub` instructions which perform signed arithmetic, the MIPS  VM do not handle signed overflows appropriately. A signed overflow occurs when:

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
As seen in the above snippets, the MIPS VM performs unsigned arithmetic for signed arithmetic instructions where the overflow does not lead to an exception. For example, consider the following: 0b0100 + 0b0100,

Performing unsigned addition will lead to a result 0b1000, but according to MIPS specification - please check the relevant information on the MIPS ISA opcodes in [link](https://www.cs.cmu.edu/afs/cs/academic/class/15740-f97/public/doc/mips-isa.pdf), the signed addition will throw an exception for this result because the sign bit is 0 for both operands while the sign bit is 1 for the result. The only impact of this is that it deviates from the MIPS specification as the offchain MIPS VM has the same deviation.

## [QA-02]: The new `CLOCK_EXTENSION` feature can be abused to extend the dispute game above 7 days.

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
It is possible for an attacker to always extend a game by creating a long chain of games in the dispute game and making a move below the 3 hour duration in order to extend it, although they will likely lose their bonds for doing so. This is bounded by the `MAX_DEPTH` which is 73 on mainnet, and therefore the maximum time the game can be extended by will be 73 * 3 hours = 219 hours = ~9 days.

Although this is intentional as acknowledged by the NatSpec, it is still a possible attack and so that is why I filed it here.
