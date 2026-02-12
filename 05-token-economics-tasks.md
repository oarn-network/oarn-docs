# Token Economics Tasks

## Overview
OARN uses a dual-token system: COMP (compute payments) and GOV (governance).

---

## 1. COMP Token Design

### Emission Schedule
- [ ] Define precise emission formula
- [ ] Year 1: 100M tokens
- [ ] Year 2: 80M tokens (20% reduction)
- [ ] Year 3: 64M tokens
- [ ] Halving every 2 years
- [ ] Target max supply: ~500M
- [ ] Create emission curve visualization

### Distribution Model
- [ ] 80% to Compute Nodes
  - [ ] Define reward calculation per task
  - [ ] Factor in task complexity
  - [ ] Factor in model size
  - [ ] Factor in response time
- [ ] 10% to Validators
  - [ ] Define validator fee structure
  - [ ] Validator-routed mode premium
- [ ] 5% to DAO Treasury
  - [ ] Automatic allocation
  - [ ] Multi-sig/timelock
- [ ] 5% to Protocol Development (2 years only)
  - [ ] Vesting schedule
  - [ ] Sunset clause

### Burn Mechanism
- [ ] 2% of all task payments burned
- [ ] Implement in smart contract
- [ ] Track total burned (transparency)

### Pricing Model
- [ ] Define compute unit (1 COMP = X tokens of inference)
- [ ] Adjust for model size (7B vs 70B)
- [ ] Adjust for task complexity
- [ ] Market-based discovery (supply/demand)

---

## 2. GOV Token Design

### Fixed Supply
- [ ] Total: 100,000,000 GOV
- [ ] No inflation
- [ ] No burn (preserve voting power)

### Initial Distribution
- [ ] 40% Early Compute Contributors
  - [ ] Define airdrop criteria
  - [ ] Snapshot mechanism
  - [ ] Claiming process
- [ ] 30% Public Genesis Sale
  - [ ] Cap per wallet (anti-whale)
  - [ ] Fair launch mechanism
  - [ ] No pre-sale, no VC allocation
- [ ] 20% DAO Treasury
  - [ ] Multi-year vesting (4 years)
  - [ ] Controlled by governance
- [ ] 10% Core Contributors
  - [ ] 4-year vesting
  - [ ] No cliff (cliffless)
  - [ ] Tied to contributions

### Utility
- [ ] 1 GOV = 1 base vote
- [ ] Quadratic voting implementation
- [ ] Validator staking (10,000 GOV minimum)
- [ ] Proposal submission (1,000 GOV stake)

---

## 3. Token Launch Strategy

### Pre-Launch (Months 0-6)
- [ ] No tokens in circulation
- [ ] Work tracked for future airdrop
- [ ] Build contribution leaderboard
- [ ] No speculative trading

### Genesis Launch (Month 6-9)
- [ ] Snapshot compute contributions
- [ ] Calculate airdrop amounts
- [ ] Public sale mechanism (if any)
- [ ] DEX liquidity provision

### Mature Phase (Month 9+)
- [ ] Full token circulation
- [ ] DEX trading enabled
- [ ] Market-driven pricing

---

## 4. Anti-Whale Mechanisms

### Governance Protections
- [ ] Quadratic voting: power = sqrt(tokens)
- [ ] Maximum 5% vote per address
- [ ] Time-lock: 30 days before voting
- [ ] Snapshot at proposal creation

### Sale Protections
- [ ] Per-wallet cap on genesis sale
- [ ] KYC-free but Sybil-resistant
- [ ] Gradual unlock (vesting)

### Compute Fairness
- [ ] Small node bonuses
- [ ] Geographic diversity rewards
- [ ] Diminishing returns for large operators

---

## 5. Economic Simulations

### Modeling Tasks
- [ ] Build token economy simulation
- [ ] Model various adoption scenarios:
  - [ ] Low adoption (1,000 nodes)
  - [ ] Medium adoption (100,000 nodes)
  - [ ] High adoption (1M+ nodes)
- [ ] Model attack scenarios:
  - [ ] Whale accumulation
  - [ ] Pump and dump
  - [ ] Compute manipulation
- [ ] Model sustainability:
  - [ ] Break-even analysis
  - [ ] Treasury runway

### Key Metrics to Model
- [ ] Token velocity
- [ ] Node profitability
- [ ] Researcher cost savings
- [ ] Validator returns
- [ ] Treasury growth

---

## 6. Price Discovery Mechanism

### Initial Phase (No Market)
- [ ] Internal pricing: 1 COMP = X GPU-seconds
- [ ] Algorithmic adjustment based on demand
- [ ] Floor price guarantee (from treasury)

### Market Phase
- [ ] DEX listing (Uniswap v3 on Arbitrum)
- [ ] Initial liquidity provision
- [ ] Arbitrage alignment
- [ ] Oracle integration (Chainlink)

---

## 7. Treasury Management

### Revenue Sources
- [ ] Transaction fees (2-3%)
- [ ] Validator staking rewards
- [ ] Premium features (private tasks)
- [ ] Optional donations

### Expenses
- [ ] Infrastructure (bootstrap nodes, IPFS)
- [ ] Developer grants
- [ ] Security audits
- [ ] Marketing/education
- [ ] Legal/compliance

### Controls
- [ ] Multi-sig wallet (3-of-5 initially)
- [ ] DAO governance (post-launch)
- [ ] Transparency reports (monthly)
- [ ] Spending proposals require vote

---

## 8. Validator Economics

### Staking Requirements
- [ ] Minimum stake: 10,000 GOV
- [ ] Slashing risk: Up to 10% for failures
- [ ] Cooldown: 7 days to unstake

### Revenue Model
- [ ] % of validated task fees
- [ ] Premium for validator-routed tasks
- [ ] Reputation bonuses

### Premium Node Registration
- [ ] Validators register high-performance nodes
- [ ] SLA requirements defined
- [ ] Performance-based rewards

---

## 9. Incentive Alignment

### Node Operators
- [ ] Earn COMP for completed tasks
- [ ] Reputation builds over time
- [ ] Long-term holders earn GOV airdrop

### Researchers
- [ ] Pay COMP for compute
- [ ] Cheaper than cloud alternatives
- [ ] Contribute compute to earn tokens

### Validators
- [ ] Stake GOV to earn fees
- [ ] Risk slashing for bad behavior
- [ ] High uptime rewarded

### Developers
- [ ] Grants from DAO treasury
- [ ] Earn GOV for contributions
- [ ] Build on open platform

---

## 10. Legal Considerations

### Token Classification
- [ ] Utility token design (not security)
- [ ] No profit expectations from others' efforts
- [ ] Work-based distribution
- [ ] Legal opinion from crypto law firm

### Geographic Restrictions
- [ ] Research US/EU regulations
- [ ] Geofencing if necessary
- [ ] OFAC compliance

---

## 11. Documentation

- [ ] Token economics whitepaper section (done in main whitepaper)
- [ ] Visual token flow diagrams
- [ ] Emission schedule calculator
- [ ] FAQ for common questions
- [ ] Blog posts explaining mechanics

---

## 12. Implementation Checklist

### Smart Contracts
- [ ] COMP token (ERC-20)
- [ ] GOV token (ERC-20 + voting)
- [ ] Emission controller
- [ ] Burn mechanism
- [ ] Vesting contracts
- [ ] Airdrop distributor

### Off-Chain
- [ ] Contribution tracking system
- [ ] Airdrop calculation scripts
- [ ] Treasury dashboard
- [ ] Analytics for token metrics
