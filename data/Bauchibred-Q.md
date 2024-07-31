# QA Report for **Optimism Superchain**

## Table of Contents

| Issue ID | Description |
| -------- | ----------- |
| [QA-01](#qa-01-allow-challengers-to-pass-in-the-address-they-would-like-to-receive-their-bonds-to) | Allow challengers to pass in the address they would like to receive their bonds to |
| [QA-02](#qa-02-fix-typos-_(multiple-instances)_) | Fix typos _(Multiple instances)_ |
| [QA-03](#qa-03-preimageoracle#loadblobpreimagepart()-has-a-non-consistent-documentation-in-comparison-to-its-sister-functions) | `PreimageOracle#loadBlobPreimagePart()` has a non-consistent documentation in comparison to it's sister functions |
| [QA-04](#qa-04-protocol-in-some-instances-does-not-protect/reward-honest-users) | Protocol in some instances does not protect/reward honest users |
| [QA-05](#qa-05-the-subtle-invariant-of-requiring-an-eoa-to-be-the-one-querying-addleaveslpp-can-easily-be-broken) | The subtle invariant of requiring an EOA to be the one querying `addLeavesLPP` can easily be broken |
| [QA-06](#qa-06-min-bond-ether-is-quite-small-&-makes-it-easy-for-the-whale-attack-from-the-readme-to-be-possible) | Min bond ether is quite small & makes it easy for the whale attack from the readMe to be possible |
| [QA-07](#qa-07-sometimes-defense-against-invalid-claims-are-not-allowed) | Sometimes defense against invalid claims are not allowed |
| [QA-08](#qa-08-cannontypes#partoffset()-passes-its-return-value-in-the-_wrong_-unsigned-integer-size) | `CannonTypes#partOffset()` passes it's return value in the _wrong_ unsigned integer size |
| [QA-09](#qa-09-an-owner-can-always-ensure-dispute-games-are-not-created) | An owner can always ensure dispute games are not created |
| [QA-10](#qa-10-remove-duplicate-imports) | Remove duplicate imports |
| [QA-11](#qa-11-return-gas-bomb-when-paying-out-bonds) | Return gas bomb when paying out bonds |
| [QA-12](#qa-12-import-declarations-should-import-specific-identifiers-rather-than-the-whole-file) | Import declarations should import specific identifiers, rather than the whole file |
| [QA-13](#qa-13-paying-out-bonds-could-silently-fail) | Paying out bonds could silently fail |
| [QA-14](#qa-14-an-admin-can-make-everyone-not-be-able-to-create-valid-bonds) | An admin can make everyone not be able to create valid bonds |

## QA-01 Allow challengers to pass in the address they would like to receive their bonds to

### Proof of Concept

Take a look at https://github.com/code-423n4/2024-07-optimism/blob/2e11e15e7a9a86f90de090ebf9e3516279e30897/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L568-L606

```solidity
    function challengeLPP(
        address _claimant,
        uint256 _uuid,
        LibKeccak.StateMatrix memory _stateMatrix,
        Leaf calldata _preState,
        bytes32[] calldata _preStateProof,
        Leaf calldata _postState,
        bytes32[] calldata _postStateProof
    )
        external
    {
        // Verify that both leaves are present in the merkle tree.
        bytes32 root = getTreeRootLPP(_claimant, _uuid);
        if (
            !(
                _verify(_preStateProof, root, _preState.index, _hashLeaf(_preState))
                    && _verify(_postStateProof, root, _postState.index, _hashLeaf(_postState))
            )
        ) revert InvalidProof();

        // Verify that the prestate passed matches the intermediate state claimed in the leaf.
        if (keccak256(abi.encode(_stateMatrix)) != _preState.stateCommitment) revert InvalidPreimage();

        // Verify that the pre/post state are contiguous.
        if (_preState.index + 1 != _postState.index) revert StatesNotContiguous();

        // Absorb and permute the input bytes.
        LibKeccak.absorb(_stateMatrix, _postState.input);
        LibKeccak.permutation(_stateMatrix);

        // Verify that the post state hash doesn't match the expected hash.
        if (keccak256(abi.encode(_stateMatrix)) == _postState.stateCommitment) revert PostStateMatches();

        // Mark the keccak claim as countered.
        proposalMetadata[_claimant][_uuid] = proposalMetadata[_claimant][_uuid].setCountered(true);
  ///@audit
        // Pay out the bond to the challenger.
        _payoutBond(_claimant, _uuid, msg.sender);
    }
```

Which calls https://github.com/code-423n4/2024-07-optimism/blob/2e11e15e7a9a86f90de090ebf9e3516279e30897/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L788-L800

```solidity
    function _payoutBond(address _claimant, uint256 _uuid, address _to) internal {
        // Pay out the bond to the claimant.
        uint256 bond = proposalBonds[_claimant][_uuid];
        proposalBonds[_claimant][_uuid] = 0;
        (bool success,) = _to.call{ value: bond }("");//@audit
        if (!success) revert BondTransferFailed();
    }
```

### Impact

QA, since this just limits the amount of users that can challenge, but current implementation means some users would have to create fresh contracts in order to challenge in the case their contracts can't receive Ether.

### Recommended Mitigation Steps

Consider applying these changes:

```diff
    function challengeLPP(
        address _claimant,
        uint256 _uuid,
        LibKeccak.StateMatrix memory _stateMatrix,
        Leaf calldata _preState,
        bytes32[] calldata _preStateProof,
        Leaf calldata _postState,
        bytes32[] calldata _postStateProof,
+        address bondReceiver
    )
        external
    {
        // Verify that both leaves are present in the merkle tree.
        bytes32 root = getTreeRootLPP(_claimant, _uuid);
        if (
            !(
                _verify(_preStateProof, root, _preState.index, _hashLeaf(_preState))
                    && _verify(_postStateProof, root, _postState.index, _hashLeaf(_postState))
            )
        ) revert InvalidProof();

        // Verify that the prestate passed matches the intermediate state claimed in the leaf.
        if (keccak256(abi.encode(_stateMatrix)) != _preState.stateCommitment) revert InvalidPreimage();

        // Verify that the pre/post state are contiguous.
        if (_preState.index + 1 != _postState.index) revert StatesNotContiguous();

        // Absorb and permute the input bytes.
        LibKeccak.absorb(_stateMatrix, _postState.input);
        LibKeccak.permutation(_stateMatrix);

        // Verify that the post state hash doesn't match the expected hash.
        if (keccak256(abi.encode(_stateMatrix)) == _postState.stateCommitment) revert PostStateMatches();

        // Mark the keccak claim as countered.
        proposalMetadata[_claimant][_uuid] = proposalMetadata[_claimant][_uuid].setCountered(true);
  ///@audit
        // Pay out the bond to the challenger.
-        _payoutBond(_claimant, _uuid, msg.sender);
+        _payoutBond(_claimant, _uuid, bondReceiver);
    }
```

And similarly for all other instances where `_payoutBond` gets queried.

## QA-02 Fix typos _(Multiple instances)_

### Proof of Concept

> _NB: Fixes are indicated by " ** ** "_
- Typos from [PreimageOracle.sol](https://github.com/code-423n4/2024-07-optimism/blob/2e11e15e7a9a86f90de090ebf9e3516279e30897/packages/contracts-bedrock/src/cannon/PreimageOracle.sol):

```diff
-    /// @notice Mapping of claimants to proposal UUIDs to the preimage part picked up during the absorbtion process.
+    /// @notice Mapping of claimants to proposal UUIDs to the preimage part picked up during the **absorption** process.

..snip

-            //         free memory ptr region. Since the exact number of btyes that is copied into scratch space is
+            //         free memory ptr region. Since the exact number of **bytes** that is copied into scratch space is

..snip
-            // Compute the key: `keccak256(commitment ++ z)`. Since the exact number of btyes that is copied into
+            // Compute the key: `keccak256(commitment ++ z)`. Since the exact number of **bytes** that is copied into

..snip
-            // Reuse the `ptr` to store the preimage part: <sizePrefix ++ precompileStatus ++ returrnData>
+            // Reuse the `ptr` to store the preimage part: <sizePrefix ++ precompileStatus ++ **returnData**>

..snip

-            // compute part given ofset
+            // compute part given **offset**
            part := mload(add(sub(ptr, 0x08), _partOffset))
..snip
            // If the preimage part is in the data we're about to absorb, persist the part to the caller's large
-            // preimaage metadata.
+            // **preimage** metadata.



```

- Typos from [MIPS.sol](https://github.com/code-423n4/2024-07-optimism/blob/2e11e15e7a9a86f90de090ebf9e3516279e30897/packages/contracts-bedrock/src/cannon/MIPS.sol):

```diff

..snip
-                // Validate the address alignement.
+                // Validate the address **alignment**.
                if and(_addr, 3) { revert(0, 0) }
..snip
-                // Validate the address alignement.
+                // Validate the address **alignment**.
                if and(_addr, 3) { revert(0, 0) }




```

### Impact

QA

### Recommended Mitigation Steps

```diff
+ Apply the fixes
```

## QA-03 `PreimageOracle#loadBlobPreimagePart()` has a non-consistent documentation in comparison to it's sister functions

### Proof of Concept

Take a look at https://github.com/code-423n4/2024-07-optimism/blob/2e11e15e7a9a86f90de090ebf9e3516279e30897/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L244-L332

```solidity
    function loadBlobPreimagePart(
        uint256 _z,
        uint256 _y,
        bytes calldata _commitment,
        bytes calldata _proof,
        uint256 _partOffset
    )
        external
    {
        bytes32 key;
        bytes32 part;
        assembly {
            // Compute the versioned hash. The SHA2 hash of the 48 byte commitment is masked with the version byte,
            // which is currently 1. https://eips.ethereum.org/EIPS/eip-4844#parameters
            // SAFETY: We're only reading 48 bytes from `_commitment` into scratch space, so we're not reading into the
            //         free memory ptr region. Since the exact number of btyes that is copied into scratch space is
            //         the same size as the hash input, there's no concern of dirty memory being read into the hash
            //         input.
            calldatacopy(0x00, _commitment.offset, 0x30)
            let success := staticcall(gas(), 0x02, 0x00, 0x30, 0x00, 0x20)
            if iszero(success) {
                // Store the "ShaFailed()" error selector.
                mstore(0x00, 0xf9112969)
                // revert with "ShaFailed()"
                revert(0x1C, 0x04)
            }
            // Set the `VERSIONED_HASH_VERSION_KZG` byte = 1 in the high-order byte of the hash.
            let versionedHash := or(and(mload(0x00), not(shl(248, 0xFF))), shl(248, 0x01))

            // we leave solidity slots 0x40 and 0x60 untouched, and everything after as scratch-memory.
            let ptr := 0x80

            // Load the inputs for the point evaluation precompile into memory. The inputs to the point evaluation
            // precompile are packed, and not supposed to be ABI-encoded.
            mstore(ptr, versionedHash)
            mstore(add(ptr, 0x20), _z)
            mstore(add(ptr, 0x40), _y)
            calldatacopy(add(ptr, 0x60), _commitment.offset, 0x30)
            calldatacopy(add(ptr, 0x90), _proof.offset, 0x30)

            // Verify the KZG proof by calling the point evaluation precompile. If the proof is invalid, the precompile
            // will revert.
            success :=
                staticcall(
                    gas(), // forward all gas
                    0x0A, // point evaluation precompile address
                    ptr, // input ptr
                    0xC0, // input size = 192 bytes
                    0x00, // output ptr
                    0x00 // output size
                )
            if iszero(success) {
                // Store the "InvalidProof()" error selector.
                mstore(0x00, 0x09bde339)
                // revert with "InvalidProof()"
                revert(0x1C, 0x04)
            }

//snip
        preimagePartOk[key][_partOffset] = true;
        preimageParts[key][_partOffset] = part;
        preimageLengths[key] = 32;
    }
```

This function is used to e verify that `p(_z) = _y` given `_commitment` that corresponds to the polynomial `p(x)` and a KZG proof. The value `y` is the pre-image, and the preimage key is `5 ++ keccak256(_commitment ++ z)[1:]`.

Issue however is the fact that the documentation within this in relation to its sister function is not consistent, this is because this comment https://github.com/code-423n4/2024-07-optimism/blob/2e11e15e7a9a86f90de090ebf9e3516279e30897/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L256

```solidity
            // Compute the versioned hash. The SHA2 hash of the 48 byte commitment is masked with the version byte,
```

Hints the use of `SHA2`, whereas the SHA256 precompile (`0x02`) is queried as can be seen here: https://github.com/code-423n4/2024-07-optimism/blob/2e11e15e7a9a86f90de090ebf9e3516279e30897/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L263-L264

```solidity
            let success := staticcall(gas(), 0x02, 0x00, 0x30, 0x00, 0x20)

```

Making the whole approach a bit confusing for all integrators, auditors/devs & users

### Impact

QA - best practice

### Recommended Mitigation Steps

Consider applying these changes: https://github.com/code-423n4/2024-07-optimism/blob/2e11e15e7a9a86f90de090ebf9e3516279e30897/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L256-L269

```diff
-            // Compute the versioned hash. The SHA2 hash of the 48 byte commitment is masked with the version byte,
+            // Compute the versioned hash. The SHA256 hash of the 48 byte commitment is masked with the version byte,
            // which is currently 1. https://eips.ethereum.org/EIPS/eip-4844#parameters
            // SAFETY: We're only reading 48 bytes from `_commitment` into scratch space, so we're not reading into the
            //         free memory ptr region. Since the exact number of btyes that is copied into scratch space is
            //         the same size as the hash input, there's no concern of dirty memory being read into the hash
            //         input.
            calldatacopy(0x00, _commitment.offset, 0x30)
            let success := staticcall(gas(), 0x02, 0x00, 0x30, 0x00, 0x20)
            if iszero(success) {
                // Store the "ShaFailed()" error selector.
                mstore(0x00, 0xf9112969)
                // revert with "ShaFailed()"
                revert(0x1C, 0x04)
            }
```

## QA-04 Protocol in some instances does not protect/reward honest users

### Proof of Concept

Take a look at https://github.com/code-423n4/2024-07-optimism/blob/2e11e15e7a9a86f90de090ebf9e3516279e30897/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L234-L312

```solidity
    function step(
        uint256 _claimIndex,
        bool _isAttack,
        bytes calldata _stateData,
        bytes calldata _proof
    )
        public
        virtual
    {
        // INVARIANT: Steps cannot be made unless the game is currently in progress.
        if (status != GameStatus.IN_PROGRESS) revert GameNotInProgress();

        // Get the parent. If it does not exist, the call will revert with OOB.
        ClaimData storage parent = claimData[_claimIndex];

        // Pull the parent position out of storage.
        Position parentPos = parent.position;
        // Determine the position of the step.
        Position stepPos = parentPos.move(_isAttack);

        // INVARIANT: A step cannot be made unless the move position is 1 below the `MAX_GAME_DEPTH`
        if (stepPos.depth() != MAX_GAME_DEPTH + 1) revert InvalidParent();

        // Determine the expected pre & post states of the step.
        Claim preStateClaim;
        ClaimData storage postState;
        if (_isAttack) {
            // If the step position's index at depth is 0, the prestate is the absolute
            // prestate.
            // If the step is an attack at a trace index > 0, the prestate exists elsewhere in
            // the game state.
            // NOTE: We localize the `indexAtDepth` for the current execution trace subgame by finding
            //       the remainder of the index at depth divided by 2 ** (MAX_GAME_DEPTH - SPLIT_DEPTH),
            //       which is the number of leaves in each execution trace subgame. This is so that we can
            //       determine whether or not the step position is represents the `ABSOLUTE_PRESTATE`.
            preStateClaim = (stepPos.indexAtDepth() % (1 << (MAX_GAME_DEPTH - SPLIT_DEPTH))) == 0
                ? ABSOLUTE_PRESTATE
                : _findTraceAncestor(Position.wrap(parentPos.raw() - 1), parent.parentIndex, false).claim;
            // For all attacks, the poststate is the parent claim.
            postState = parent;
        } else {
            // If the step is a defense, the poststate exists elsewhere in the game state,
            // and the parent claim is the expected pre-state.
            preStateClaim = parent.claim;
            postState = _findTraceAncestor(Position.wrap(parentPos.raw() + 1), parent.parentIndex, false);
        }

        // INVARIANT: The prestate is always invalid if the passed `_stateData` is not the
        //            preimage of the prestate claim hash.
        //            We ignore the highest order byte of the digest because it is used to
        //            indicate the VM Status and is added after the digest is computed.
        if (keccak256(_stateData) << 8 != preStateClaim.raw() << 8) revert InvalidPrestate();

        // Compute the local preimage context for the step.
        Hash uuid = _findLocalContext(_claimIndex);

        // INVARIANT: If a step is an attack, the poststate is valid if the step produces
        //            the same poststate hash as the parent claim's value.
        //            If a step is a defense:
        //              1. If the parent claim and the found post state agree with each other
        //                 (depth diff % 2 == 0), the step is valid if it produces the same
        //                 state hash as the post state's claim.
        //              2. If the parent claim and the found post state disagree with each other
        //                 (depth diff % 2 != 0), the parent cannot be countered unless the step
        //                 produces the same state hash as `postState.claim`.
        // SAFETY:    While the `attack` path does not need an extra check for the post
        //            state's depth in relation to the parent, we don't need another
        //            branch because (n - n) % 2 == 0.
        bool validStep = VM.step(_stateData, _proof, uuid.raw()) == postState.claim.raw();
        bool parentPostAgree = (parentPos.depth() - postState.position.depth()) % 2 == 0;
        if (parentPostAgree == validStep) revert ValidStep();

        // INVARIANT: A step cannot be made against a claim for a second time.
        if (parent.counteredBy != address(0)) revert DuplicateStep();

        // Set the parent claim as countered. We do not need to append a new claim to the game;
        // instead, we can just set the existing parent as countered.
        parent.counteredBy = msg.sender;//@audit
    }
```

This function is used to step either in the direction of an attack/defense, issue however is that this approach is not done in a commit-reveal pattern, which then means that a tech savvy user can just frontrun honest users attempt to `step` by passing in the same tx data with more gas and steal the bonds from the honest users.

### Impact

Borderline low/medium.

> This bug case is quite similar to the popular suggestion of _using a commit-reveal scheme for liquidations_, so depending on the protocol's intention this could be QA or not.

### Recommended Mitigation Steps

Consider ensuring even not so _savvy_ users can receive rewards for their contributions.

## QA-05 The subtle invariant of requiring an EOA to be the one querying `addLeavesLPP` can easily be broken

### Proof of Concept

Take a look at https://github.com/code-423n4/2024-07-optimism/blob/2e11e15e7a9a86f90de090ebf9e3516279e30897/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L417-L438

```solidity
    function initLPP(uint256 _uuid, uint32 _partOffset, uint32 _claimedSize) external payable {
        // The bond provided must be at least `MIN_BOND_SIZE`.
        if (msg.value < MIN_BOND_SIZE) revert InsufficientBond();

        // The caller of `addLeavesLPP` must be an EOA, so that the call inputs are always available in block bodies.
        if (msg.sender != tx.origin) revert NotEOA();

        // The part offset must be within the bounds of the claimed size + 8.
        if (_partOffset >= _claimedSize + 8) revert PartOffsetOOB();

        // The claimed size must be at least `MIN_LPP_SIZE_BYTES`.
        if (_claimedSize < MIN_LPP_SIZE_BYTES) revert InvalidInputSize();

        // Initialize the proposal metadata.
        LPPMetaData metaData = proposalMetadata[msg.sender][_uuid];
        proposalMetadata[msg.sender][_uuid] = metaData.setPartOffset(_partOffset).setClaimedSize(_claimedSize);
        proposals.push(LargePreimageProposalKeys(msg.sender, _uuid));

        // Assign the bond to the proposal.
        proposalBonds[msg.sender][_uuid] = msg.value;
    }

```

This funtion is used to initialize a large preimage proposal and it must be called before adding any leaves. Per the [doumentation](https://github.com/code-423n4/2024-07-optimism/blob/2e11e15e7a9a86f90de090ebf9e3516279e30897/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L421), the caller of `addLeavesLPP` must be an EOA, so that the call inputs are always available in block bodies, which is why this check exists `    if (msg.sender != tx.origin) revert NotEOA();`, however this concept is flawed cause the check can easily be sidestepped. This is because with the introduction of `EIP 3074` a user might be directly integrating with their assets, but not as the `msg.sender`.

There are [stale comments](https://github.com/code-423n4/2024-07-optimism/blob/2e11e15e7a9a86f90de090ebf9e3516279e30897/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L463) showcasing protocol is somehow in the know of this issue, however this comments suggest that protocol thinks EIP-3074 has not been introduced, however, `EIP-3074` has _officially_ been included in the next Ethereum hard fork Pectra upgrade (Prague upgrade for short) ... [source](https://www.coinlive.com/news/eip-3074-is-included-in-the-ethereum-prague-upgrade-learn-eip3074), so we can conclude that the new [functionalities from the proposal](https://eips.ethereum.org/EIPS/eip-3074) has been finalized and is to be adopted, which means that even normal EOAs would not be able to integrate with the protocol.

### Impact

QA

### Recommended Mitigation Steps

Consider having a work around for this or explicit documentation.

## QA-06 Min bond ether is quite small & makes it easy for the whale attack from the readMe to be possible

### Proof of Concept

Take a look at https://github.com/code-423n4/2024-07-optimism/blob/2e11e15e7a9a86f90de090ebf9e3516279e30897/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L24-L25

```solidity
    /// @notice The minimum bond size for large preimage proposals.
    uint256 public constant MIN_BOND_SIZE = 0.25 ether;
```

### Impact

### Recommended Mitigation Steps

## QA-07 Sometimes defense against invalid claims are not allowed

### Proof of Concept

Take a look at https://github.com/code-423n4/2024-07-optimism/blob/2e11e15e7a9a86f90de090ebf9e3516279e30897/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L610-L625

```solidity
            // INVARIANT: Cannot resolve a subgame containing an unresolved claim
            if (!resolvedSubgames[challengeIndex]) revert OutOfOrderResolution();

            ClaimData storage claim = claimData[challengeIndex];

            // If the child subgame is uncountered and further left than the current left-most counter,
            // update the parent subgame's `countered` address and the current `leftmostCounter`.
            // The left-most correct counter is preferred in bond payouts in order to discourage attackers
            // from countering invalid subgame roots via an invalid defense position. As such positions
            // cannot be correctly countered.
            // Note that correctly positioned defense, but invalid claimes can still be successfully countered.
            if (claim.counteredBy == address(0) && checkpoint.leftmostPosition.raw() > claim.position.raw()) {
                checkpoint.counteredBy = claim.claimant;
                checkpoint.leftmostPosition = claim.position;
            }
        }
```

The comment indicates that the system prefers the left-most correct counter when awarding bonds to discourage attackers from countering invalid subgame roots with invalid defense positions. The logic behind this is that an "invalid defense against an invalid claim" is technically considered a "freeloader claim." If such a claim goes uncontested, the overall resolution remains correct. However, it's implied that these types of defenses cannot be properly disputed even if someone wants to.

Issue however is that this design implies that there would be valid states where "invalid defenses" cannot be correctly countered, and thus, they remain uncontested.

### Impact

1. **Freeloader Claims**: The logic treats an invalid defense as a "freeloader claim." If such a claim remains uncontested, it incorrectly appears valid, leading to potential manipulation of the game's outcome.
2. **Undisputable Invalid Defenses**: Preventing the dispute of invalid defenses means that even if there is a clear case against such a defense, the system disallows it, leading to unresolved invalid actions.
3. **Incentivized Malicious Behavior**: This design could incentivize attackers to exploit the system by creating invalid defenses, knowing they cannot be disputed, and potentially gain bonds illegitimately.

### Recommended Mitigation Steps

Consider modifying the logic to permit the dispute of invalid defenses. This ensures that any invalid actions can be challenged and resolved, maintaining the integrity of the game.

Alternatively, implement stricter validation checks to identify and mark invalid defenses. This helps in preventing freeloader claims from going uncontested.

## QA-08 `CannonTypes#partOffset()` passes it's return value in the _wrong_ unsigned integer size

### Proof of Concept

Take a look at https://github.com/code-423n4/2024-07-optimism/blob/2e11e15e7a9a86f90de090ebf9e3516279e30897/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L65-L70

```solidity

    function partOffset(LPPMetaData _self) internal pure returns (uint64 partOffset_) {
        assembly {
            partOffset_ := and(shr(160, _self), U32_MASK)
        }
    }
```

Also see https://github.com/code-423n4/2024-07-optimism/blob/2e11e15e7a9a86f90de090ebf9e3516279e30897/packages/contracts-bedrock/src/cannon/libraries/CannonTypes.sol#L6-L17

```solidity
/// @notice Packed LPP metadata.
/// ┌─────────────┬────────────────────────────────────────────┐
/// │ Bit Offsets │                Description                 │
/// ├─────────────┼────────────────────────────────────────────┤
/// │ [0, 64)     │ Timestamp (Finalized - All data available) │
/// │ [64, 96)    │ Part Offset                                │
/// │ [96, 128)   │ Claimed Size                               │
/// │ [128, 160)  │ Blocks Processed (Inclusive of Padding)    │
/// │ [160, 192)  │ Bytes Processed (Non-inclusive of Padding) │
/// │ [192, 256)  │ Countered                                  │
/// └─────────────┴────────────────────────────────────────────┘
t
```

Evidently the `Part Offset` covers 32 bits, however unlike other functions that return their values in the uint they represent, `partOffset` returns it's own in 2x the value.

### Impact

QA

### Recommended Mitigation Steps

Consider applying these changes:

```diff

-    function partOffset(LPPMetaData _self) internal pure returns (uint64 partOffset_) {
+    function partOffset(LPPMetaData _self) internal pure returns (uint32 partOffset_) {
        assembly {
            partOffset_ := and(shr(160, _self), U32_MASK)
        }
    }
```

## QA-09 An owner can always ensure dispute games are not created

### Proof of Concept

Take a look at https://github.com/code-423n4/2024-07-optimism/blob/2e11e15e7a9a86f90de090ebf9e3516279e30897/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L205-L208

```solidity
    function setInitBond(GameType _gameType, uint256 _initBond) external onlyOwner {
        initBonds[_gameType] = _initBond;
        emit InitBondUpdated(_gameType, _initBond);
    }
```

This function sets the init bond.

Now see https://github.com/code-423n4/2024-07-optimism/blob/2e11e15e7a9a86f90de090ebf9e3516279e30897/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L84-L132

```solidity
    function create(
        GameType _gameType,
        Claim _rootClaim,
        bytes calldata _extraData
    )
        external
        payable
        returns (IDisputeGame proxy_)
    {
        // Grab the implementation contract for the given `GameType`.
        IDisputeGame impl = gameImpls[_gameType];

        // If there is no implementation to clone for the given `GameType`, revert.
        if (address(impl) == address(0)) revert NoImplementation(_gameType);

        // If the required initialization bond is not met, revert.
        if (msg.value != initBonds[_gameType]) revert IncorrectBondAmount();

        // Get the hash of the parent block.
        bytes32 parentHash = blockhash(block.number - 1);

        // Clone the implementation contract and initialize it with the given parameters.
        //
        // CWIA Calldata Layout:
        // ┌──────────────┬────────────────────────────────────┐
        // │    Bytes     │            Description             │
        // ├──────────────┼────────────────────────────────────┤
        // │ [0, 20)      │ Game creator address               │
        // │ [20, 52)     │ Root claim                         │
        // │ [52, 84)     │ Parent block hash at creation time │
        // │ [84, 84 + n) │ Extra data (opaque)                │
        // └──────────────┴────────────────────────────────────┘
        proxy_ = IDisputeGame(address(impl).clone(abi.encodePacked(msg.sender, _rootClaim, parentHash, _extraData)));
        proxy_.initialize{ value: msg.value }();

        // Compute the unique identifier for the dispute game.
        Hash uuid = getGameUUID(_gameType, _rootClaim, _extraData);

        // If a dispute game with the same UUID already exists, revert.
        if (GameId.unwrap(_disputeGames[uuid]) != bytes32(0)) revert GameAlreadyExists(uuid);

        // Pack the game ID.
        GameId id = LibGameId.pack(_gameType, Timestamp.wrap(uint64(block.timestamp)), address(proxy_));

        // Store the dispute game id in the mapping & emit the `DisputeGameCreated` event.
        _disputeGames[uuid] = id;
        _disputeGameList.push(id);
        emit DisputeGameCreated(address(proxy_), _gameType, _rootClaim);
    }
```

Evidently, there is a need to attach the correct bond value before creating a dispute game, which then allows an owner to be able to e sure no games are created by simply front running the calls to `create()` by changing the `bond` value.

### Impact

QA

Per C4's new rules o centralisation risks, this should be raised in an audit

### Recommended Mitigation Steps

Consider clearly documenting this fact, otherwise N/A.

### Proof of Concept

Take a look at https://github.com/code-423n4/2024-07-optimism/blob/2e11e15e7a9a86f90de090ebf9e3516279e30897/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L253-L256

```solidity

        // INVARIANT: A step cannot be made unless the move position is 1 below the `MAX_GAME_DEPTH`
        if (stepPos.depth() != MAX_GAME_DEPTH + 1) revert InvalidParent();

```

This is one of the checks before a step is being made, as seen there is a need to check that `stepPos.depth() == MAX_GAME_DEPTH + 1` however the comments suggest otherwise and instead hint that `A step cannot be made unless the move position is 1 below the `MAX_GAME_DEPTH``

### Impact

QA... confusing code for all integrators

### Recommended Mitigation Steps

Consider applying these changes

```diff

-        // INVARIANT: A step cannot be made unless the move position is 1 below the `MAX_GAME_DEPTH`
+        // INVARIANT: A step cannot be made unless the move position is 1 above the `MAX_GAME_DEPTH`
        if (stepPos.depth() != MAX_GAME_DEPTH + 1) revert InvalidParent();

```

### Proof of Concept

Take a look at https://github.com/code-423n4/2024-07-optimism/blob/2e11e15e7a9a86f90de090ebf9e3516279e30897/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L319-L325

```solidity
    function move(Claim _disputed, uint256 _challengeIndex, Claim _claim, bool _isAttack) public payable virtual {
        // INVARIANT: Moves cannot be made unless the game is currently in progress.
        if (status != GameStatus.IN_PROGRESS) revert GameNotInProgress();

        // Get the parent. If it does not exist, the call will revert with OOB.
        ClaimData memory parent = claimData[_challengeIndex];
//snip
}
```

As hinted by the documentation if the parent does not exist, it is expected for the query to revert with an OOB error, however this is not being included in the function.

### Impact

QA, function does not work as intended.

### Recommended Mitigation Steps

Consider checking if the parent is valid and exists, otherwise revert here: https://github.com/code-423n4/2024-07-optimism/blob/2e11e15e7a9a86f90de090ebf9e3516279e30897/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L324

## QA-10 Remove duplicate imports

### Proof of Concept

Take a look at https://github.com/code-423n4/2024-07-optimism/blob/2e11e15e7a9a86f90de090ebf9e3516279e30897/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L17-L21

```solidity
import { Types } from "src/libraries/Types.sol";//@audit
import { Hashing } from "src/libraries/Hashing.sol";
import { RLPReader } from "src/libraries/rlp/RLPReader.sol";
import "src/dispute/lib/Types.sol";
import "src/dispute/lib/Errors.sol";//@audit
```

Evidently and as has been indicated by the @audit tag, the `src/libraries/Types.sol` is being imported twice.

### Impact

QA, confusion in code.

### Recommended Mitigation Steps

Consider only the `src/libraries/Types.sol` once.

## QA-11 Return gas bomb when paying out bonds

### Proof of Concept

Take a look at https://github.com/code-423n4/2024-07-optimism/blob/2e11e15e7a9a86f90de090ebf9e3516279e30897/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L788-L794

```solidity
    function _payoutBond(address _claimant, uint256 _uuid, address _to) internal {
        // Pay out the bond to the claimant.
        uint256 bond = proposalBonds[_claimant][_uuid];
        proposalBonds[_claimant][_uuid] = 0;
        (bool success,) = _to.call{ value: bond }("");
        if (!success) revert BondTransferFailed();
    }
```

No limit at the amount of gas the call can spend at most.

### Impact
QA
### Recommended Mitigation Steps
Consider attaching a gas limit.

## QA-12 Import declarations should import specific identifiers, rather than the whole file

### Proof of Concept

Multiple instances in scope, for example take a look at https://github.com/code-423n4/2024-07-optimism/blob/2e11e15e7a9a86f90de090ebf9e3516279e30897/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L20-L21

```solidity
import "src/dispute/lib/Types.sol";
import "src/dispute/lib/Errors.sol";
```

Evidently, the imports being done is not name specific, but this is not the best implementation cause this could lead to polluting the symbol namespace.

### Impact

QA, albeit this could lead to the potential pollution of the symbol namespace and a slower compilation speed.

### Recommended Mitigation Steps

Consider using import declarations of the form `import {<identifier_name>} from "some/file.sol"` which avoids polluting the symbol namespace making flattened files smaller, and speeds up compilation (but does not save any gas).

This implementation is what's currently done in https://github.com/code-423n4/2024-07-optimism/blob/2e11e15e7a9a86f90de090ebf9e3516279e30897/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L4-L20

```solidity
import { FixedPointMathLib } from "@solady/utils/FixedPointMathLib.sol";

import { IDelayedWETH } from "src/dispute/interfaces/IDelayedWETH.sol";
import { IDisputeGame } from "src/dispute/interfaces/IDisputeGame.sol";
import { IFaultDisputeGame } from "src/dispute/interfaces/IFaultDisputeGame.sol";
import { IInitializable } from "src/dispute/interfaces/IInitializable.sol";
import { IBigStepper, IPreimageOracle } from "src/dispute/interfaces/IBigStepper.sol";
import { IAnchorStateRegistry } from "src/dispute/interfaces/IAnchorStateRegistry.sol";

import { Clone } from "@solady/utils/Clone.sol";
import { Types } from "src/libraries/Types.sol";
import { ISemver } from "src/universal/ISemver.sol";

import { Types } from "src/libraries/Types.sol";
import { Hashing } from "src/libraries/Hashing.sol";
import { RLPReader } from "src/libraries/rlp/RLPReader.sol";
import "src/dispute/lib/Types.sol";
```

## QA-13 Paying out bonds could silently fail

### Proof of Concept

Take a look at https://github.com/code-423n4/2024-07-optimism/blob/2e11e15e7a9a86f90de090ebf9e3516279e30897/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L788-L794

```solidity
    function _payoutBond(address _claimant, uint256 _uuid, address _to) internal {
        // Pay out the bond to the claimant.
        uint256 bond = proposalBonds[_claimant][_uuid];
        proposalBonds[_claimant][_uuid] = 0;
        (bool success,) = _to.call{ value: bond }("");
        if (!success) revert BondTransferFailed();
    }
```

This is how bonds are being paid out, issue is there are no checks to ensure the claimant is an existing/valid address, which could lead to silent failures when the bonds are being paid out.

Coded POC

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.21;
import "hardhat/console.sol";
contract test {
constructor() {
bytes memory txData*; (bool success,) = payable(address(0)).call{ value: 0 }(txData*); console.log("success",success);
}
}
```

### Impact
QA, heavy on the side of user mistake.
### Recommended Mitigation Steps
Consider integrating a check that the account is valid.

## QA-14 An admin can make everyone not be able to create valid bonds

### Proof of Concept

In order to create bonds this function is called: [DisputeGameFactory#create()]()

Issue however is that there is a check: https://github.com/code-423n4/2024-07-optimism/blob/2e11e15e7a9a86f90de090ebf9e3516279e30897/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L98-L100

```solidity

        // If the required initialization bond is not met, revert.
        if (msg.value != initBonds[_gameType]) revert IncorrectBondAmount();
```

And the `initBonds[_gameType]` value can be set by an admin, who can decide to put this very high and ensure no one can create anything

### Impact

QA

> _NB: Per C4's current rule, Centralization risks need to be attached to the QA report._

### Recommended Mitigation Steps

Consider hardcoding a maximum `initBonds` for each `gameType` to ensure the value is never set higher than this.

