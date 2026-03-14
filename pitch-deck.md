# OARN Network - Investor Pitch Deck

**Open AI Research Network**

*"Decentralized AI Infrastructure for Everyone"*

---

## Market Opportunity

| Metric | Value |
|--------|-------|
| AI Compute Market by 2030 | **$200B+** |
| Companies Controlling 90% | **3** (AWS, Google, Azure) |
| Cost Reduction Potential | **10x** |

---

## The Problem

### Centralized Control
AWS, Google, and Azure control 90% of AI compute. Single points of failure, censorship risk, and vendor lock-in trap users.

### Unaffordable Access
GPU costs $3-5/hour. Small researchers and startups are priced out. Innovation is concentrated in Big Tech.

### No Verification
Cloud providers can return wrong results. There's no way to verify that AI outputs are correct or untampered.

### Privacy Concerns
Sensitive research data must be sent to centralized servers. IP theft and surveillance risks are real.

### Idle GPU Capacity
Millions of consumer GPUs sit idle worldwide. No marketplace exists to monetize spare compute power.

### Vendor Lock-in
Proprietary APIs, formats, and workflows. High switching costs keep users trapped in ecosystems.

---

## Our Solution

A **decentralized network** where anyone can provide AI compute and get **verified results** through multi-node consensus.

### Multi-Node Consensus
3+ nodes execute each task independently. Results are compared on-chain. Impossible to cheat or return false results.

### ONNX Runtime
Industry-standard ML framework. Run any model on any hardware. No vendor lock-in.

### Built on Arbitrum
Low fees ($0.01/tx), fast finality (250ms), Ethereum security. Perfect for microtransactions.

---

## How It Works

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│ 1. Submit   │───▶│ 2. Execute  │───▶│ 3. Consensus│───▶│ 4. Rewards  │
│    Task     │    │    (Nodes)  │    │   (On-chain)│    │   (Paid)    │
└─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
```

1. **Submit Task** - Upload model + input to IPFS. Set reward and consensus level.
2. **Nodes Execute** - Multiple nodes claim and run inference independently.
3. **Consensus** - Results compared on-chain. Agreement = verified output.
4. **Rewards** - Honest nodes earn ETH + COMP tokens automatically.

---

## Technology Stack

| Component | Technology | Why |
|-----------|------------|-----|
| Blockchain | Arbitrum (Ethereum L2) | Low fees, high throughput, EVM compatible |
| Node Software | Rust + libp2p | Memory safe, fast, true P2P networking |
| ML Runtime | ONNX Runtime | Industry standard, any model format |
| Storage | IPFS | Decentralized, content-addressed |
| Discovery | ENS + DHT | No hardcoded servers, fully decentralized |

### Metrics

| Metric | Value |
|--------|-------|
| Tests Passing | **190** |
| Open Source | **100%** |
| Status | **Live on Testnet** |

---

## Competitive Landscape

| Feature | Render | Akash | io.net | **OARN** |
|---------|--------|-------|--------|----------|
| Consensus Verification | ❌ | ❌ | ❌ | ✅ |
| AI/ML Native | ❌ | ❌ | ✅ | ✅ |
| On-chain Verification | ❌ | ❌ | ❌ | ✅ |
| Decentralized | ✅ | ✅ | ❌ | ✅ |
| ONNX Support | ❌ | ❌ | ❌ | ✅ |

**Key differentiator:** Verifiable results through on-chain consensus. No other DePIN network offers cryptographic proof that AI outputs are correct.

---

## Use Cases

### GENESIS-001: Drug Discovery
Optimize insulin synthesis parameters using 10,000 AI model variations. Target: 67% yield (vs 40% standard), $28/gram (vs $50).

**Status: MVP Complete** ✅

### Academic Research
Universities can run large-scale ML experiments without cloud vendor lock-in. Verified results for reproducible science.

### Privacy-Sensitive AI
Medical imaging, financial models, proprietary data. No central server sees your data.

### AI Marketplaces
Developers deploy models, users pay per inference. Built-in payments and verification.

---

## Dual Token Model

### GOV Token (Governance)

| Property | Value |
|----------|-------|
| Supply | 100M (Fixed) |
| Purpose | Governance voting |

**Distribution:**
- 40% - Early contributors
- 30% - Public sale
- 20% - DAO treasury
- 10% - Core team (4yr vest)

### COMP Token (Compute)

| Property | Value |
|----------|-------|
| Supply | Inflationary (decreasing) |
| Purpose | Compute rewards |

**Emission Schedule:**
- Year 1: 100M tokens
- Year 2: 80M (-20%)
- Year 3: 64M (-20%)
- Optional 2% burn on transfer

---

## Traction & Milestones

| Metric | Status |
|--------|--------|
| Testnet | ✅ Live on Arbitrum Sepolia |
| Node Consensus | ✅ 3/3 Verified |
| Gas Savings (Batch) | 99.99% |

### Completed

- ✅ Multi-node consensus (TaskRegistryV2) - 3+ nodes verify results
- ✅ Batch task processing - 10,000+ params in single transaction
- ✅ Real ONNX inference - Production ML execution
- ✅ SDK v0.2.0 published to npm (@oarnnetwork/sdk)
- ✅ Crowdfunding feature (fundTask) live
- ✅ Docker deployment ready
- ✅ Internal security review complete
- ✅ 190 tests passing across all contracts

---

## Roadmap

| Quarter | Milestone | Details |
|---------|-----------|---------|
| **Q1 2026** ✅ | Testnet Launch | Multi-node consensus, batch tasks, SDK |
| **Q2 2026** | GENESIS-001 | First real research task, 1000+ nodes |
| **Q3 2026** | Mainnet Launch | Arbitrum One, token generation |
| **Q4 2026** | Scale | 10K nodes, enterprise partnerships |

---

## Team

Building in public. Open source from day one.

### Core Contributors
Anonymous founding team with experience in blockchain, ML systems, and distributed computing.

### Open Collaboration
All code on GitHub. Community contributions welcome. Transparent development.

### Advisory
Seeking advisors in DeSci, DePIN, and AI research domains.

---

## Investment Opportunity

| Metric | Value |
|--------|-------|
| Seed Round Target | **$1.5M** |
| Runway | **18 months** |
| Investment Structure | **SAFE** |

### Use of Funds

| Category | Allocation |
|----------|------------|
| Engineering (node software, contracts, SDK) | 40% |
| Infrastructure (IPFS, RPC, bootstrap nodes) | 25% |
| Community & Marketing (grants, bounties) | 20% |
| Operations & Legal | 15% |

---

## Contact

**"Decentralizing AI, one inference at a time."**

| Channel | Link |
|---------|------|
| Website | [oarn.network](https://oarn.network) |
| GitHub | [github.com/oarn-network](https://github.com/oarn-network) |
| Email | [oarn@proton.me](mailto:oarn@proton.me) |
| Twitter | [@OARNNetwork](https://twitter.com/OARNNetwork) |
| Discord | [Join Discord](https://discord.gg/RsrQwNvt) |

---

*OARN Network - Open AI Research Network*

*Testnet Live on Arbitrum Sepolia*
