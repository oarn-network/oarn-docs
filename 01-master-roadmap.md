# OARN Master Roadmap & Task Tracker

## Overview
This document tracks all major milestones across the project lifecycle.

---

## Phase 1: Foundation (Months 0-3) - Q1 2026

### Completed
- [x] Whitepaper v0.1 released
- [x] GitHub repository public

### In Progress / Todo
- [ ] Core team formation (5-10 contributors)
- [ ] Technical specification finalized
- [ ] Smart contract development begins
- [ ] Proof of concept: 3 nodes sharing tasks
- [ ] Mock token system (testnet)
- [ ] Basic web interface

**Exit Criteria:** Working PoC with 3 nodes successfully distributing and completing a simple task.

---

## Phase 2: Alpha (Months 3-6) - Q2 2026

- [ ] Smart contracts deployed on Arbitrum testnet
- [ ] Node software v0.1 (CLI only)
- [ ] 50-100 alpha testers recruited
- [ ] First research task: Sentiment analysis with 10 model variants
- [ ] Working task distribution system
- [ ] Token minting & rewards functional
- [ ] Validator mechanism tested
- [ ] Local mode fully functional
- [ ] Validator-routed mode prototype

**Exit Criteria:** 100 testers running nodes, completing real tasks, earning testnet tokens.

---

## Phase 3: Beta (Months 6-9) - Q3 2026

- [ ] Public testnet launch
- [ ] 1,000+ nodes online
- [ ] Web interface for task submission
- [ ] First real research partnership (university/institution)
- [ ] Security audit completed (CertiK, Trail of Bits, or equivalent)
- [ ] Battle-tested smart contracts
- [ ] Community governance begins (testnet)
- [ ] Full documentation & tutorials published
- [ ] All three operational modes (local, standard, validator-routed) production-ready

**Exit Criteria:** Passed security audit, 1000+ active nodes, one research partner onboarded.

---

## Phase 4: Mainnet (Months 9-12) - Q4 2026

- [ ] Mainnet launch on Arbitrum
- [ ] Genesis token distribution (fair launch)
- [ ] DAO governance live
- [ ] 10,000+ nodes target
- [ ] First breakthrough research result published
- [ ] Fully decentralized network
- [ ] Founder control relinquished to DAO
- [ ] Self-sustaining ecosystem (revenue covers costs)

**Exit Criteria:** Network generating real research value, DAO operational, no single points of failure.

---

## Phase 5: Maturity (Year 2+) - 2027 and Beyond

- [ ] 1M+ nodes worldwide
- [ ] Partnerships with major research institutions
- [ ] Mobile node support (smartphones contribute idle compute)
- [ ] Specialized hardware exploration (ASIC for AI inference)
- [ ] Migration to sovereign blockchain (if needed)
- [ ] First life-saving discovery attributed to OARN

---

## Key Metrics to Track

| Metric | Phase 1 | Phase 2 | Phase 3 | Phase 4 | Phase 5 |
|--------|---------|---------|---------|---------|---------|
| Active Nodes | 3 | 100 | 1,000 | 10,000 | 1M+ |
| Tasks Completed | 10 | 1,000 | 100,000 | 1M | 100M+ |
| Research Partners | 0 | 0 | 1 | 10 | 100+ |
| Token Holders | 0 | 100 | 1,000 | 10,000 | 1M+ |
| Core Contributors | 5-10 | 20 | 50 | 100 | 500+ |

---

## Dependencies Between Workstreams

```
Smart Contracts ──────┐
                      ├──> Alpha Launch ──> Beta ──> Mainnet
Node Software ────────┤
                      │
P2P Infrastructure ───┤
                      │
Token Economics ──────┘

Security Audit ──────────────────────────> Beta (blocker)

Legal Review ────────────────────────────> Mainnet (blocker)

Research Partnership ────────────────────> Beta (nice-to-have)
```

---

## Security-First Requirements (BLOCKERS)

These items MUST be completed before any public release:

### Zero Hardcoded Values
- [ ] No hardcoded RPC URLs in codebase
- [ ] No hardcoded bootstrap node addresses
- [ ] No hardcoded contract addresses
- [ ] No hardcoded API keys (never use API keys)
- [ ] All discovery via DHT/ENS/on-chain registry

### Decentralized Discovery Infrastructure
- [ ] OARNRegistry.sol deployed
- [ ] ENS names registered (oarn-registry.eth, oarn-rpc.eth, oarn-bootstrap.eth)
- [ ] DHT discovery implemented
- [ ] Multiple fallback mechanisms working

### Anonymous Communication
- [ ] Tor integration functional
- [ ] .onion addresses for bootstrap nodes
- [ ] Traffic padding implemented
- [ ] No clearnet-only critical paths

### Build Security
- [ ] Reproducible builds verified
- [ ] Multi-signature releases
- [ ] IPFS distribution with content addressing

---

## Related Task Files

1. [Smart Contracts Tasks](./02-smart-contracts-tasks.md)
2. [Node Software Tasks](./03-node-software-tasks.md)
3. [Infrastructure Tasks](./04-infrastructure-tasks.md)
4. [Token Economics Tasks](./05-token-economics-tasks.md)
5. [Governance Tasks](./06-governance-tasks.md)
6. [Community & Partnerships](./07-community-tasks.md)
7. [Security Tasks](./08-security-tasks.md)
8. [Legal & Compliance](./09-legal-tasks.md)
9. [Competitor Analysis](./10-competitor-analysis.md)
10. [Security Hardening](./11-security-hardening.md) **(CRITICAL)**
