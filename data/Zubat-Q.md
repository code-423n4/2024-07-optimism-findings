# SRAV Opcode logic is not correct

## Links to affected code

https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L855-L858

## Vulnerability details


### Impact

The SRAV opcode logic in `MIPS.sol` is as follows:

```solidity
// srav
else if (func == 0x07) {
    return SE(rt >> rs, 32 - rs);
}
```

Notice that the `rt` value (i.e. the 32 bit value from the `rt` register) is shifted the number of bits indicated by the full `rs` value (i.e. the 32 bit value from the `rs` register).

According to the [MIPS32 spec](https://s3-eu-west-1.amazonaws.com/downloads-mips/documents/MD00086-2B-MIPS32BIS-AFP-6.06.pdf), the shift amount for SRAV should only be based on the lowest 5 bits of the `rs` register. This means, for example, a full shift of 32 bits is impossible with SRAV, but it is indeed possible in the `MIPS.sol` implementation.

Fortunately, this behavior discrepancy does not seem possible to trigger in the op-program code.

### Recommended Mitigation Steps

To make the code more compliant with the MIPS32 spec, consider changing the SRAV logic to the following:

```solidity
// srav
else if (func == 0x07) {
    uint32 shamt = rs & 0x1F;
    return SE(rt >> shamt, 32 - shamt);
}
```

# `PreimageOracle` does not support zero-length requests

## Links to affected code

https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol

## Vulnerability details

### Impact

If the `MIPS.sol` execution trace could ever reach a read syscall that requests a zero-length read, the program could get stuck. To see this, note that all read syscalls result in a call to the `PreimageOracle`:

```solidity
else if (a0 == FD_PREIMAGE_READ) {
    // verify proof 1 is correct, and get the existing memory.
    uint32 mem = readMem(a1 & 0xFFffFFfc, 1); // mask the addr to align it to 4 bytes
    bytes32 preimageKey = state.preimageKey;
    // If the preimage key is a local key, localize it in the context of the caller.
    if (uint8(preimageKey[0]) == 1) {
        preimageKey = PreimageKeyLib.localize(preimageKey, _localContext);
    }
    (bytes32 dat, uint256 datLen) = ORACLE.readPreimage(preimageKey, state.preimageOffset);
    
    // ...
}
```

Also, note that the `PreimageOracle` only returns results that have been stored before:

```solidity
function readPreimage(bytes32 _key, uint256 _offset) external view returns (bytes32 dat_, uint256 datLen_) {
    require(preimagePartOk[_key][_offset], "pre-image must exist");
    // ...
}
```

Finally, notice that most preimage types (including `loadKeccak256PreimagePart()`, `loadSha256PreimagePart()`, `loadBlobPreimagePart()`, `loadPrecompilePreimagePart()`, and `initLPP()`) will revert if the offset implies there is zero data being stored:

```solidity
// revert if part offset >= size+8 (i.e. parts must be within bounds)
if iszero(lt(_partOffset, add(size, 8))) {
    // Store "PartOffsetOOB()"
    mstore(0x00, 0xfe254987)
    // Revert with "PartOffsetOOB()"
    revert(0x1c, 0x04)
}
```


Fortunately, this does not seem to be a major problem, because it appears that the op-program code will never explicitly request a zero-length read due to [this internal Go code](https://github.com/golang/go/blob/8659ad972f0f1fb389397cb35f810d9ccb36539f/src/internal/poll/fd_unix.go#L146-L153):

```go
if len(p) == 0 {
    // If the caller wanted a zero byte read, return immediately
    // without trying (but after acquiring the readLock).
    // Otherwise syscall.Read returns 0, nil which looks like
    // io.EOF.
    // TODO(bradfitz): make it wait for readability? (Issue 15735)
    return 0, nil
}
```



### Recommended Mitigation Steps

In case anything in op-program changes in the future, consider explicitly handling zero-length reads in a way that ensures the program does not stall. For example, consider modifying `MIPS.sol` as follows:

```diff
else if (a0 == FD_PREIMAGE_READ) {
+   if (a2 != 0) {
        // verify proof 1 is correct, and get the existing memory.
        uint32 mem = readMem(a1 & 0xFFffFFfc, 1); // mask the addr to align it to 4 bytes
        bytes32 preimageKey = state.preimageKey;
        // If the preimage key is a local key, localize it in the context of the caller.
        if (uint8(preimageKey[0]) == 1) {
            preimageKey = PreimageKeyLib.localize(preimageKey, _localContext);
        }
        (bytes32 dat, uint256 datLen) = ORACLE.readPreimage(preimageKey, state.preimageOffset);

        // Transform data for writing to memory
        // We use assembly for more precise ops, and no var count limit
        assembly {
            let alignment := and(a1, 3) // the read might not start at an aligned address
            let space := sub(4, alignment) // remaining space in memory word
            if lt(space, datLen) { datLen := space } // if less space than data, shorten data
            if lt(a2, datLen) { datLen := a2 } // if requested to read less, read less
            dat := shr(sub(256, mul(datLen, 8)), dat) // right-align data
            dat := shl(mul(sub(sub(4, datLen), alignment), 8), dat) // position data to insert into memory
                // word
            let mask := sub(shl(mul(sub(4, alignment), 8), 1), 1) // mask all bytes after start
            let suffixMask := sub(shl(mul(sub(sub(4, alignment), datLen), 8), 1), 1) // mask of all bytes
                // starting from end, maybe none
            mask := and(mask, not(suffixMask)) // reduce mask to just cover the data we insert
            mem := or(and(mem, not(mask)), dat) // clear masked part of original memory, and insert data
        }

        // Write memory back
        writeMem(a1 & 0xFFffFFfc, 1, mem);
        state.preimageOffset += uint32(datLen);
        v0 = uint32(datLen);
+   }
}
```

# `loadPrecompilePreimagePart()` can preload data for precompiles that don't exist yet

## Links to affected code

https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L335-L389

## Vulnerability details

### Impact

The `loadPrecompilePreimagePart()` function allows arbitrary addresses for the precompile call. It's possible that future versions of the op-program codebase could request results from precompiles that currently do not exist. Since nothing is preventing these precompiles from being called today (which would result in a successful call with no return data, since it'd currently be equivalent to calling an EOA), incorrect results could be stored now to be used in the future.

### Recommended Mitigation Steps

Ensure that the current `PreimageOracle` is not used in future versions of the codebase that use new precompile addresses that currently don't exist. Additionally, consider adding a whitelist to `loadPrecompilePreimagePart()` so that it can only store results for precompile addresses that are intended to be used.

# Incorrect revert offset in `MIPS.sol`

## Links to affected code

https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L573-L577

## Vulnerability details

### Impact

In `MIPS.sol`, the following check exists in `readMem()`:


```solidity
// Verify the root matches.
if iszero(eq(node, memRoot)) {
    mstore(0, 0x0badf00d)
    revert(0, 32)
}
```

Since the error message is only 4 bytes and placed in bytes 28-32, it is likely not intended to revert with 32 bytes of data starting at offset 0.

### Recommended Mitigation Steps

Change the revert offset to the following:

```solidity
// Verify the root matches.
if iszero(eq(node, memRoot)) {
    mstore(0, 0x0badf00d)
    revert(0x1c, 0x04)
}
```

# `PreimageOracle` can store dirty bytes in `preimageParts`

## Links to affected code

https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L335-L389

https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L747-L749

## Vulnerability details


### Impact

In the `PreimageOracle` contract, it's possible that the preimage part offset results in a "partial read", i.e. an offset with less than 32 bytes in the remaining preimage. In most scenarios, there is code that intentionally pads these values with trailing zero bytes, for example:

```solidity
// Clean the word at `ptr + 0x28` to ensure that data out of bounds of the preimage is zero, if the part
// offset requires a partial read.
mstore(add(ptr, 0x28), 0x00)
```

However, there are two locations where this zero padding is not guaranteed and dirty bytes can be stored. These two locations are:

1. In the `loadPrecompilePreimagePart()` function. Since this function overwrites the precompile input with the precompile output, there can be dirty bytes if the input length is larger than the output length. For example, consider the following test that can be added to `packages/contracts-bedrock/test/cannon/PreimageOracle.t.sol`:

    ```solidity
    function test_loadPrecompilePreimagePart_dirty_bytes_bug() public {
        address precompile = address(bytes20(uint160(0x02))); // sha256

        // This input is 64 bytes, which is large enough to trigger the issue
        bytes memory input = abi.encodePacked(keccak256("b"), keccak256("c"));

        // Will use offset 10, which will read 1 byte into the incorrectly dirty memory
        uint256 offset = 10;
        bytes32 key = precompilePreimageKey(precompile, input);
        oracle.loadPrecompilePreimagePart(offset, precompile, input);
        bytes32 part = oracle.preimageParts(key, offset);

        // The first 31 bytes correctly match the last 31 bytes of sha256(input)
        assertEq(
            sha256(input) & 0x00ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff,
            part >> 8
        );

        // This shows the issue. The last byte should be 0 padding, but instead it's just the
        // dirty memory left over from the input. Specifically at input index 
        // 8 (size) + 1 (result) + 32 (output) - 20 (offset where input is stored) 
        assertTrue(uint8(uint256(part)) != 0);
        assertEq(uint8(uint256(part)), uint8(input[8 + 1 + 32 - 20]));
    }
    ```
    
2. In the `addLeavesLPP()` function. Since the `_extractPreimagePart()` helper function can read past the end of the `_input` bytes when finalizing, there can be dirty bytes that reach into the `_stateCommitments` array. For example, consider the following test which can also be added to `packages/contracts-bedrock/test/cannon/PreimageOracle.t.sol`:

    ```solidity
    function test_addLeaves_dirty_bytes_bug() public {
        // We need the stateCommitments length to be at least 2 bytes (i.e. its length needs
        // to be at least 256). Also for the dirty memory read to work, we want the length of
        // data to be divisible by 32. So using 136 * 257 + 24 works well here.
        bytes memory data = new bytes(136 * 257 + 24);
        for (uint256 i; i < data.length; i++) data[i] = 0xFF;
        uint32 offset = uint32(data.length + 8 - 1);

        oracle.initLPP{ value: oracle.MIN_BOND_SIZE() }(TEST_UUID, offset, uint32(data.length));

        LibKeccak.StateMatrix memory stateMatrix;
        bytes32[] memory stateCommitments = _generateStateCommitments(stateMatrix, data);
        assertEq(stateCommitments.length, 258);

        oracle.addLeavesLPP(TEST_UUID, 0, data, stateCommitments, true);

        // Notice that the value saved into `proposalParts` is not padded with zeros at the end
        bytes32 partPaddedWithZero = bytes32(abi.encodePacked(data[data.length-1], bytes31(0)));
        assertFalse(oracle.proposalParts(address(this), TEST_UUID) == partPaddedWithZero);

        // Instead, it's padded with zeros followed by a dirty 1 byte (from the length of the state commitments)
        bytes32 partPaddedWithDirtyByte = bytes32(abi.encodePacked(data[data.length-1], bytes30(0), uint8(1)));
        assertTrue(oracle.proposalParts(address(this), TEST_UUID) == partPaddedWithDirtyByte);
    }
    ```

Fortunately, this does not seem exploitable, as `MIPS.sol` will always mask out unnecessary bytes from the `PreimageOracle` before it stores them in its internal memory.

### Recommended Mitigation Steps

Consider if zero padding in the `preimageParts` is important enough to warrant a change. If not, consider documenting this behavior in the code. If so, consider adding extra zero padding logic in both functions mentioned above.


# Potential Incompatibility with Future Versioned SHA256 Hash

## Links to affected code
https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L235-L236
## Vulnerability details
### Impact
The Sha256Key is solely used for retrieving the Blob KZG commitment for a specific blob hash. The current blob hash is a versioned SHA256 hash, with the first byte replaced with '\x01' to ensure forward compatibility. However, this "version" byte is overwritten by the "preimage type" byte Sha256KeyType. It's unclear whether this collision will cause future confusion or incompatibility.
### Recommended Mitigation Steps
Ensure that the current Sha256Key is only used for SHA256 Hash with version '\x01'.

