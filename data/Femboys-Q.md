`PreimageOracle#loadLocalData` doesn't enforce that the size of the claimed data doesn't change between two calls, which seems like it breaks an invariant.

https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L120-L157

`PreimageOracle#loadKeccak256PreimagePart` and `PreimageOracle#loadSha256PreimagePart` assume that the length of the payload will be located at offset `0x44`, but this is only true if the caller uses standard ABI encoding. If a caller manipulates calldata, they might be able to cause an offchain observer to miscompute the key that was loaded.
https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L166
https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L201

The MIPS specification states that a division operation should not result in an arithmetic exception, but the MIPS emulator reverts if the divisor is zero.
https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L426
https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L436

When compiled for MIPS, `op-program` contains the `rdhwr` and `teq` instructions, although they are not executed (`rdhwr` is run if cgo is enabled, `teq` is used as an unreachable instruction). However, it might still be worth explicitly implementing them as no-ops.