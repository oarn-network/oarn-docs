# OARN Network - Internal Security Review

**Date:** 2026-03-01 (updated 2026-03-26 — WetLabOracle addendum)
**Reviewer:** Internal Security Audit
**Scope:** Smart Contracts + Node Software

---

## Executive Summary

| Component | Critical | High | Medium | Low | Info |
|-----------|----------|------|--------|-----|------|
| Smart Contracts (original) | 1 | 3 | 5 | 5 | 5 |
| Node (Rust) | 0 | 2 | 5 | 5 | 0 |
| **WetLabOracle (added 2026-03-26)** | **0** | **2** | **5** | **3** | **0** |
| **Total** | **1** | **7** | **15** | **13** | **5** |

**Overall Risk:** MEDIUM - Critical issue exists in deprecated V1 contract (not used in production). WetLabOracle is live on testnet with two new High severity findings requiring attention before mainnet.

---

## Smart Contract Findings

### Critical

#### C-1: Reentrancy in TaskRegistry._completeTask() [V1 ONLY]
**File:** `TaskRegistry.sol:280-303`
**Status:** NOT APPLICABLE - TaskRegistryV2 is used in production and has this fixed.

The old TaskRegistry contract updates state inside a loop while making external calls. TaskRegistryV2 correctly implements checks-effects-interactions pattern.

---

### High Severity

#### H-1: Silent Reward Transfer Failures
**Files:** `TaskRegistry.sol:294-298`, `TaskRegistryV2.sol:395-400`
**Impact:** Nodes could permanently lose rewards if ETH transfer fails.

```solidity
(bool success, ) = nodesToReward[i].call{value: rewardPerNode}("");
if (success) {
    totalDistributed += rewardPerNode;
    // Failed transfers are silently ignored
}
```

**Recommendation:** Implement pull-based withdrawal or track failed transfers for later claim.

#### H-2: Front-Running on Task Claiming
**Files:** `TaskRegistryV2.sol:231-248`
**Impact:** Attackers can front-run profitable task claims.

**Recommendation:** Consider commit-reveal scheme or reputation-based claiming.

#### H-3: Centralized Dispute Resolution
**File:** `TaskRegistryV2.sol:420-432`
**Impact:** Owner can arbitrarily decide dispute outcomes.

**Recommendation:** Multi-sig or decentralized arbitration for mainnet.

---

### Medium Severity

| ID | Issue | File | Recommendation |
|----|-------|------|----------------|
| M-1 | Weak randomness in provider selection | OARNRegistry.sol | Consider Chainlink VRF |
| M-2 | Emission cap not enforced | COMPToken.sol | Implement per-year tracking |
| M-3 | Unbounded loop in getAvailableTasks | TaskRegistry.sol | Off-chain indexing |
| M-4 | DoS via unique result hashes | TaskRegistryV2.sol | Limit unique hashes |
| M-5 | Stake accounting after unstake | OARNRegistry.sol | Zero stakes on unstake |

---

### Low Severity

- L-1: Missing zero-address check for node
- L-2: External call in overloaded submitTask
- L-3: No minimum stake after slashing
- L-4: Missing event for emergency withdrawal
- L-5: ID scheme footgun (starts at 1)

---

## Node Software Findings

### High Severity

#### H-1: Private Key Logging Risk
**File:** `main.rs:71-76`
**Risk:** Error handling could leak key material.

**Recommendation:** Use opaque error messages, consider `secrecy` crate.

#### H-2: Plaintext Private Key in Config
**File:** `config.rs:262-288`
**Risk:** Config files with keys could be exposed.

**Recommendation:** Implement encrypted keystore support (marked TODO).

---

### Medium Severity

| ID | Issue | File | Recommendation |
|----|-------|------|----------------|
| M-1 | Detailed error info leakage | Multiple | Generic user errors, debug logging |
| M-2 | Unvalidated ONNX execution | compute.rs | Model validation, sandboxing |
| M-3 | No TLS cert pinning | discovery.rs | Certificate pinning for critical endpoints |
| M-4 | Race in task ID query | blockchain.rs | Parse ID from tx events |
| M-5 | Non-atomic active_tasks | compute.rs | Use AtomicUsize |

---

### Low Severity

- L-1: IPFS cache path traversal risk
- L-2: Hardcoded public IPFS gateways
- L-3: Unchecked .unwrap() calls
- L-4: Non-critical random usage
- L-5: P2P keypair not persisted

---

## Positive Findings

### Smart Contracts
- Solidity 0.8.24 provides overflow protection
- Proper use of OpenZeppelin libraries
- TaskRegistryV2 fixes V1 reentrancy
- Immutable core addresses in OARNRegistry
- Fixed supply GOV token

### Node Software
- Zero `unsafe` blocks
- Strong crypto libraries (sha2, sha3, ethers, libp2p/noise)
- rustls for TLS (no OpenSSL)
- Type-safe input parsing
- Dynamic infrastructure discovery
- Graceful shutdown handling

---

## Action Items

### Must Fix Before Mainnet

| Priority | Issue | Action |
|----------|-------|--------|
| HIGH | H-1 (Contracts) | Implement pull-based rewards or failed transfer tracking |
| HIGH | H-2 (Node) | Add encrypted keystore support |
| MEDIUM | M-2 (Contracts) | Enforce COMPToken emission caps |
| MEDIUM | M-4 (Node) | Parse task ID from transaction events |

### Should Fix

| Priority | Issue | Action |
|----------|-------|--------|
| MEDIUM | M-1 (Contracts) | Document randomness limitations |
| MEDIUM | M-2 (Node) | Add model validation/sandboxing |
| MEDIUM | M-5 (Node) | Make active_tasks atomic |

### Can Defer

- Front-running protection (H-2 Contracts) - complex, low immediate risk
- Decentralized disputes (H-3 Contracts) - governance feature
- TLS pinning (M-3 Node) - defense in depth
- P2P key persistence (L-5 Node) - convenience feature

---

## Dependency Audit Status

### Contracts
- OpenZeppelin: Latest secure version
- Solidity 0.8.24: No known vulnerabilities

### Node (from cargo-audit 2026-02-28)
- 2 vulnerabilities (in transitive deps, blocked by upstream)
- `dotenv` replaced with `dotenvy`

---

---

## WetLabOracle Security Review — 2026-03-26

**Contract:** `WetLabOracle.sol` — deployed 2026-03-21 at `0xF8991A56cB5B9073a3eEC87E95Dfb055fdDF0094` (Arbitrum Sepolia)

### Overview

WetLabOracle allows a set of owner-certified labs to submit physical experiment results on-chain. When `requiredLabConfirmations` labs agree on the same `resultHash` for a task, consensus is reached and GOV token rewards are credited to each confirming lab's `pendingRewards` balance for pull-based claiming.

---

### High Severity

#### W-H1: Reward Pool Insolvency — Rewards Credited Without Balance Check
**File:** `WetLabOracle.sol:222-231` (`_checkConsensus`)
**Impact:** Labs can have `pendingRewards` credited that they can never claim.

```solidity
// Inside _checkConsensus — no balance check before crediting
pendingRewards[lab] += rewardPerVerification;
```

If total credited rewards exceed the GOV balance held by the contract, `claimReward()` will revert with "GOV transfer failed" for the last labs to claim. Labs that ran physical experiments would be unable to collect earned rewards.

**Recommendation:** Before crediting, verify the pool has sufficient balance:
```solidity
uint256 needed = rewardPerVerification * count;
require(IERC20(govToken).balanceOf(address(this)) >= needed, "Insufficient reward pool");
```
Or add an `emitInsufficientPool` warning event and skip reward crediting rather than reverting — this lets consensus still be recorded even if the pool is empty.

**Status:** Must fix before mainnet.

---

#### W-H2: Single-Step Ownership Transfer (Ownable, not Ownable2Step)
**File:** `WetLabOracle.sol:23`
**Impact:** A mistyped address in `transferOwnership()` would permanently lose admin capability: no one could certify labs, adjust rewards, or deposit GOV.

**Recommendation:** Upgrade to `Ownable2Step`:
```solidity
import "@openzeppelin/contracts/access/Ownable2Step.sol";
contract WetLabOracle is Ownable2Step, ReentrancyGuard {
```

**Status:** Should fix before mainnet.

---

### Medium Severity

| ID | Issue | File | Recommendation |
|----|-------|------|----------------|
| W-M1 | `oarnRegistry` is stored and validated but never used anywhere in the contract | `:47` | Document as reserved for future task validation, or remove |
| W-M2 | No `withdrawExcessRewards()` — owner cannot recover mistakenly over-deposited GOV | `:168` | Add owner-only withdrawal capped to `balance - sum(pendingRewards)` |
| W-M3 | `setRequiredConfirmations` has no upper bound — could be set higher than certified lab count, permanently blocking consensus | `:251` | Add `require(newRequired <= taskLabSubmitters[taskId].length)` or global lab count cap |
| W-M4 | `setRewardPerVerification` has no bounds — can be set to 0 or astronomically high | `:243` | Add `require(newReward > 0 && newReward <= MAX_REWARD)` |
| W-M5 | No way to invalidate consensus after the fact — owner cannot override a bad result post-consensus | Design | Consider owner-only `invalidateConsensus(taskId)` for exceptional cases |

---

### Low Severity

| ID | Issue | Recommendation |
|----|-------|----------------|
| W-L1 | `metric` string has no length cap — large strings inflate gas cost | `require(bytes(metric).length <= 64)` |
| W-L2 | O(n²) `_checkConsensus` loop acceptable for small certified lab sets but undocumented | Add NatSpec: "Expected ≤20 certified labs per task" |
| W-L3 | `block.timestamp` used for `submittedAt` / `verifiedAt` — manipulable within ~15s | Acceptable for this use case; document |

---

### Positive Findings

- **Checks-Effects-Interactions correctly applied** in `claimReward()` — state zeroed before token transfer ✅
- **`nonReentrant` guards** on all state-changing external functions ✅
- **Pull-based rewards** — avoids the silent transfer failure issue (H-1) found in TaskRegistryV2 ✅
- **`requiredLabConfirmations >= 2` enforced** in both constructor and setter ✅
- **Zero-address checks** on constructor arguments ✅
- **`rewarded` flag** prevents double-crediting within `_checkConsensus` ✅
- **No external calls inside `_checkConsensus`** — reentrancy-safe ✅
- **Immutable `govToken`** prevents admin from swapping the reward token ✅

---

### Action Items for WetLabOracle

| Priority | ID | Action |
|----------|----|--------|
| **HIGH** | W-H1 | Add reward pool balance check before crediting in `_checkConsensus` |
| **HIGH** | W-H2 | Upgrade to `Ownable2Step` |
| MEDIUM | W-M2 | Add `withdrawExcessRewards()` for owner fund recovery |
| MEDIUM | W-M3/M4 | Add bounds validation on `setRequiredConfirmations` and `setRewardPerVerification` |
| LOW | W-M1 | Document or remove unused `oarnRegistry` field |

**Security posture:** Acceptable for testnet. Both High findings require code changes before mainnet deployment. None are exploitable by external actors on testnet (all require either owner action or certified lab access).

---

## Conclusion

The OARN codebase demonstrates good security practices overall. The critical reentrancy issue exists only in the deprecated V1 contract. For mainnet launch:

1. **Required:** Fix reward distribution failure handling
2. **Required:** Add encrypted keystore support
3. **Required:** Enforce emission caps
4. **Recommended:** Document accepted risks (randomness, centralized disputes)

The multi-node consensus mechanism has been tested and works correctly. Security posture is **acceptable for testnet** and **needs improvements for mainnet**.
