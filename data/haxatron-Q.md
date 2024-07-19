## [L-01]: The new `CLOCK_EXTENSION` feature can be abused to extend the dispute game above 7 days.

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
