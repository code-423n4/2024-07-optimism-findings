### 1
The required bond increases with depth, which causes *proof of whale*, if we made it decreases with depth instead, the problem can be partially mitigated. Honest party only needs one root claim, then defending will cost less than attacking.

### 2
For spearbit issue 5.1.4, we can fix it by making block number challenger the lowest priority. The honest party only challenges block number when the root is correct.

### 3
Each WETH.unlock call resets the timestamp for claimant's full amount, instead of only the newly added amount.

    function unlock(address _guy, uint256 _wad) external {
        WithdrawalRequest storage wd = withdrawals[msg.sender][_guy];
        wd.timestamp = block.timestamp;
        wd.amount += _wad;
    }

### 4
Gas consumption for two parties are unbalanced, depending on the parity of max depth, challenger or defender will always need to load preimage and execute step function, which costs much more gas than their opponent does.

### 5
>If the potential grandchild is an execution trace bisection root claim and their clock has less than CLOCK_EXTENSION seconds remaining, exactly CLOCK_EXTENSION * 2 seconds are allocated for the potential grandchild. This extra time is alloted to allow for completion of the off-chain FPVM run to generate the initial instruction trace.

However, if the clock has more than CLOCK_EXTENSION but less than CLOCK_EXTENSION * 2 seconds remaining, no time will be allocated. So CLOCK_EXTENSION * 2 seconds cannot always be guaranteed for off-chain trace generation.

        if (nextDuration.raw() > MAX_CLOCK_DURATION.raw() - CLOCK_EXTENSION.raw()) {
            // If the potential grandchild is an execution trace bisection root, double the clock extension.
            uint64 extensionPeriod =
                nextPositionDepth == SPLIT_DEPTH - 1 ? CLOCK_EXTENSION.raw() * 2 : CLOCK_EXTENSION.raw();
            nextDuration = Duration.wrap(MAX_CLOCK_DURATION.raw() - extensionPeriod);
        }