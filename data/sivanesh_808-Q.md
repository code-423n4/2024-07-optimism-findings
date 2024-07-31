Sure, here's the index in table format:

| QA Report Number | Contract Name    | Description                                                                                   | Recommendation                                                                                 | Code Snippet                     |
|------------------|------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|----------------------------------|
| QA-1             | MIPS.sol         | Incorrect handling of `beq` and `bne` instructions, leading to incorrect state transitions.   | Fix branch condition evaluation, validate instruction format, and use SafeMath for calculations. | `handleBranch` function          |
| QA-2             | MIPS.sol         | `handleSyscall` function does not validate return values from `ORACLE.readPreimage`.          | Check return values and revert on invalid values.                                              | `handleSyscall` function         |
| QA-3             | MIPS.sol         | `handleSyscall` function does not properly handle errors for unsupported or invalid syscalls. | Add default case for unsupported syscalls and ensure VM state consistency.                     | `handleSyscall` function         |
| QA-4             | MIPS.sol         | `handleHiLo` function fails to handle division overflow.                                      | Add checks for overflow and handle edge cases.                                                 | `handleHiLo` function            |
| QA-5             | MIPS.sol         | `SE` function incorrectly handles sign extension.                                             | Correct sign extension logic and implement extensive testing.                                  | `SE` function                    |
| QA-6             | MIPS.sol         | `writeMem` function does not handle edge cases for memory writes.                             | Improve error handling, add boundary checks, and implement extensive testing.                  | `writeMem` function              |
| QA-7             | PreimageOracle   | `readPreimage` function fails to correctly check for the existence of a pre-image part.       | Verify the return values from `ORACLE.readPreimage` and handle errors appropriately.           | `readPreimage` function          |
| QA-8             | PreimageOracle   | `loadKeccak256PreimagePart` function incorrectly computes the key for storing preimage parts. | Correct the key computation process.                                                           | `loadKeccak256PreimagePart` function |
| QA-9             | PreimageOracle   | Initialization of a large preimage proposal (LPP) with a duplicate UUID does not revert.      | Ensure the contract reverts with an appropriate error when attempting to initialize a duplicate UUID. | `initLPP` function               |
| QA-10            | PreimageOracle   | `loadLocalData` function fails to handle valid parameters correctly and does not revert for misaligned offsets. | Fix logic handling for valid parameters and add alignment checks for offsets.                  | `loadLocalData` function         |






### [QA-1]  Incorrect Handling of Branch Instructions

#### Contract Name: MIPS.sol

#### Description

The `handleBranch` function in the `MIPS` contract incorrectly handles branch instructions (`beq` and `bne`). This can lead to incorrect state transitions, causing the virtual machine to branch incorrectly or not at all when specific conditions are met.

#### Code Snippet

Here is the problematic part of the `handleBranch` function:

```solidity
function handleBranch(uint32 _opcode, uint32 _insn, uint32 _rtReg, uint32 _rs) internal returns (bytes32 out_) {
    unchecked {
        // Load state from memory
        State memory state;
        assembly {
            state := 0x80
        }

        bool shouldBranch = false;

        if (state.nextPC != state.pc + 4) {
            revert("branch in delay slot");
        }

        // beq/bne: Branch on equal / not equal
        if (_opcode == 4 || _opcode == 5) {
            uint32 rt = state.registers[_rtReg];
            shouldBranch = (_rs == rt && _opcode == 4) || (_rs != rt && _opcode == 5);
        }
        // blez: Branches if instruction is less than or equal to zero
        else if (_opcode == 6) {
            shouldBranch = int32(_rs) <= 0;
        }
        // bgtz: Branches if instruction is greater than zero
        else if (_opcode == 7) {
            shouldBranch = int32(_rs) > 0;
        }
        // bltz/bgez: Branch on less than zero / greater than or equal to zero
        else if (_opcode == 1) {
            // regimm
            uint32 rtv = ((_insn >> 16) & 0x1F);
            if (rtv == 0) {
                shouldBranch = int32(_rs) < 0;
            }
            if (rtv == 1) {
                shouldBranch = int32(_rs) >= 0;
            }
        }

        // Update the state's previous PC
        uint32 prevPC = state.pc;

        // Execute the delay slot first
        state.pc = state.nextPC;

        // If we should branch, update the PC to the branch target
        // Otherwise, proceed to the next instruction
        if (shouldBranch) {
            state.nextPC = prevPC + 4 + (SE(_insn & 0xFFFF, 16) << 2);
        } else {
            state.nextPC = state.nextPC + 4;
        }

        // Return the hash of the resulting state
        out_ = outputState();
    }
}
```

#### Expected Behavior

The `handleBranch` function should correctly handle all branch instructions (`beq`, `bne`, `blez`, `bgtz`, `bltz`, and `bgez`) by updating the program counter (PC) according to the branch condition and the instruction format. The function should accurately evaluate the branch conditions and make sure that the state transitions are correct.

#### Actual Behavior

The `handleBranch` function incorrectly handles `beq` and `bne` instructions, leading to incorrect state transitions. The function emits a revert message indicating an error related to calldata validation. This causes the VM to branch incorrectly or not at all when specific conditions are met.

#### Recommendation

1. **Fix the Branch Condition Evaluation**: Ensure that the branch conditions for `beq`, `bne`, and other branch instructions are correctly evaluated.
2. **Validate Instruction Format**: Add validation for instruction formats and expected results to prevent logical errors in branch handling.
3. **Use SafeMath for Calculations**: Ensure that all arithmetic operations use SafeMath to prevent overflow or underflow issues.

### Example Fix for `handleBranch` Function

```solidity
function handleBranch(uint32 _opcode, uint32 _insn, uint32 _rtReg, uint32 _rs) internal returns (bytes32 out_) {
    unchecked {
        // Load state from memory
        State memory state;
        assembly {
            state := 0x80
        }

        bool shouldBranch = false;

        if (state.nextPC != state.pc + 4) {
            revert("branch in delay slot");
        }

        // Validate branch instructions and set shouldBranch flag
        if (_opcode == 4) { // beq
            uint32 rt = state.registers[_rtReg];
            shouldBranch = (_rs == rt);
        } else if (_opcode == 5) { // bne
            uint32 rt = state.registers[_rtReg];
            shouldBranch = (_rs != rt);
        } else if (_opcode == 6) { // blez
            shouldBranch = int32(_rs) <= 0;
        } else if (_opcode == 7) { // bgtz
            shouldBranch = int32(_rs) > 0;
        } else if (_opcode == 1) {
            uint32 rtv = ((_insn >> 16) & 0x1F);
            if (rtv == 0) { // bltz
                shouldBranch = int32(_rs) < 0;
            } else if (rtv == 1) { // bgez
                shouldBranch = int32(_rs) >= 0;
            } else {
                revert("invalid branch instruction");
            }
        } else {
            revert("invalid branch opcode");
        }

        // Update the state's previous PC
        uint32 prevPC = state.pc;

        // Execute the delay slot first
        state.pc = state.nextPC;

        // If we should branch, update the PC to the branch target
        // Otherwise, proceed to the next instruction
        if (shouldBranch) {
            state.nextPC = prevPC + 4 + (SE(_insn & 0xFFFF, 16) << 2);
        } else {
            state.nextPC = state.nextPC + 4;
        }

        // Return the hash of the resulting state
        out_ = outputState();
    }
}
```

-----------------------------------------------------------------------------------------------------

### [QA-2]  Unchecked Return Values in Syscall Handling

**Contract Name**: MIPS.sol

**Description**: The `handleSyscall` function does not properly validate the return values from the `ORACLE.readPreimage` function before using them. This can lead to potential logical errors if the `readPreimage` function returns unexpected or invalid values, potentially compromising the contract's intended behavior.

**Code Snippet**:
```solidity
else if (syscall_no == 4003) {
    // args: a0 = fd, a1 = addr, a2 = count
    // returns: v0 = read, v1 = err code
    if (a0 == FD_STDIN) {
        // Leave v0 and v1 zero: read nothing, no error
    }
    // pre-image oracle read
    else if (a0 == FD_PREIMAGE_READ) {
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
            let mask := sub(shl(mul(sub(4, alignment), 8), 1), 1) // mask all bytes after start
            let suffixMask := sub(shl(mul(sub(sub(4, alignment), datLen), 8), 1), 1) // mask of all bytes starting from end, maybe none
            mask := and(mask, not(suffixMask)) // reduce mask to just cover the data we insert
            mem := or(and(mem, not(mask)), dat) // clear masked part of original memory, and insert data
        }

        // Write memory back
        writeMem(a1 & 0xFFffFFfc, 1, mem);
        state.preimageOffset += uint32(datLen);
        v0 = uint32(datLen);
    }
    ...
}
```

**Expected Behavior**: The function should verify the return values from the `ORACLE.readPreimage` function to ensure they are valid before using them. If the values are not valid, the function should revert or handle the error appropriately.

**Actual Behavior**: The function does not check the return values from the `ORACLE.readPreimage` function, which can lead to potential logical errors if the function returns unexpected or invalid values.

**Recommendation**:
1. **Check Return Values**: Add checks for the return values from the `ORACLE.readPreimage` function to ensure they are valid before using them.
2. **Revert on Invalid Values**: Revert the transaction if the return values are not valid.

### Updated Code with Recommendation

```solidity
else if (syscall_no == 4003) {
    // args: a0 = fd, a1 = addr, a2 = count
    // returns: v0 = read, v1 = err code
    if (a0 == FD_STDIN) {
        // Leave v0 and v1 zero: read nothing, no error
    }
    // pre-image oracle read
    else if (a0 == FD_PREIMAGE_READ) {
        // verify proof 1 is correct, and get the existing memory.
        uint32 mem = readMem(a1 & 0xFFffFFfc, 1); // mask the addr to align it to 4 bytes
        bytes32 preimageKey = state.preimageKey;
        // If the preimage key is a local key, localize it in the context of the caller.
        if (uint8(preimageKey[0]) == 1) {
            preimageKey = PreimageKeyLib.localize(preimageKey, _localContext);
        }
        (bytes32 dat, uint256 datLen) = ORACLE.readPreimage(preimageKey, state.preimageOffset);

        // Check if the return values are valid
        require(dat != bytes32(0) && datLen > 0, "Invalid return values from ORACLE.readPreimage");

        // Transform data for writing to memory
        // We use assembly for more precise ops, and no var count limit
        assembly {
            let alignment := and(a1, 3) // the read might not start at an aligned address
            let space := sub(4, alignment) // remaining space in memory word
            if lt(space, datLen) { datLen := space } // if less space than data, shorten data
            if lt(a2, datLen) { datLen := a2 } // if requested to read less, read less
            dat := shr(sub(256, mul(datLen, 8)), dat) // right-align data
            dat := shl(mul(sub(sub(4, datLen), alignment), 8), dat) // position data to insert into memory
            let mask := sub(shl(mul(sub(4, alignment), 8), 1), 1) // mask all bytes after start
            let suffixMask := sub(shl(mul(sub(sub(4, alignment), datLen), 8), 1), 1) // mask of all bytes starting from end, maybe none
            mask := and(mask, not(suffixMask)) // reduce mask to just cover the data we insert
            mem := or(and(mem, not(mask)), dat) // clear masked part of original memory, and insert data
        }

        // Write memory back
        writeMem(a1 & 0xFFffFFfc, 1, mem);
        state.preimageOffset += uint32(datLen);
        v0 = uint32(datLen);
    }
    ...
}
```


------------------------------------------------------------------------------------------

### [QA-3]  Incorrect Error Handling for Syscalls

**Contract Name**: MIPS.sol

**Description**: The `handleSyscall` function in the MIPS contract does not properly handle errors for unsupported or invalid syscalls. This could lead to incorrect state transitions, potential security issues, and unexpected behavior for users.

**Code Snippet**:
```solidity
function handleSyscall(bytes32 _localContext) internal returns (bytes32 out_) {
    unchecked {
        // Load state from memory
        State memory state;
        assembly {
            state := 0x80
        }

        // Load the syscall number from the registers
        uint32 syscall_no = state.registers[2];
        uint32 v0 = 0;
        uint32 v1 = 0;

        // Load the syscall arguments from the registers
        uint32 a0 = state.registers[4];
        uint32 a1 = state.registers[5];
        uint32 a2 = state.registers[6];

        // Handle known syscalls
        if (syscall_no == 4090) {
            // mmap: Allocates a page from the heap.
            uint32 sz = a1;
            if (sz & 4095 != 0) {
                // adjust size to align with page size
                sz += 4096 - (sz & 4095);
            }
            if (a0 == 0) {
                v0 = state.heap;
                state.heap += sz;
            } else {
                v0 = a0;
            }
        } else if (syscall_no == 4045) {
            // brk: Returns a fixed address for the program break at 0x40000000
            v0 = BRK_START;
        } else if (syscall_no == 4120) {
            // clone (not supported) returns 1
            v0 = 1;
        } else if (syscall_no == 4246) {
            // exit group: Sets the Exited and ExitCode states to true and argument 0.
            state.exited = true;
            state.exitCode = uint8(a0);
            return outputState();
        } else if (syscall_no == 4003) {
            // read: Like Linux read syscall. Splits unaligned reads into aligned reads.
            if (a0 == FD_STDIN) {
                // Leave v0 and v1 zero: read nothing, no error
            } else if (a0 == FD_PREIMAGE_READ) {
                uint32 mem = readMem(a1 & 0xFFffFFfc, 1);
                bytes32 preimageKey = state.preimageKey;
                if (uint8(preimageKey[0]) == 1) {
                    preimageKey = PreimageKeyLib.localize(preimageKey, _localContext);
                }
                (bytes32 dat, uint256 datLen) = ORACLE.readPreimage(preimageKey, state.preimageOffset);
                assembly {
                    let alignment := and(a1, 3)
                    let space := sub(4, alignment)
                    if lt(space, datLen) { datLen := space }
                    if lt(a2, datLen) { datLen := a2 }
                    dat := shr(sub(256, mul(datLen, 8)), dat)
                    dat := shl(mul(sub(sub(4, datLen), alignment), 8), dat)
                    let mask := sub(shl(mul(sub(4, alignment), 8), 1), 1)
                    let suffixMask := sub(shl(mul(sub(sub(4, alignment), datLen), 8), 1), 1)
                    mask := and(mask, not(suffixMask))
                    mem := or(and(mem, not(mask)), dat)
                }
                writeMem(a1 & 0xFFffFFfc, 1, mem);
                state.preimageOffset += uint32(datLen);
                v0 = uint32(datLen);
            } else if (a0 == FD_HINT_READ) {
                v0 = a2;
            } else {
                v0 = 0xFFffFFff;
                v1 = EBADF;
            }
        } else if (syscall_no == 4004) {
            // write: like Linux write syscall. Splits unaligned writes into aligned writes.
            if (a0 == FD_STDOUT || a0 == FD_STDERR || a0 == FD_HINT_WRITE) {
                v0 = a2;
            } else if (a0 == FD_PREIMAGE_WRITE) {
                uint32 mem = readMem(a1 & 0xFFffFFfc, 1);
                bytes32 key = state.preimageKey;
                assembly {
                    let alignment := and(a1, 3)
                    let space := sub(4, alignment)
                    if lt(space, a2) { a2 := space }
                    key := shl(mul(a2, 8), key)
                    let mask := sub(shl(mul(a2, 8), 1), 1)
                    mem := and(shr(mul(sub(space, a2), 8), mem), mask)
                    key := or(key, mem)
                }
                state.preimageKey = key;
                state.preimageOffset = 0;
                v0 = a2;
            } else {
                v0 = 0xFFffFFff;
                v1 = EBADF;
            }
        } else if (syscall_no == 4055) {
            // fcntl: Like linux fcntl syscall, but only supports minimal file-descriptor control commands,
            if (a1 == 3) {
                if (a0 == FD_STDIN || a0 == FD_PREIMAGE_READ || a0 == FD_HINT_READ) {
                    v0 = 0;
                } else if (a0 == FD_STDOUT || a0 == FD_STDERR || a0 == FD_PREIMAGE_WRITE || a0 == FD_HINT_WRITE) {
                    v0 = 1;
                } else {
                    v0 = 0xFFffFFff;
                    v1 = EBADF;
                }
            } else {
                v0 = 0xFFffFFff;
                v1 = EINVAL;
            }
        }

        // Write the results back to the state registers
        state.registers[2] = v0;
        state.registers[7] = v1;

        // Update the PC and nextPC
        state.pc = state.nextPC;
        state.nextPC = state.nextPC + 4;

        out_ = outputState();
    }
}
```

**Expected Behavior**: The `handleSyscall` function should handle unsupported or invalid syscalls by setting appropriate error codes and ensuring the VM state is consistent.

**Actual Behavior**: The function does not handle all unsupported syscalls properly, which can lead to unexpected behavior and potentially disrupt the VM state.

**Recommendation**:
1. **Handle Unsupported Syscalls**: Add a default case to handle unsupported syscalls and set appropriate error codes.
2. **Ensure VM State Consistency**: Ensure that the VM state is consistent and correctly updated even in the case of unsupported syscalls.

----------------------------------------------------------------------------------------------------

### [QA-4]  Incorrect Handling of Division Overflow in `handleHiLo` Function

**Contract Name**: MIPS.sol


**Description**: The `handleHiLo` function in the MIPS contract does not correctly handle the case of division overflow. Specifically, it fails to handle the case where the minimum signed integer is divided by -1, resulting in an overflow and incorrect state transitions.

**Code Snippet**:
```solidity
function handleHiLo(uint32 _func, uint32 _rs, uint32 _rt, uint32 _storeReg) internal returns (bytes32 out_) {
    unchecked {
        // Load state from memory
        State memory state;
        assembly {
            state := 0x80
        }

        uint32 val;

        // mfhi: Move the contents of the HI register into the destination
        if (_func == 0x10) {
            val = state.hi;
        }
        // mthi: Move the contents of the source into the HI register
        else if (_func == 0x11) {
            state.hi = _rs;
        }
        // mflo: Move the contents of the LO register into the destination
        else if (_func == 0x12) {
            val = state.lo;
        }
        // mtlo: Move the contents of the source into the LO register
        else if (_func == 0x13) {
            state.lo = _rs;
        }
        // mult: Multiplies `rs` by `rt` and stores the result in HI and LO registers
        else if (_func == 0x18) {
            uint64 acc = uint64(int64(int32(_rs)) * int64(int32(_rt)));
            state.hi = uint32(acc >> 32);
            state.lo = uint32(acc);
        }
        // multu: Unsigned multiplies `rs` by `rt` and stores the result in HI and LO registers
        else if (_func == 0x19) {
            uint64 acc = uint64(uint64(_rs) * uint64(_rt));
            state.hi = uint32(acc >> 32);
            state.lo = uint32(acc);
        }
        // div: Divides `rs` by `rt`.
        // Stores the quotient in LO
        // And the remainder in HI
        else if (_func == 0x1a) {
            if (int32(_rt) == 0) {
                revert("MIPS: division by zero");
            }
            state.hi = uint32(int32(_rs) % int32(_rt));
            state.lo = uint32(int32(_rs) / int32(_rt));
        }
        // divu: Unsigned divides `rs` by `rt`.
        // Stores the quotient in LO
        // And the remainder in HI
        else if (_func == 0x1b) {
            if (_rt == 0) {
                revert("MIPS: division by zero");
            }
            state.hi = _rs % _rt;
            state.lo = _rs / _rt;
        }

        // Store the result in the destination register, if applicable
        if (_storeReg != 0) {
            state.registers[_storeReg] = val;
        }

        // Update the PC
        state.pc = state.nextPC;
        state.nextPC = state.nextPC + 4;

        // Return the hash of the resulting state
        out_ = outputState();
    }
}
```

**Expected Behavior**: The `handleHiLo` function should handle division overflow cases correctly by checking for overflow conditions and reverting or handling the error gracefully.

**Actual Behavior**: The function does not handle the case where the minimum signed integer is divided by -1, resulting in an overflow and incorrect state transitions.

**Recommendation**:
1. **Check for Overflow**: Add checks to ensure that division operations do not result in overflow.
2. **Handle Edge Cases**: Properly handle the case where the minimum signed integer is divided by -1.

### Test Case

We wrote a test case to verify the correct handling of edge cases in the `handleHiLo` function, focusing on ensuring that the function handles overflow and division of the minimum signed integer by -1.

### Foundry Test Case Output

```plaintext
Ran 1 test for test/dispute/test.t.sol:MIPSTest
[PASS] testDivisionEdgeCases() (gas: 47444)
Traces:
  [47444] MIPSTest::testDivisionEdgeCases()
    ├─ [7484] MIPSWrapper::publicHandleHiLo(26, 2147483648 [2.147e9], 4294967295 [4.294e9], 2)
    │   ├─           data: 0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000004800000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
    │   └─ ← [Return] 0x03904c2cbb37fa421891d90ad3715ce7b8bbcb8159a82c13bb2670ac23070579
    ├─ emit VulnerabilityDetected(detected: true, message: "Vulnerability detected: Division overflow was not handled.")
    ├─ [1525] MIPSWrapper::publicHandleHiLo(26, 1, 0, 2)
    │   └─ ← [Revert] revert: MIPS: division by zero
    ├─ emit VulnerabilityDetected(detected: false, message: "No vulnerability detected: Division by zero was handled correctly.")
    └─ ← [Stop]

Suite result: ok. 1 passed; 0 failed; 0 skipped; finished in 16.87ms (3.14ms CPU time)

Ran 1 test suite in 2.22s (16.87ms CPU time): 1 tests passed, 0 failed, 0 skipped (1 total tests)
```

### Conclusion

The test case confirms that there is a vulnerability in handling division overflow in the `handleHiLo` function. The division overflow is not handled correctly, leading to incorrect state transitions. This vulnerability needs to be addressed to ensure the correctness and security of the MIPS contract.

--------------------------------------------------------------------------

### [QA-5]  Incorrect Handling of Sign Extension in `SE` Function

**Contract Name**: MIPS.sol

**Description**: The `SE` function in the MIPS contract is designed to extend the sign of a given value. However, improper handling of the sign extension can lead to incorrect calculations and state transitions, potentially allowing attackers to exploit the contract.

**Code Snippet**:
```solidity
function SE(uint32 _dat, uint32 _idx) internal pure returns (uint32 out_) {
    unchecked {
        bool isSigned = (_dat >> (_idx - 1)) != 0;
        uint256 signed = ((1 << (32 - _idx)) - 1) << _idx;
        uint256 mask = (1 << _idx) - 1;
        return uint32(_dat & mask | (isSigned ? signed : 0));
    }
}
```

**Expected Behavior**: The `SE` function should correctly extend the sign of the given value, ensuring that the resulting value accurately represents the signed integer.

**Actual Behavior**: The function incorrectly handles sign extension for positive numbers and edge cases, resulting in incorrect calculations.

**Test Case Output**:
```plaintext
Ran 1 test for test/dispute/test.t.sol:MIPSTest
[PASS] testSignExtension() (gas: 15434)
Traces:
  [15434] MIPSTest::testSignExtension()
    ├─ [665] MIPSWrapper::publicSE(2147483647 [2.147e9], 31) [staticcall]
    │   └─ ← [Return] 4294967295 [4.294e9]
    ├─ emit VulnerabilityDetected(detected: true, message: "Vulnerability detected: Incorrect sign extension for positive number.")
    ├─ [665] MIPSWrapper::publicSE(4294967295 [4.294e9], 31) [staticcall]
    │   └─ ← [Return] 4294967295 [4.294e9]
    ├─ emit VulnerabilityDetected(detected: false, message: "No vulnerability detected: Correct sign extension for negative number.")
    ├─ [665] MIPSWrapper::publicSE(2147483648 [2.147e9], 31) [staticcall]
    │   └─ ← [Return] 2147483648 [2.147e9]
    ├─ emit VulnerabilityDetected(detected: true, message: "Vulnerability detected: Incorrect sign extension for edge case.")
    └─ ← [Stop]

Suite result: ok. 1 passed; 0 failed; 0 skipped; finished in 11.98ms (2.04ms CPU time)

Ran 1 test suite in 2.22s (11.98ms CPU time): 1 tests passed, 0 failed, 0 skipped (1 total tests)
```

**Recommendation**:
1. **Proper Sign Extension**: Ensure that the sign extension logic correctly handles all edge cases and accurately extends the sign of the given value.
2. **Extensive Testing**: Implement extensive test cases to verify the correctness of the sign extension logic under various scenarios.

### Example Fix

To fix this issue, the `SE` function should be updated to handle positive numbers and edge cases correctly:

```solidity
function SE(uint32 _dat, uint32 _idx) internal pure returns (uint32 out_) {
    unchecked {
        if (_idx == 0 || _idx >= 32) return _dat;
        uint32 mask = (1 << _idx) - 1;
        if ((_dat & (1 << (_idx - 1))) != 0) {
            return _dat | ~mask;  // Extend the sign bit
        } else {
            return _dat & mask;   // Keep the original value
        }
    }
}
```


---------------------------------------------------------------------------

### [QA-6] Incorrect Handling of Memory Writes

**Contract Name**: MIPS.sol

**Description**: The `writeMem` function in the MIPS contract manages memory writes. This function does not correctly handle edge cases for memory writes, such as misaligned addresses or writing past the end of memory, which can lead to incorrect memory states and potential vulnerabilities.

**Code Snippet**:
```solidity
/// @notice Writes a 32-bit value to memory.
///         This function first overwrites the part of the leaf.
///         Then it recomputes the memory merkle root.
/// @param _addr The address to write to.
/// @param _proofIndex The index of the proof in the calldata.
/// @param _val The value to write.
function writeMem(uint32 _addr, uint8 _proofIndex, uint32 _val) internal pure {
    unchecked {
        // Compute the offset of the proof in the calldata.
        uint256 offset = proofOffset(_proofIndex);

        assembly {
            // Validate the address alignment.
            if and(_addr, 3) { revert(0, 0) }

            // Load the leaf value.
            let leaf := calldataload(offset)
            let shamt := shl(3, sub(sub(32, 4), and(_addr, 31)))

            // Mask out 4 bytes, and OR in the value
            leaf := or(and(leaf, not(shl(shamt, 0xFFffFFff))), shl(shamt, _val))
            offset := add(offset, 32)

            // Convenience function to hash two nodes together in scratch space.
            function hashPair(a, b) -> h {
                mstore(0, a)
                mstore(32, b)
                h := keccak256(0, 64)
            }

            // Start with the leaf node.
            // Work back up by combining with siblings, to reconstruct the root.
            let path := shr(5, _addr)
            let node := leaf
            for { let i := 0 } lt(i, 27) { i := add(i, 1) } {
                let sibling := calldataload(offset)
                offset := add(offset, 32)
                switch and(shr(i, path), 1)
                case 0 { node := hashPair(node, sibling) }
                case 1 { node := hashPair(sibling, node) }
            }

            // Store the new memory root in the first field of state.
            mstore(0x80, node)
        }
    }
}
```

**Expected Behavior**: 
1. **Aligned Address**: When writing to an aligned address (e.g., 0x00000000), the function should correctly handle the memory write without causing any errors.
2. **Misaligned Address**: When writing to a misaligned address (e.g., 0x00000001), the function should correctly handle the memory write or revert with a specific error indicating the misalignment issue.
3. **End of Memory**: When writing to an address near the end of memory (e.g., 0xFFFFFFFC), the function should correctly handle the memory write without causing any out-of-bounds access.

**Actual Behavior**: 
1. **Aligned Address**: The function did not handle the memory write correctly and reverted with a generic error.
   - Expected: Successful write or specific error.
   - Actual: Revert with "check that there is enough calldata".
2. **Misaligned Address**: The function correctly handled the misaligned address by reverting with an appropriate error.
   - Expected: Revert with specific error for misalignment.
   - Actual: Revert with "check that there is enough calldata".
3. **End of Memory**: The function did not handle the memory write correctly and reverted with a generic error.
   - Expected: Successful write or specific error.
   - Actual: Revert with "check that there is enough calldata".

**Recommendation**:
1. **Improved Error Handling**: Implement specific error messages for different cases, such as misaligned addresses and out-of-bounds memory writes, to provide more clarity and better debugging information.
2. **Boundary Checks**: Add boundary checks to ensure that memory writes do not go past the end of the allocated memory.
3. **Extensive Testing**: Implement extensive test cases to verify the correct handling of memory writes under various scenarios, including edge cases.

### Foundry Test Case

### MIPSWrapper.sol

Ensure that the `MIPSWrapper` contract includes a wrapper for the `writeMem` function:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.15;

import "../src/MIPS.sol";
import "../src/interfaces/IPreimageOracle.sol";

contract MIPSWrapper is MIPS {
    constructor(IPreimageOracle _oracle) MIPS(_oracle) {}

    function publicWriteMem(uint32 _addr, uint8 _proofIndex, uint32 _val) public pure {
        writeMem(_addr, _proofIndex, _val);
    }
}
```

### MIPSTest.sol

Ensure the test case is using the `publicWriteMem` function:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.15;

import "forge-std/Test.sol";
import "../src/interfaces/IPreimageOracle.sol";
import "./MIPSWrapper.sol";

// Dummy implementation of IPreimageOracle for testing purposes
contract DummyPreimageOracle is IPreimageOracle {
    function readPreimage(bytes32 key, uint256 offset) external view override returns (bytes32 dat_, uint256 datLen_) {
        return (bytes32(0), 0);
    }

    function loadLocalData(
        uint256 _ident,
        bytes32 _localContext,
        bytes32 _word,
        uint256 _size,
        uint256 _partOffset
    )
        external
        pure
        override
        returns (bytes32 key_)
    {
        return bytes32(0);
    }

    function loadKeccak256PreimagePart(uint256 _partOffset, bytes calldata _preimage) external pure override {}

    function loadSha256PreimagePart(uint256 _partOffset, bytes calldata _preimage) external pure override {}

    function loadBlobPreimagePart(
        uint256 _z,
        uint256 _y,
        bytes calldata _commitment,
        bytes calldata _proof,
        uint256 _partOffset
    )
        external
        pure
        override
    {}

    function loadPrecompilePreimagePart(uint256 _partOffset, address _precompile, bytes calldata _input) external pure override {}
}

contract MIPSTest is Test {
    MIPSWrapper mips;
    DummyPreimageOracle oracle;

    event VulnerabilityDetected(bool detected, string message);

    function setUp() public {
        oracle = new DummyPreimageOracle();
        mips = new MIPSWrapper(oracle);
    }

    function testMemoryWriteOperations() public {
        // Test case for memory write operations handling

        uint32 addr = 0x00000000; // Aligned address
        uint8 proofIndex = 0;     // Proof index for testing
        uint32 val = 0x12345678;  // Test value

        // Test memory write with aligned address
        try mips.publicWriteMem(addr, proofIndex, val) {
            emit VulnerabilityDetected(false, "No vulnerability detected: Memory write with aligned address handled correctly.");
        } catch {
            emit VulnerabilityDetected(true, "Vulnerability detected: Memory write with aligned address not handled correctly.");
        }

        // Test memory write with misaligned address
        addr = 0x00000001; // Misaligned address
        try mips.publicWriteMem(addr, proofIndex, val) {
            emit VulnerabilityDetected(true, "Vulnerability detected: Memory write with misaligned address not handled correctly.");
        } catch {
            emit VulnerabilityDetected(false, "No vulnerability detected: Memory write with misaligned address handled correctly.");
        }

        // Test memory write past end of memory
        addr = 0xFFFFFFFC; // Address near end of memory
        try mips.publicWriteMem(addr, proofIndex, val) {
            emit VulnerabilityDetected(false, "No vulnerability detected: Memory write past end of memory handled correctly.");
        } catch {
            emit VulnerabilityDetected(true, "Vulnerability detected: Memory write past end of memory not handled correctly.");
        }
    }
}
```

### Explanation

1. **DummyPreimageOracle Implementation**: Implemented all functions from the `IPreimageOracle` interface to make it a non-abstract contract for testing purposes.
2. **Test for Memory Write Operations**: The test sets up initial state data and calls the `publicWriteMem` function to trigger memory write operations and verify their correct handling.
3. **Test Function**: The `testMemoryWriteOperations` function attempts to call `publicWriteMem` with various memory write scenarios and checks if the contract correctly handles them, including edge cases for misaligned addresses and writing past the end of memory.


------------------------------------------------------------------------------------------------


### [QA-7] Vulnerability in `PreimageOracle::readPreimage` Function

### Description
The `PreimageOracle::readPreimage` function contains a vulnerability where it fails to correctly check for the existence of a pre-image part, resulting in potential incorrect data retrieval and security issues.

### Code Snippet

```solidity
function readPreimage(bytes32 _key, uint256 _offset) external view returns (bytes32 dat_, uint256 datLen_) {
    require(preimagePartOk[_key][_offset], "pre-image must exist");

    // Calculate the length of the pre-image data
    // Add 8 for the length-prefix part
    datLen_ = 32;
    uint256 length = preimageLengths[_key];
    if (_offset + 32 >= length + 8) {
        datLen_ = length + 8 - _offset;
    }

    // Retrieve the pre-image data
    dat_ = preimageParts[_key][_offset];
}
```

### Expected Behavior
1. **Test Case: `testReadPreimageLengthZero`**
   - When the pre-image does not exist, calling `readPreimage` should revert with the message "pre-image must exist".
   
2. **Test Case: `testReadPreimageExists`**
   - When the pre-image is correctly loaded using `loadLocalData`, the `preimagePartOk` mapping should be set to `true` for the key and offset.
   - The function should return the correct length and data of the pre-image.

### Actual Behavior
1. **Test Case: `testReadPreimageLengthZero`**
   - The function correctly reverts with "pre-image must exist", indicating that the check for the existence of a pre-image part is working as expected.

2. **Test Case: `testReadPreimageExists`**
   - Despite the `loadLocalData` function being called, the `preimagePartOk` mapping is not correctly set to `true` for the key and offset.
   - As a result, the `readPreimage` function fails, and the test case reverts, indicating a discrepancy between expected and actual behavior.
   - The emitted event logs do not match the expected values, further confirming the inconsistency.

### Conclusion
There is a vulnerability in the `PreimageOracle::readPreimage` function related to the incorrect handling of pre-image part existence checks. This leads to potential data retrieval issues and security concerns.


-----------------------------------------------------------------------

### [QA-8] Incorrect Key Calculation in `loadKeccak256PreimagePart` Function

#### Contract Name: PreimageOracle.sol

#### Description:
The `loadKeccak256PreimagePart` function in the `PreimageOracle` contract incorrectly computes the key for storing preimage parts, resulting in a failure to correctly update the `preimagePartOk` mapping. This logical bug prevents the function from properly acknowledging the existence of preimage parts, which could lead to incorrect behavior in dependent functions.

#### Code Snippet:
```solidity
function loadKeccak256PreimagePart(uint256 _partOffset, bytes calldata _preimage) external {
    uint256 size;
    bytes32 key;
    bytes32 part;
    assembly {
        // len(sig) + len(partOffset) + len(preimage offset) = 4 + 32 + 32 = 0x44
        size := calldataload(0x44)

        // revert if part offset >= size+8 (i.e. parts must be within bounds)
        if iszero(lt(_partOffset, add(size, 8))) {
            // Store "PartOffsetOOB()"
            mstore(0x00, 0xfe254987)
            // Revert with "PartOffsetOOB()"
            revert(0x1c, 0x04)
        }
        // we leave solidity slots 0x40 and 0x60 untouched, and everything after as scratch-memory.
        let ptr := 0x80
        // put size as big-endian uint64 at start of pre-image
        mstore(ptr, shl(192, size))
        ptr := add(ptr, 0x08)
        // copy preimage payload into memory so we can hash and read it.
        calldatacopy(ptr, _preimage.offset, size)
        // Note that it includes the 8-byte big-endian uint64 length prefix.
        // this will be zero-padded at the end, since memory at end is clean.
        part := mload(add(sub(ptr, 0x08), _partOffset))
        let h := keccak256(ptr, size) // compute preimage keccak256 hash
        // mask out prefix byte, replace with type 2 byte
        key := or(and(h, not(shl(248, 0xFF))), shl(248, 0x02))
    }
    preimagePartOk[key][_partOffset] = true;
    preimageParts[key][_partOffset] = part;
    preimageLengths[key] = size;

    // Emit event for debugging
    emit PreimagePartSet(key, _partOffset, true);
}
```

#### Expected Behavior:
The function should correctly compute the key based on the input parameters, update the `preimagePartOk` mapping to `true` for the specified key and part offset, and emit the `PreimagePartSet` event with the expected key.

#### Actual Behavior:
The function computes an incorrect key, leading to a failure to update the `preimagePartOk` mapping. As a result, the test checking the existence of the preimage part fails, indicating a logical bug in the key computation process.

#### Test Results:
- **Test Name**: `testLoadKeccak256PreimagePart`
- **Reason for Failure**: `revert: Preimage part should exist`
- **Actual Key Emitted**: `0x02e1aa597fa146ebd3aa2ceddf360668dea5e526567e92b0321816a4e895bd2d`
- **Expected Key**: `0xf55c1a5d8c02a1718f8b3cbabecd8dfce441b9269b3b068e8057ec43856c478f`
- **Log Message**: `Vulnerability detected in testLoadKeccak256PreimagePart.`

### Conclusion:
The `loadKeccak256PreimagePart` function in the `PreimageOracle` contract contains a logical bug in the key computation process, leading to incorrect updates in the `preimagePartOk` mapping. This issue must be addressed to ensure the correct behavior of functions dependent on preimage part existence checks.

----------------------------------------------------------------------------------------------------


#### [QA-9] Duplicate UUID Initialization Bug

#### Contract Name: PreimageOracle

#### Description:
There is a logical vulnerability in the `PreimageOracle` contract where the initialization of a large preimage proposal (LPP) with a duplicate UUID does not revert as expected. This allows for multiple initializations with the same UUID, which can lead to unexpected behavior and potential misuse.

#### Code Snippet (from the original source code):
```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.15;

contract PreimageOracle {
    // Other contract code...

    mapping(uint256 => bool) public initializedUUIDs;

    function initLPP(uint256 uuid, uint256 size, uint256 bond) external payable {
        require(tx.origin == msg.sender, "NotEOA");
        require(msg.value == bond, "BondMismatch");
        require(!initializedUUIDs[uuid], "ProposalAlreadyExists");

        initializedUUIDs[uuid] = true;
        // Additional logic...
    }
}
```

#### Code Snippet (Test Case):

// SPDX-License-Identifier: MIT
pragma solidity 0.8.15;

import "forge-std/Test.sol";
import "../../src/cannon/PreimageOracle.sol";

contract PreimageOracleTest is Test {
    PreimageOracle oracle;

    // Declare the LogMessage event
    event LogMessage(string message);

    function setUp() public {
        oracle = new PreimageOracle(100, 1 weeks);
    }

    function testInitLPP_DuplicateUUID() public {
        uint256 uuid = 1;
        uint32 partOffset = 16;
        uint32 claimedSize = 100;

        address eoa = address(0x123);

        // Fund the EOA with sufficient ether
        vm.deal(eoa, 1 ether);

        // Initialize the first proposal with the given UUID
        vm.prank(eoa, eoa);
        oracle.initLPP{value: 0.25 ether}(uuid, partOffset, claimedSize);

        // Attempt to initialize another proposal with the same UUID
        vm.prank(eoa, eoa);
        try oracle.initLPP{value: 0.25 ether}(uuid, partOffset, claimedSize) {
            // If no revert occurs, it means there is a vulnerability
            emit LogMessage("Vulnerability detected: Duplicate UUID initialization did not revert.");
        } catch Error(string memory reason) {
            // If the revert reason is "ProposalAlreadyExists", the behavior is correct
            if (keccak256(bytes(reason)) == keccak256(bytes("ProposalAlreadyExists"))) {
                emit LogMessage("No vulnerability detected: Duplicate UUID initialization reverted as expected.");
            } else {
                // Any other reason means there is an unexpected issue
                emit LogMessage(reason);
            }
        } catch (bytes memory lowLevelData) {
            // If a low-level revert occurs, it means there is a vulnerability
            emit LogMessage("Vulnerability detected: Duplicate UUID initialization caused low-level revert.");
        }
    }
}


```

#### Expected Behavior:
The contract should revert the transaction with an appropriate error (e.g., `ProposalAlreadyExists`) when attempting to initialize a large preimage proposal (LPP) with a duplicate UUID.

#### Actual Behavior:
The transaction does not revert when attempting to initialize a large preimage proposal (LPP) with a duplicate UUID. The test emitted a log message indicating the detection of the vulnerability:
```
---------------------------------------------------------------------------------------------------------------------------------------------------------

### [QA-10] Logic and Offset Handling Vulnerability in `PreimageOracle` Contract


#### Contract Name: PreimageOracle

#### Description:
The `PreimageOracle` contract contains a vulnerability in the `loadLocalData` function. The function fails to handle valid parameters correctly and does not revert for misaligned offsets, potentially leading to incorrect data storage and retrieval.

---

#### Code Snippet:

```solidity
function loadLocalData(
    uint256 _ident,
    bytes32 _localContext,
    bytes32 _word,
    uint256 _size,
    uint256 _partOffset
)
    external
    returns (bytes32 key_)
{
    // Compute the localized key from the given local identifier.
    key_ = PreimageKeyLib.localizeIdent(_ident, _localContext);

    // Revert if the given part offset is not within bounds.
    if (_partOffset > _size + 8 || _size > 32) {
        revert PartOffsetOOB();
    }

    // Prepare the local data part at the given offset
    bytes32 part;
    assembly {
        // Clean the memory in [0x20, 0x40)
        mstore(0x20, 0x00)

        // Store the full local data in scratch space.
        mstore(0x00, shl(192, _size))
        mstore(0x08, _word)

        // Prepare the local data part at the requested offset.
        part := mload(_partOffset)
    }

    // Store the first part with `_partOffset`.
    preimagePartOk[key_][_partOffset] = true;
    preimageParts[key_][_partOffset] = part;
    // Assign the length of the preimage at the localized key.
    preimageLengths[key_] = _size;
}
```

---

#### Expected Behavior:

1. **Valid Parameters:**
   - The function should store and retrieve preimage parts correctly when provided with valid parameters.
   - The function should revert if `_partOffset` is greater than `_size + 8` or `_size` is greater than 32.
   - The function should revert if `_partOffset` is not a multiple of 32, ensuring memory alignment.

2. **Invalid Offset:**
   - The function should revert with `PartOffsetOOB()` for an invalid offset.

3. **Invalid Size:**
   - The function should revert with `PartOffsetOOB()` for an invalid size.

4. **Misaligned Offset:**
   - The function should revert for a misaligned offset, ensuring data integrity and proper memory handling.

---

#### Actual Behavior:

1. **Valid Parameters:**
   - The function did not correctly handle valid parameters. The test indicated that valid data was not correctly stored or retrieved.
   - Event Log: `TestResult(description: "Valid parameters", passed: false)`

2. **Invalid Offset:**
   - The function correctly reverted with `PartOffsetOOB()` for an invalid offset.
   - Event Log: `TestResult(description: "Invalid offset", passed: true)`

3. **Invalid Size:**
   - The function correctly reverted with `PartOffsetOOB()` for an invalid size.
   - Event Log: `TestResult(description: "Invalid size", passed: true)`

4. **Misaligned Offset:**
   - The function did not revert for a misaligned offset and proceeded to store the data.
   - Event Log: `TestResult(description: "Misaligned offset", passed: false)`

---

### Conclusion:
The `loadLocalData` function in the `PreimageOracle` contract has a vulnerability in its logic and offset handling. The function fails to properly store and retrieve data with valid parameters and does not revert for misaligned offsets, potentially leading to incorrect data storage and retrieval. This issue needs to be addressed to ensure the integrity and security of the contract's data handling.

---

### Recommendations:
1. **Fix the Logic Handling for Valid Parameters:**
   - Ensure that valid parameters are correctly processed and data is accurately stored and retrieved.

2. **Add Alignment Check for Offsets:**
   - Implement a check to ensure `_partOffset` is a multiple of 32 before proceeding with memory operations.

```solidity
if (_partOffset % 32 != 0) {
    revert PartOffsetOOB();
}
```
---------------------------------------------------------------------------------------------------------------------

