# OARN Network
## Open AI Research Network

### The Problem
- AI compute is centralized in the hands of a few big tech companies
- Researchers lack affordable access to GPU/TPU resources
- No transparent, verifiable AI inference marketplace exists
- Privacy concerns when running models on centralized cloud providers

### The Solution
**OARN Network** is a decentralized infrastructure for AI research and inference.

- **Run a Node** - Contribute compute resources and earn rewards
- **Submit Tasks** - Pay for AI inference with multi-node consensus
- **Govern the Network** - Vote on proposals with GOV tokens
- **Verified Results** - Multi-node consensus ensures accurate outputs

### How It Works
```
1. User submits AI task (model + input) with reward
2. Multiple nodes claim and execute the task
3. Consensus algorithm verifies matching results
4. Nodes receive ETH + COMP token rewards
5. User receives verified inference output
```

### Key Features

| Feature | Description |
|---------|-------------|
| **Multi-Node Consensus** | 3+ nodes verify each result (Majority, SuperMajority, Unanimous) |
| **ONNX Runtime** | Industry-standard ML inference framework |
| **IPFS Storage** | Decentralized model and data storage |
| **P2P Network** | libp2p-based peer discovery (mDNS + DHT) |
| **On-Chain Registry** | All infrastructure discovered via smart contracts |
| **Governance** | Token-based voting for network upgrades |

### Token Economics

| Token | Purpose |
|-------|---------|
| **COMP** | Computation rewards for node operators |
| **GOV** | Governance voting rights |

### Tech Stack
- **Blockchain**: Arbitrum (L2 on Ethereum)
- **Node Software**: Rust + libp2p + ONNX Runtime
- **Smart Contracts**: Solidity + Hardhat
- **Storage**: IPFS

### Current Status
- Testnet live on Arbitrum Sepolia
- Multi-node consensus verified (3/3)
- 190 contract tests passing
- SDK v0.2.0 on npm (@oarnnetwork/sdk)
- Crowdfunding feature live (fundTask)
- GENESIS-001 MVP complete
- Docker deployment ready

### Raising
- **Round:** Seed ($1.5M target)
- **Structure:** SAFE
- **Use:** Engineering, Infrastructure, Community

### Links
- Website: https://oarn.network
- GitHub: https://github.com/oarn-network
- Discord: https://discord.gg/RsrQwNvt
- Twitter: https://twitter.com/OARNNetwork

### Get Involved
1. **Run a Node**: `docker-compose up -d`
2. **Submit Tasks**: Use the CLI or SDK
3. **Join Governance**: Hold GOV tokens and vote

---
*Decentralizing AI, one inference at a time.*
