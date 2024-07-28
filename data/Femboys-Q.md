`PreimageOracle#loadLocalData` doesn't enforce that the size of the claimed data doesn't change between two calls, which seems like it breaks an invariant.

https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L120-L157

