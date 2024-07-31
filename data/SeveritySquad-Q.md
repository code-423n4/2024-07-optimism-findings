`Low Risk & Non Critical Findings`
# Report
## Low Risk Findings 
**Low Risk Findings List**
| Number | Issue Details | Instances |
|-----:|----|-----|
| L-01 | Incorrect checks in preImage Oracle allow for bypass validations against OOB errors | 2 |
| L-02 |  Theoretical Overflow at LibPosition#wrap() could potentially lead to manipulation of position | 1 |
| L-03 | Hashing leafs of 64 bytes allows the preimage to be vulnerable to second node preimage attacks | 2 |
| L-04 | Opcode differences btw the 2 chains are not handled whenever executing the single instruction trace | 4 |
| L-05 | Unsafe cast of the bond can lead to loss of funds even when the bond > max.128 by 1 | 1 |
| L-06 | The used Proxy for the DisputeGameFactory.sol is not fully EIP1967 compliant  | 1 |
| L-07 | partial parts can be forced to be stored in the preimageoracle even when not finalized | 2 |
| L-08 | Lack of input validation at loadPrecompilePreimagePart()

# L-01 Incorrect checks in preImage Oracle allow for bypass validations against OOB errors
## Summary 
## Vulnerability Details


# L-02 Theoretical Overflow at LibPosition#wrap() could potentially lead to manipulation of position 

The wrap() function could overflow whenever `indexAtDepth` is set to `type(uint128).max`. 

```solidity
    function wrap(uint8 _depth, uint128 _indexAtDepth) internal pure returns (Position position_) {
        assembly {
            // gindex = 2^{_depth} + _indexAtDepth
// @audit-issue Overflow possible when `_indexAtDepth` is set at `type(uint128).max`.
            position_ := add(shl(_depth, 1), _indexAtDepth)
        }
    }   
```
## Recommended Mitigation Steps

Please add a check before the assembly code to make sure that `_indexAtDepth` would never overflow and return a wrong `_position`.


# L-06 The used Proxy for the DisputeGameFactory.sol is not fully EIP1967 compliant

**NOTE:**
*- This should be seen as in scope, this issue pinpoint a functionality of [DisputeGameFactoryProxy.sol](https://docs.optimism.io/chain/addresses) the proxy that is used by the DisputeGameFactory.sol (in-scope)*

The [setImplementation function](https://etherscan.io/address/0xe5965Ab5962eDc7477C8520243A95517CD252fA9#code#F1#L101) is designed to set a new implementation contract address for upgrading the DisputeGameFactory.sol contract to a new logic. This function stores the new implementation address in the EIP1967 implementation slot and immediately applies the new logic. The current function does not validate whether the new address is actually a contract. This validation step is crucial and is included in the official ERC1967 implementation examples. 

### Impact:

Failing to validate that the new implementation address is a contract can lead to serious issues. Specifically, if a non-contract address is set as the implementation, the proxy would be unable to delegate calls correctly, leading to potentially severe operational failures. This could render the proxy dysfunctional and may result in a significant disruption of services, especially if the function is executed in a live production environment.

### Recommendation:

To align with the official ERC1967 standard and enhance the robustness of the setImplementation function, it is recommended to include a check to validate that the new implementation address is indeed a contract. This can be done by adding a line to require that the new implementation address satisfies the Address.isContract condition, as follows:

```solidity
require(Address.isContract(newImplementation), "ERC1967: new implementation is not a contract");	
```

References:	
- ERC1967: Link to the [official documentation and examples](https://eips.ethereum.org/EIPS/eip-1967#abstract:~:text=/**%0A%20%20%20%20%20*%20%40dev%20Stores%20a,newImplementation%3B%0A%20%20%20%20%7D)
- Solidity Documentation on Address.isContract: Link to [Solidity docs](https://docs.soliditylang.org/en/v0.8.6/units-and-global-variables.html#address-related)
- [Identic issue from Euler contest on Cantina](https://cantina.xyz/code/41306bb9-2bb8-4da6-95c3-66b85e11639f/findings/320) *(got confirmed by judge as low)*

# L-03 Hashing leafs of 64 bytes allows the preimage to be vulnerable to second node preimage attacks
https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L756C3-L785C6
## Summary
```solidity
  function _verify(
        bytes32[] calldata _proof,
        bytes32 _root,
        uint256 _index,
        bytes32 _leaf
    )
        internal
        pure
        returns (bool isValid_)
    {
        /// @solidity memory-safe-assembly
        assembly {
            function hashTwo(a, b) -> hash {
                mstore(0x00, a)
                mstore(0x20, b)
                hash := keccak256(0x00, 0x40)
            }

            let value := _leaf
            for { let i := 0x00 } lt(i, KECCAK_TREE_DEPTH) { i := add(i, 0x01) } {
                let branchValue := calldataload(add(_proof.offset, shl(0x05, i)))

                switch and(shr(i, _index), 0x01)
                case 1 { value := hashTwo(branchValue, value) }
                default { value := hashTwo(value, branchValue) }
            }

            isValid_ := eq(value, _root)
        }
    }
```
# L-08 | Lack of input validation at loadPrecompilePreimagePart()

## Summary

The loadPrecompilePreimagePart() function has a straightforward functionality which prepares a precompile result to be read by a precompile key for the specified offset. However there's a major flaw within the function that allow anyone to set an arbitrary address to the `_precompile` parameter although the code does not require it.

## Proof of Concept

The function uses `staticcall` to call the precompiled contract at `_precompile` address. But before initiating the `staticcall` there must be verified wether the address is a precompile contract or any arbitrary external contract. 

https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L357

The user supplied address is directly used within the `staticcall` without verifying if the address is a precompiled contract.

## Impact 

Initiating calls to arbitrary addresses (e.g EOA, malicious contracts,...) instead of a precompiled contracts which is the main purpose of the function, could lead to unintended behavior.


## Tools Used

Manual review

## Recommended Mitigation Steps

Consider adding a check where only precompiled contracts (0x01 to 0x09, 0x0a) could be called. 
