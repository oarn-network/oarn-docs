# OARN Implementation Guide

## Complete Step-by-Step Guide to Building the OpenAI Research Network

This guide provides a comprehensive, ordered checklist of everything needed to build OARN from concept to mainnet launch.

---

## Table of Contents

1. [Prerequisites & Setup](#1-prerequisites--setup)
2. [Phase 1: Foundation (Months 0-3)](#2-phase-1-foundation-months-0-3)
3. [Phase 2: Alpha (Months 3-6)](#3-phase-2-alpha-months-3-6)
4. [Phase 3: Beta (Months 6-9)](#4-phase-3-beta-months-6-9)
5. [Phase 4: Mainnet (Months 9-12)](#5-phase-4-mainnet-months-9-12)
6. [Accounts & Services Required](#6-accounts--services-required)
7. [Hardware Requirements](#7-hardware-requirements)
8. [Software & Tools](#8-software--tools)
9. [Budget Estimate](#9-budget-estimate)
10. [Team Requirements](#10-team-requirements)

---

## 1. Prerequisites & Setup

### 1.1 Accounts to Create (Do First)

| Account | Purpose | Priority | Cost |
|---------|---------|----------|------|
| **GitHub** | Code hosting, CI/CD | Critical | Free |
| **ENS Name** | oarn.eth, oarn-registry.eth, oarn-rpc.eth, oarn-bootstrap.eth | Critical | ~$50-200/year |
| **Alchemy** | Arbitrum RPC (backup) | High | Free tier |
| **Infura** | Arbitrum RPC (backup) | High | Free tier |
| **QuickNode** | Arbitrum RPC (backup) | Medium | Free tier |
| **Pinata** | IPFS pinning | High | Free tier to start |
| **Cloudflare** | DNS, CDN, DDoS protection | High | Free tier |
| **Discord** | Community server | High | Free |
| **Twitter/X** | Announcements | High | Free |
| **Gitcoin** | Grants program | Medium | Free |
| **Immunefi** | Bug bounty platform | Medium | % of bounties |
| **Arbiscan** | Contract verification | High | Free |

### 1.2 Development Wallets to Create

| Wallet | Purpose | Security Level |
|--------|---------|----------------|
| **Deployer Wallet** | Contract deployment | Hardware wallet (Ledger/Trezor) |
| **Multi-sig (3-of-5)** | Admin operations | Gnosis Safe |
| **Testnet Wallet** | Testing | Software wallet OK |
| **Gas Wallet** | CI/CD deployments | Software wallet, minimal funds |

### 1.3 Domain & Identity Setup

```
┌─────────────────────────────────────────────────────────────┐
│                    Identity Setup Order                      │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  1. Register ENS names (oarn.eth, etc.)                     │
│     └── Requires ETH for registration                       │
│                                                             │
│  2. Create GitHub organization (github.com/oarn-network)    │
│     └── Enable 2FA, add branch protection                   │
│                                                             │
│  3. Create Discord server                                   │
│     └── Set up channels, roles, bots                        │
│                                                             │
│  4. Create Twitter/X account (@OARNetwork)                  │
│     └── Enable 2FA                                          │
│                                                             │
│  5. Set up Cloudflare for oarn.network domain              │
│     └── DNS, SSL, DDoS protection                           │
│                                                             │
│  6. Set up Gnosis Safe multi-sig                           │
│     └── Add 5 signers, configure 3-of-5                     │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 2. Phase 1: Foundation (Months 0-3)

### 2.1 Week 1-2: Project Setup

- [ ] **Create GitHub organization**
  ```bash
  # Repositories to create:
  oarn-network/whitepaper      # Documentation
  oarn-network/contracts       # Smart contracts
  oarn-network/node            # Node software (Rust)
  oarn-network/sdk-python      # Python SDK
  oarn-network/sdk-js          # JavaScript SDK
  oarn-network/website         # Public website
  oarn-network/docs            # Technical documentation
  ```

- [ ] **Initialize smart contracts repository**
  ```bash
  mkdir contracts && cd contracts
  npx hardhat init
  npm install @openzeppelin/contracts
  npm install @openzeppelin/contracts-upgradeable
  npm install @nomicfoundation/hardhat-toolbox
  npm install hardhat-deploy
  npm install dotenv
  ```

- [ ] **Initialize node software repository**
  ```bash
  cargo new oarn-node
  cd oarn-node
  # Add dependencies to Cargo.toml:
  # - libp2p (networking)
  # - ethers-rs (blockchain)
  # - tokio (async runtime)
  # - arti (Tor)
  # - clap (CLI)
  ```

- [ ] **Set up CI/CD pipelines**
  ```yaml
  # .github/workflows/contracts.yml
  # .github/workflows/node.yml
  # .github/workflows/docs.yml
  ```

### 2.2 Week 3-4: Smart Contract Development (Part 1)

- [ ] **Implement OARNRegistry.sol**
  - [ ] RPC provider registration/staking
  - [ ] Bootstrap node registration/staking
  - [ ] Random provider selection
  - [ ] Slashing mechanism
  - [ ] Unit tests (100% coverage)

- [ ] **Implement COMP Token (ERC-20)**
  - [ ] Minting function (controlled)
  - [ ] Burn mechanism (2%)
  - [ ] Emission schedule
  - [ ] Unit tests

- [ ] **Implement GOV Token (ERC-20 + Votes)**
  - [ ] Fixed supply (100M)
  - [ ] Delegation
  - [ ] Snapshot functionality
  - [ ] Unit tests

### 2.3 Week 5-6: Smart Contract Development (Part 2)

- [ ] **Implement TaskRegistry.sol**
  - [ ] Task submission
  - [ ] Task claiming
  - [ ] Result submission
  - [ ] Validator-routed mode
  - [ ] Payment distribution
  - [ ] Unit tests

- [ ] **Implement ValidatorRegistry.sol**
  - [ ] Validator staking
  - [ ] Premium node registration
  - [ ] Task routing
  - [ ] Attestation
  - [ ] Slashing
  - [ ] Unit tests

- [ ] **Implement Governance.sol**
  - [ ] Proposal creation
  - [ ] Quadratic voting
  - [ ] Execution with timelock
  - [ ] Unit tests

### 2.4 Week 7-8: Node Software Foundation

- [ ] **Implement CLI structure**
  ```
  oarn init          # Initialize node
  oarn start         # Start daemon
  oarn stop          # Stop daemon
  oarn status        # Show status
  oarn wallet create # Create wallet
  oarn wallet import # Import wallet
  ```

- [ ] **Implement wallet management**
  - [ ] BIP-39 seed generation
  - [ ] BIP-44 derivation paths
  - [ ] AES-256-GCM encryption
  - [ ] Argon2id key derivation
  - [ ] Hardware wallet support (stub)

- [ ] **Implement configuration system**
  - [ ] TOML config parsing
  - [ ] No hardcoded values
  - [ ] Environment variable support
  - [ ] Validation

### 2.5 Week 9-10: P2P Networking

- [ ] **Implement libp2p integration**
  - [ ] Node initialization
  - [ ] QUIC transport
  - [ ] Noise encryption
  - [ ] DHT (Kademlia)
  - [ ] Peer discovery

- [ ] **Implement decentralized discovery**
  - [ ] ENS resolution
  - [ ] DHT bootstrap node discovery
  - [ ] On-chain registry queries
  - [ ] Fallback mechanisms

- [ ] **Implement Tor integration**
  - [ ] arti (Rust Tor) integration
  - [ ] Hidden service creation
  - [ ] .onion address support

### 2.6 Week 11-12: Integration & Testing

- [ ] **Deploy to Arbitrum Sepolia testnet**
  - [ ] All contracts
  - [ ] Verify on Arbiscan
  - [ ] Test transactions

- [ ] **Create proof-of-concept**
  - [ ] 3 nodes communicating
  - [ ] Task submission and claiming
  - [ ] Result submission
  - [ ] Payment distribution

- [ ] **Internal security review**
  - [ ] Code audit (self)
  - [ ] Slither analysis
  - [ ] Mythril analysis

### Phase 1 Exit Criteria
- [ ] All core contracts deployed on testnet
- [ ] Node software can connect to network
- [ ] 3-node PoC working
- [ ] Basic documentation complete

---

## 3. Phase 2: Alpha (Months 3-6)

### 3.1 Month 4: Node Software Completion

- [ ] **Implement Ollama integration**
  - [ ] Ollama process management
  - [ ] Model loading/unloading
  - [ ] Inference execution
  - [ ] Result parsing

- [ ] **Implement IPFS integration**
  - [ ] IPFS client (kubo or js-ipfs)
  - [ ] Upload/download
  - [ ] Pinning
  - [ ] Model caching

- [ ] **Implement task execution pipeline**
  - [ ] Task discovery
  - [ ] Capability matching
  - [ ] Task claiming (blockchain)
  - [ ] Input download (IPFS)
  - [ ] Model execution
  - [ ] Result upload (IPFS)
  - [ ] Result submission (blockchain)

- [ ] **Implement privacy features**
  - [ ] Message padding
  - [ ] Random delays
  - [ ] Peer rotation
  - [ ] Decoy traffic (optional)

### 3.2 Month 5: Alpha Testing

- [ ] **Recruit 50-100 alpha testers**
  - [ ] Discord announcement
  - [ ] Twitter outreach
  - [ ] Crypto/AI communities

- [ ] **Set up alpha infrastructure**
  - [ ] 3 bootstrap nodes (testnet)
  - [ ] IPFS pinning node
  - [ ] Monitoring (Prometheus/Grafana)
  - [ ] Alerting

- [ ] **Create alpha documentation**
  - [ ] Installation guide
  - [ ] Configuration guide
  - [ ] Troubleshooting guide
  - [ ] FAQ

- [ ] **Run first research task**
  - [ ] Simple sentiment analysis
  - [ ] 10 model variants
  - [ ] Verify results aggregation

### 3.3 Month 6: Python SDK & Refinement

- [ ] **Implement Python SDK (oarn-py)**
  ```python
  from oarn import Network, LocalNode, Wallet

  # Local mode
  local = LocalNode(model="llama-3-8b")
  result = local.inference(prompt="...")

  # Network mode
  wallet = Wallet.load("~/.oarn/wallet.encrypted")
  net = Network(wallet=wallet)
  task = net.submit_task(...)
  results = task.wait()
  ```

- [ ] **Implement JavaScript SDK (oarn-js)**
  ```javascript
  import { Network, LocalNode, Wallet } from 'oarn';

  const wallet = await Wallet.load('~/.oarn/wallet.encrypted');
  const net = new Network({ wallet });
  const task = await net.submitTask({...});
  const results = await task.wait();
  ```

- [ ] **Fix bugs from alpha testing**

- [ ] **Improve performance based on feedback**

### Phase 2 Exit Criteria
- [ ] Node software v0.1 released
- [ ] 50+ alpha testers active
- [ ] First research task completed
- [ ] Python and JS SDKs working
- [ ] Token system functional (testnet)

---

## 4. Phase 3: Beta (Months 6-9)

### 4.1 Month 7: Security Audit Preparation

- [ ] **Code freeze for audit**
  - [ ] Feature complete
  - [ ] All tests passing
  - [ ] Documentation complete

- [ ] **Prepare audit materials**
  - [ ] Architecture documentation
  - [ ] Threat model
  - [ ] Code walkthrough
  - [ ] Known issues list

- [ ] **Select auditor**
  - [ ] Get quotes from: CertiK, Trail of Bits, OpenZeppelin, Consensys Diligence
  - [ ] Sign engagement letter
  - [ ] Provide code access

- [ ] **Internal penetration testing**
  - [ ] Node software security
  - [ ] P2P network attacks
  - [ ] Smart contract exploits

### 4.2 Month 8: Public Testnet

- [ ] **Launch public testnet**
  - [ ] Fresh contract deployment
  - [ ] Public bootstrap nodes (5 regions)
  - [ ] Public RPC endpoints

- [ ] **Scale to 1,000+ nodes**
  - [ ] Marketing push
  - [ ] Testnet incentives
  - [ ] Leaderboards

- [ ] **Launch web interface**
  - [ ] Task submission UI
  - [ ] Results viewer
  - [ ] Node dashboard
  - [ ] Wallet integration (MetaMask, WalletConnect)

- [ ] **Address audit findings**
  - [ ] Critical/High: Fix immediately
  - [ ] Medium: Fix before mainnet
  - [ ] Low: Document and plan

### 4.3 Month 9: Partnerships & Launch Prep

- [ ] **First research partnership**
  - [ ] University or research institution
  - [ ] Real research task
  - [ ] Case study

- [ ] **Bug bounty program**
  - [ ] Launch on Immunefi
  - [ ] Define severity levels
  - [ ] Fund bounty pool

- [ ] **Community governance (testnet)**
  - [ ] First proposals
  - [ ] Test voting
  - [ ] Refine parameters

- [ ] **Legal review**
  - [ ] Token legal opinion
  - [ ] Terms of service
  - [ ] Privacy policy

### Phase 3 Exit Criteria
- [ ] Security audit passed
- [ ] 1,000+ nodes on testnet
- [ ] Web interface live
- [ ] First research partnership signed
- [ ] Bug bounty program active
- [ ] Legal review complete

---

## 5. Phase 4: Mainnet (Months 9-12)

### 5.1 Month 10: Mainnet Preparation

- [ ] **Final security audit (re-audit)**
  - [ ] Verify all fixes
  - [ ] Final sign-off

- [ ] **Mainnet infrastructure**
  - [ ] 5+ bootstrap nodes (production)
  - [ ] 3+ IPFS pinning nodes
  - [ ] Multiple RPC providers registered
  - [ ] 24/7 monitoring

- [ ] **ENS configuration**
  - [ ] oarn-registry.eth → OARNRegistry contract
  - [ ] oarn-rpc.eth → TXT records with RPC providers
  - [ ] oarn-bootstrap.eth → TXT records with bootstrap nodes

- [ ] **Deployment rehearsal**
  - [ ] Full deployment on testnet
  - [ ] Verify all scripts
  - [ ] Emergency procedures tested

### 5.2 Month 11: Mainnet Launch

- [ ] **Deploy to Arbitrum One (Mainnet)**
  ```
  Deployment Order:
  1. GOV Token
  2. COMP Token
  3. OARNRegistry
  4. TaskRegistry
  5. ValidatorRegistry
  6. TokenReward
  7. Governance
  8. Configure all contracts
  9. Register initial RPC providers
  10. Register initial bootstrap nodes
  11. Update ENS records
  12. Verify all on Arbiscan
  ```

- [ ] **Genesis token distribution**
  - [ ] Snapshot compute contributions
  - [ ] Airdrop to early contributors
  - [ ] Public genesis sale (if any)

- [ ] **Launch announcement**
  - [ ] Blog post
  - [ ] Twitter thread
  - [ ] Discord announcement
  - [ ] Press release

- [ ] **Scale to 10,000+ nodes**

### 5.3 Month 12: DAO Transition

- [ ] **DAO governance live**
  - [ ] First real proposals
  - [ ] Treasury under DAO control

- [ ] **Founder transition**
  - [ ] Multi-sig control reduced
  - [ ] Founder voting power capped at 5%

- [ ] **First breakthrough research**
  - [ ] Published paper citing OARN
  - [ ] Real-world impact

### Phase 4 Exit Criteria
- [ ] Mainnet live and stable
- [ ] 10,000+ nodes
- [ ] DAO governance operational
- [ ] Self-sustaining economics
- [ ] Founder control relinquished

---

## 6. Accounts & Services Required

### 6.1 Critical (Must Have)

| Service | Account Name | Purpose | Setup Order |
|---------|--------------|---------|-------------|
| GitHub | oarn-network | Code hosting | Week 1 |
| ENS | oarn.eth | Identity | Week 1 |
| Gnosis Safe | Multi-sig | Admin wallet | Week 1 |
| Alchemy | oarn | RPC provider | Week 1 |
| Discord | OARN | Community | Week 1 |
| Twitter | @OARNetwork | Announcements | Week 1 |
| Cloudflare | oarn.network | DNS/CDN | Week 2 |
| Pinata | oarn | IPFS pinning | Week 3 |
| Arbiscan | - | Contract verify | Week 3 |

### 6.2 High Priority (Should Have)

| Service | Purpose | Setup Order |
|---------|---------|-------------|
| Infura | Backup RPC | Month 2 |
| QuickNode | Backup RPC | Month 2 |
| The Graph | Indexing | Month 3 |
| Immunefi | Bug bounty | Month 7 |
| Gitcoin | Grants | Month 6 |

### 6.3 Medium Priority (Nice to Have)

| Service | Purpose | Setup Order |
|---------|---------|-------------|
| Discourse | Forum | Month 4 |
| YouTube | Tutorials | Month 5 |
| Telegram | Announcements | Month 6 |
| Medium | Blog | Month 3 |

---

## 7. Hardware Requirements

### 7.1 Development Hardware

| Item | Specification | Purpose | Quantity |
|------|---------------|---------|----------|
| Development Machine | 32GB RAM, 8-core CPU, 512GB SSD | Coding | 1 per developer |
| GPU for Testing | RTX 3060+ (12GB VRAM) | Model inference testing | 1-2 |

### 7.2 Infrastructure Hardware (Cloud VPS)

| Phase | Servers | Specification | Provider | Est. Cost |
|-------|---------|---------------|----------|-----------|
| Phase 1 | 3 bootstrap (dev) | 4GB RAM, 2 vCPU | Hetzner | $15/mo |
| Phase 2 | 3 bootstrap + 1 IPFS | 8GB RAM, 4 vCPU | Hetzner | $60/mo |
| Phase 3 | 5 bootstrap + 3 IPFS + monitoring | 16GB RAM, 4 vCPU | Hetzner/OVH | $200/mo |
| Phase 4 | 10+ nodes globally | Various | Multi-provider | $500+/mo |

### 7.3 Bootstrap Node Locations

| Region | Location | Provider |
|--------|----------|----------|
| US East | Virginia | Hetzner/AWS |
| US West | Oregon | Hetzner/GCP |
| Europe | Frankfurt | Hetzner |
| Asia | Singapore | DigitalOcean |
| South America | Sao Paulo | Vultr |

### 7.4 Hardware Wallets

| Item | Purpose | Quantity |
|------|---------|----------|
| Ledger Nano X | Multi-sig signers | 5 |
| Backup Seeds | Seed phrase storage | 5 (metal backup) |

---

## 8. Software & Tools

### 8.1 Development Tools

| Category | Tool | Purpose |
|----------|------|---------|
| **Language** | Rust 1.75+ | Node software |
| **Language** | Solidity 0.8.20+ | Smart contracts |
| **Language** | Python 3.10+ | SDK, scripts |
| **Language** | TypeScript 5+ | SDK, web |
| **Framework** | Hardhat | Contract dev |
| **Framework** | Foundry | Contract testing |
| **IDE** | VS Code | Development |
| **Version Control** | Git | Code management |

### 8.2 Infrastructure Tools

| Tool | Purpose |
|------|---------|
| Docker | Containerization |
| Kubernetes (optional) | Orchestration |
| Terraform | Infrastructure as code |
| Ansible | Configuration management |
| Prometheus | Metrics |
| Grafana | Visualization |
| Loki | Logging |

### 8.3 Security Tools

| Tool | Purpose |
|------|---------|
| Slither | Solidity static analysis |
| Mythril | Symbolic execution |
| Echidna | Fuzzing |
| cargo-audit | Rust dependency audit |
| Semgrep | Code scanning |

### 8.4 Dependencies (Node Software)

```toml
# Cargo.toml
[dependencies]
tokio = { version = "1.0", features = ["full"] }
libp2p = { version = "0.53", features = ["tcp", "quic", "noise", "yamux", "kad", "gossipsub"] }
ethers = "2.0"
arti-client = "0.11"  # Tor
clap = { version = "4.0", features = ["derive"] }
serde = { version = "1.0", features = ["derive"] }
toml = "0.8"
tracing = "0.1"
argon2 = "0.5"
aes-gcm = "0.10"
```

### 8.5 Dependencies (Smart Contracts)

```json
// package.json
{
  "dependencies": {
    "@openzeppelin/contracts": "^5.0.0",
    "@openzeppelin/contracts-upgradeable": "^5.0.0"
  },
  "devDependencies": {
    "hardhat": "^2.19.0",
    "@nomicfoundation/hardhat-toolbox": "^4.0.0",
    "hardhat-deploy": "^0.11.0"
  }
}
```

---

## 9. Budget Estimate

### 9.1 Phase 1 (Months 0-3)

| Category | Item | Cost |
|----------|------|------|
| Infrastructure | VPS (3 servers) | $45 |
| Domains | ENS names (4) | $100 |
| Domains | oarn.network | $15 |
| Tools | GitHub (free tier) | $0 |
| Tools | Alchemy (free tier) | $0 |
| **Total Phase 1** | | **~$160** |

### 9.2 Phase 2 (Months 3-6)

| Category | Item | Cost |
|----------|------|------|
| Infrastructure | VPS (5 servers) | $180 |
| Tools | Pinata (paid) | $60 |
| Marketing | Twitter ads | $500 |
| Bounties | Alpha testing | $1,000 |
| **Total Phase 2** | | **~$1,740** |

### 9.3 Phase 3 (Months 6-9)

| Category | Item | Cost |
|----------|------|------|
| Infrastructure | VPS (10 servers) | $600 |
| Security Audit | Smart contracts | $30,000-100,000 |
| Security Audit | Node software | $20,000-50,000 |
| Bug Bounty | Initial pool | $50,000 |
| Legal | Token opinion | $25,000-50,000 |
| **Total Phase 3** | | **$125,000-250,000** |

### 9.4 Phase 4 (Months 9-12)

| Category | Item | Cost |
|----------|------|------|
| Infrastructure | VPS (20 servers) | $1,500 |
| Gas | Mainnet deployment | $500-2,000 |
| Marketing | Launch campaign | $10,000 |
| Legal | Ongoing | $5,000 |
| **Total Phase 4** | | **~$17,000-20,000** |

### 9.5 Total Estimated Budget

| Scenario | Total Cost |
|----------|------------|
| **Minimum (self-audit, minimal legal)** | ~$50,000 |
| **Standard (one audit, basic legal)** | ~$150,000 |
| **Comprehensive (multiple audits, full legal)** | ~$300,000+ |

### 9.6 Funding Sources

- [ ] Personal funds
- [ ] Gitcoin grants
- [ ] Ethereum Foundation grant
- [ ] Arbitrum Foundation grant
- [ ] Community donations
- [ ] No VC funding (per whitepaper principles)

---

## 10. Team Requirements

### 10.1 Core Team (5-10 people)

| Role | Skills Required | Priority |
|------|-----------------|----------|
| **Lead Developer** | Rust, systems programming, P2P | Critical |
| **Smart Contract Developer** | Solidity, security, testing | Critical |
| **DevOps Engineer** | Docker, K8s, CI/CD, monitoring | High |
| **Frontend Developer** | React/Vue, Web3 | High |
| **Community Manager** | Discord, Twitter, communication | High |
| **Technical Writer** | Documentation, tutorials | Medium |
| **Security Engineer** | Auditing, pentesting | Medium |
| **Designer** | UI/UX, branding | Medium |

### 10.2 Advisors (Part-time)

| Role | Purpose |
|------|---------|
| **Crypto Legal Advisor** | Token classification, compliance |
| **Security Advisor** | Code review, threat modeling |
| **Research Advisor** | Use case validation, partnerships |

### 10.3 Community Contributors

- Alpha/beta testers
- Documentation contributors
- Translation
- Bug reporters
- Node operators

---

## Quick Start Checklist

### Day 1

- [ ] Create GitHub organization
- [ ] Set up development environment
- [ ] Create Discord server
- [ ] Create Twitter account
- [ ] Register ENS names

### Week 1

- [ ] Initialize all repositories
- [ ] Set up CI/CD
- [ ] Create multi-sig wallet
- [ ] Start smart contract development
- [ ] Write initial documentation

### Month 1

- [ ] Complete OARNRegistry.sol
- [ ] Complete token contracts
- [ ] Initialize node software (Rust)
- [ ] Deploy to testnet
- [ ] Recruit first contributors

### Month 3

- [ ] All contracts on testnet
- [ ] Node software PoC working
- [ ] 3-node network functional
- [ ] Alpha testing begins

---

## Critical Path

```
┌─────────────────────────────────────────────────────────────────────┐
│                         Critical Path                                │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Week 1-4: Smart Contracts ─────────────────────────┐               │
│                                                      │               │
│  Week 5-10: Node Software ───────────────────────────┤               │
│                                                      │               │
│  Week 11-12: Integration ────────────────────────────┼── Phase 1    │
│                                                      │   Complete    │
│  Month 4-5: Alpha Testing ───────────────────────────┤               │
│                                                      │               │
│  Month 6: SDK Development ───────────────────────────┼── Phase 2    │
│                                                      │   Complete    │
│  Month 7: Security Audit ────────────────────────────┤               │
│           (BLOCKER - cannot proceed without)         │               │
│                                                      │               │
│  Month 8-9: Public Beta ─────────────────────────────┼── Phase 3    │
│                                                      │   Complete    │
│  Month 10: Mainnet Prep ─────────────────────────────┤               │
│                                                      │               │
│  Month 11: MAINNET LAUNCH ───────────────────────────┼── Phase 4    │
│                                                      │   Complete    │
│  Month 12: DAO Transition ───────────────────────────┘               │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

---

## References

- [Whitepaper](./decentralized-ai-whitepaper.md)
- [Smart Contracts Tasks](./02-smart-contracts-tasks.md)
- [Node Software Tasks](./03-node-software-tasks.md)
- [Infrastructure Tasks](./04-infrastructure-tasks.md)
- [Security Tasks](./08-security-tasks.md)
- [Security Hardening](./11-security-hardening.md)

---

**Remember: No hardcoded values, no API keys, no central dependencies. Build it right from day one.**
