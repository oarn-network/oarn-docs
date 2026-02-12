# Node Software Development Tasks

## Overview
The OARN Node Client runs on participant machines. Built in Rust (performance) with Python bindings (accessibility).

---

## 1. Project Setup

- [ ] Initialize Rust workspace (Cargo)
- [ ] Set up Python bindings (PyO3)
- [ ] Configure cross-compilation (Windows, macOS, Linux)
- [ ] Set up CI/CD (GitHub Actions)
- [ ] Create Docker images for easy deployment
- [ ] Design CLI interface (clap)

---

## 2. Core Architecture

```
┌─────────────────────────────────────┐
│ OARN Node Client                     │
├─────────────────────────────────────┤
│ CLI / API Layer                      │
├─────────────────────────────────────┤
│ Task Manager                         │
├───────────────┬─────────────────────┤
│ Inference     │ Network Manager     │
│ Engine        │ (libp2p)            │
├───────────────┼─────────────────────┤
│ Model Store   │ Blockchain Client   │
│ (GGUF/ONNX)   │ (ethers-rs)         │
├───────────────┴─────────────────────┤
│ Storage Layer (IPFS)                 │
└─────────────────────────────────────┘
```

---

## 3. CLI Commands

### Node Management
- [ ] `oarn init` - Initialize node configuration
- [ ] `oarn start` - Start node daemon
- [ ] `oarn stop` - Stop node daemon
- [ ] `oarn status` - Show node status
- [ ] `oarn config` - View/edit configuration
- [ ] `oarn logs` - View logs

### Wallet & Tokens
- [ ] `oarn wallet create` - Create new wallet
- [ ] `oarn wallet import` - Import existing wallet
- [ ] `oarn wallet balance` - Show token balances
- [ ] `oarn wallet send` - Send tokens

### Task Management
- [ ] `oarn task list` - List available tasks
- [ ] `oarn task claim <id>` - Claim a task
- [ ] `oarn task status <id>` - Check task status
- [ ] `oarn task history` - View completed tasks

### Model Management
- [ ] `oarn model list` - List installed models
- [ ] `oarn model pull <name>` - Download model
- [ ] `oarn model remove <name>` - Delete model
- [ ] `oarn model info <name>` - Show model details

### Local Mode
- [ ] `oarn local` - Start local-only mode (no network)
- [ ] `oarn local inference <prompt>` - Run local inference

---

## 4. Inference Engine Integration

### Ollama Integration
- [ ] Detect local Ollama installation
- [ ] Start/stop Ollama subprocess
- [ ] Route inference requests to Ollama
- [ ] Handle model loading/unloading
- [ ] Parse and return results

### Direct GGUF Support (Optional)
- [ ] Integrate llama.cpp bindings
- [ ] Support GGUF model loading
- [ ] GPU acceleration (CUDA, Metal, ROCm)
- [ ] Quantization support (Q4, Q8)

### Model Format Support
- [ ] GGUF (primary)
- [ ] ONNX (cross-platform)
- [ ] Safetensors
- [ ] PyTorch (research models)

---

## 5. P2P Networking (libp2p)

### Core Networking
- [ ] Initialize libp2p node
- [ ] Configure transport (QUIC preferred, TCP fallback)
- [ ] Implement peer discovery (DHT, mDNS for local)
- [ ] Handle NAT traversal (hole punching, relay)
- [ ] Connection encryption (Noise protocol)

### Task Distribution Protocol
- [ ] Define task announcement message format
- [ ] Implement task subscription (by model requirements)
- [ ] Handle task claim negotiation
- [ ] Result propagation to validators
- [ ] Gossip protocol for network-wide announcements

### Validator-Routed Mode
- [ ] Direct connection to validator nodes
- [ ] Priority task queue
- [ ] Fast result submission path
- [ ] SLA monitoring

### Network Health
- [ ] Peer scoring (reliability metrics)
- [ ] Connection limits (max peers)
- [ ] Bandwidth management
- [ ] Latency monitoring

---

## 6. Blockchain Integration

### Web3 Client (ethers-rs)
- [ ] Connect to Arbitrum RPC (public + fallback)
- [ ] Wallet management (keystore encryption)
- [ ] Transaction signing
- [ ] Gas estimation and management

### Smart Contract Interaction
- [ ] TaskRegistry: Read available tasks
- [ ] TaskRegistry: Submit task claims
- [ ] TaskRegistry: Submit results
- [ ] TokenReward: Check balances
- [ ] TokenReward: Claim rewards
- [ ] ValidatorRegistry: Register as premium node

### Event Monitoring
- [ ] Subscribe to TaskCreated events
- [ ] Handle TaskClaimed notifications
- [ ] Monitor own task completions
- [ ] Track reward distributions

---

## 7. IPFS Integration

### Storage Operations
- [ ] Initialize IPFS client (local daemon or remote)
- [ ] Upload task input data
- [ ] Download task data from CID
- [ ] Upload results
- [ ] Pin important content
- [ ] Garbage collection

### Model Storage
- [ ] Store models on IPFS
- [ ] Cache models locally
- [ ] Verify model integrity (hash check)
- [ ] Progressive download for large models

---

## 8. Task Execution Pipeline

```
1. Discover Task ─────> 2. Validate Requirements
        │                         │
        │                         v
        │               3. Claim Task (blockchain)
        │                         │
        │                         v
        │               4. Download Input Data (IPFS)
        │                         │
        │                         v
        │               5. Load Model (if not cached)
        │                         │
        │                         v
        │               6. Execute Inference
        │                         │
        │                         v
        │               7. Upload Result (IPFS)
        │                         │
        │                         v
        │               8. Submit Result Hash (blockchain)
        │                         │
        │                         v
        └──────────────> 9. Await Reward Distribution
```

### Implementation Tasks
- [ ] Task discovery and filtering
- [ ] Hardware capability matching
- [ ] Task claim transaction
- [ ] Input data retrieval
- [ ] Model loading/caching
- [ ] Inference execution with timeout
- [ ] Result formatting
- [ ] Result upload and hash submission
- [ ] Retry logic for failures
- [ ] Concurrent task handling

---

## 9. Configuration System

### Config File (~/.oarn/config.toml)
```toml
[node]
name = "my-node"
# Encrypted wallet path only - never store keys in config
wallet_path = "~/.oarn/wallet.encrypted"

[network]
mode = "standard"  # local, standard, validator_routed, tor, i2p
listen_port = 0    # 0 = random port (recommended for privacy)

# NO HARDCODED BOOTSTRAP NODES
# Discovery is automatic via DHT/ENS
# Only add custom nodes if needed, defaults are discovered
[network.discovery]
method = "auto"  # auto, dht, ens, manual
# ens_name = "oarn-bootstrap.eth"  # Optional override
# manual_nodes = []  # Only if method = "manual"

[privacy]
tor_enabled = false           # Route all traffic through Tor
i2p_enabled = false           # Alternative: I2P
padding_enabled = true        # Pad messages to fixed size
decoy_traffic = false         # Generate fake traffic
rotate_peers = true           # Rotate peer connections
min_delay_ms = 0              # Random delay before sending
max_delay_ms = 0              # (set both > 0 to enable)

[compute]
max_concurrent_tasks = 4
accepted_models = ["llama-*", "mistral-*"]
min_reward = 10  # COMP tokens

[storage]
model_cache_path = "~/.oarn/models"
max_cache_size_gb = 100

[blockchain]
# NO HARDCODED RPC URLs
# Discovery is automatic via ENS/DHT/on-chain registry
discovery_method = "auto"  # auto, ens, dht, manual
chain_id = 42161
# manual_rpc_urls = []  # Only if discovery_method = "manual"
# ens_rpc_registry = "oarn-rpc.eth"  # Optional override
```

### Security: What is NEVER in config
- Private keys or seed phrases
- API keys or tokens
- Hardcoded RPC URLs (discovered dynamically)
- Hardcoded bootstrap nodes (discovered dynamically)
- Hardcoded contract addresses (on-chain registry)
- Any credentials or secrets

### Implementation Tasks
- [ ] Config file parsing (TOML)
- [ ] Config validation
- [ ] Environment variable overrides
- [ ] Secure wallet path handling
- [ ] Dynamic config reload

---

## 10. Monitoring & Metrics

### Metrics Collection
- [ ] Tasks completed (count, success rate)
- [ ] Tokens earned
- [ ] Network stats (peers, bandwidth)
- [ ] Inference performance (tokens/sec)
- [ ] GPU/CPU utilization

### Interfaces
- [ ] Prometheus endpoint (/metrics)
- [ ] JSON API endpoint
- [ ] CLI dashboard (TUI with ratatui)

---

## 11. Security

### Zero Hardcoded Values (CRITICAL)
- [ ] NO hardcoded RPC URLs - use decentralized discovery
- [ ] NO hardcoded bootstrap nodes - use DHT/ENS discovery
- [ ] NO hardcoded contract addresses - use on-chain registry
- [ ] NO hardcoded API keys - never use API keys
- [ ] NO hardcoded DNS names - use ENS/.onion/.i2p
- [ ] All discovery via DHT, ENS, or on-chain registry

### Key & Wallet Security
- [ ] Wallet encryption (AES-256-GCM with Argon2id KDF)
- [ ] Private key never in memory longer than needed
- [ ] Secure memory allocation (mlock, zero on drop)
- [ ] Hardware wallet support (Ledger/Trezor) for validators
- [ ] HD wallet with ephemeral addresses for privacy
- [ ] No key material in logs or crash dumps

### Network Security
- [ ] End-to-end encryption on ALL connections (Noise protocol)
- [ ] Tor integration via arti (Rust Tor)
- [ ] I2P support as Tor alternative
- [ ] .onion hidden service for node
- [ ] Traffic padding to fixed message sizes
- [ ] Random delays to prevent timing analysis
- [ ] Decoy traffic generation (optional)
- [ ] Peer rotation to prevent correlation
- [ ] No clearnet fallback in privacy mode

### Decentralized Discovery
- [ ] RPC endpoints via ENS (oarn-rpc.eth)
- [ ] RPC endpoints via DHT (/oarn/rpc/providers)
- [ ] Bootstrap nodes via DHT (no hardcoded list)
- [ ] Contract addresses via on-chain registry
- [ ] Multiple discovery methods with fallback
- [ ] Health checking and automatic failover

### Execution Security
- [ ] Sandboxed model execution (seccomp, namespaces)
- [ ] Resource limits (CPU, memory, time, disk)
- [ ] No network access during inference
- [ ] File system isolation
- [ ] Input validation (prevent injection)
- [ ] Output sanitization

### Build Security
- [ ] Reproducible builds (deterministic)
- [ ] Signed releases (multiple signers required)
- [ ] No auto-update without user consent
- [ ] IPFS distribution with content addressing

---

## 12. Testing

### Unit Tests
- [ ] All CLI commands
- [ ] Configuration parsing
- [ ] Task pipeline stages
- [ ] Blockchain interactions (mock)

### Integration Tests
- [ ] Full task lifecycle (local)
- [ ] P2P communication (multi-node)
- [ ] IPFS upload/download
- [ ] Blockchain transactions (testnet)

### Performance Tests
- [ ] Concurrent task handling
- [ ] Large model loading
- [ ] Network throughput

---

## 13. Distribution & Installation

### Binaries
- [ ] Pre-built binaries (Windows, macOS, Linux)
- [ ] Installer scripts
- [ ] Homebrew formula (macOS)
- [ ] APT/RPM packages (Linux)
- [ ] Chocolatey package (Windows)

### Docker
- [ ] Minimal Docker image
- [ ] Docker Compose for full stack
- [ ] Kubernetes Helm chart

### One-Line Install
```bash
curl -sSL https://oarn.network/install.sh | bash
```

---

## 14. Documentation

- [ ] Installation guide
- [ ] Quick start tutorial
- [ ] CLI reference
- [ ] Configuration reference
- [ ] Troubleshooting guide
- [ ] API documentation
- [ ] Contributing guide
