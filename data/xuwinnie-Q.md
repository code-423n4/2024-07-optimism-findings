### 1
The required bond increases with depth, which causes *proof of whale*, if we made it decreases with depth instead, the problem can be partially mitigated. Honest party only needs one root claim, then defending will cost less than attacking.

### 2
For spearbit issue 5.1.4, we can fix it by making block number challenger the lowest priority. The honest party only challenges block number when the root is correct.

### 3
Each WETH.unlock call resets the timestamp for claimant's full amount, instead of only the newly added amount.

### 4
Gas consumption for two parties are unbalanced, depending on the parity of max depth, challenger or defender will always need to load preimage and execute step function, which costs much more gas than their opponent does.