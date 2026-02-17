# OARN Architecture Overview

This document describes the technical architecture of the Open AI Research Network (OARN).

## System Overview

```
                                    +-------------------+
                                    |    ENS Registry   |
                                    | oarn-registry.eth |
                                    +--------+----------+
                                             |
                                             | resolves to
                                             v
+-------------------+              +-------------------+              +-------------------+
|   Client/User     | <----------> |   OARNRegistry    | <----------> |   Compute Nodes   |
|   Application     |   discover   |   (Arbitrum)      |   register   |   (Rust p2p)      |
+-------------------+              +-------------------+              +-------------------+
        |                                   |                                  |
        |                                   |                                  |
        v                                   v                                  v
+-------------------+              +-------------------+              +-------------------+
|   Task Submission |              |   TaskRegistry    |              |   IPFS Storage    |
|   (via Node)      | -----------> |   GOV/COMP Tokens | <----------- |   (Models/Data)   |
+-------------------+              +-------------------+              +-------------------+
```

## Core Components

### 1. Smart Contracts (Arbitrum)

Located in `oarn-contracts/`:

```
contracts/
├── OARNRegistry.sol      # Central discovery registry
├── TaskRegistry.sol      # Task lifecycle management
├── GOVToken.sol          # Governance token (ERC-20 + Votes)
└── COMPToken.sol         # Compute reward token (ERC-20)
```

#### OARNRegistry

The **single entry point** for all OARN infrastructure discovery:

- **Immutable core addresses**: TaskRegistry, TokenReward, ValidatorRegistry, Governance, GOVToken
- **Dynamic infrastructure**: RPC providers, bootstrap nodes
- **Staking mechanism**: Providers stake GOV tokens, can be slashed for misbehavior
- **No hardcoded values**: Clients discover everything through this contract

#### TaskRegistry

Manages the complete task lifecycle:

1. **Submission**: Users submit tasks with model CID, input CID, requirements
2. **Claiming**: Nodes claim tasks they can execute
3. **Execution**: Node runs inference and submits result
4. **Verification**: Optional validator verification
5. **Reward**: COMP tokens distributed to executor

#### Token System

- **GOV Token**: Fixed supply (100M), used for governance voting and staking
- **COMP Token**: Inflationary with decreasing emission (100M year 1, -20%/year)

### 2. Node Software (Rust)

Located in `oarn-node/`:

```
src/
├── main.rs         # Entry point, CLI handling
├── cli.rs          # Command line interface (clap)
├── config.rs       # TOML configuration management
├── network.rs      # libp2p networking
├── discovery.rs    # Infrastructure discovery
├── blockchain.rs   # Ethereum interaction
├── storage.rs      # IPFS integration
└── compute.rs      # AI inference engine
```

#### Discovery Module

Ensures **zero hardcoded values**:

```
                    +----------------+
                    |  Config File   |
                    +-------+--------+
                            |
                            v
+----------+    +----------+----------+    +-----------+
|   ENS    |--->|     Discovery      |<---|    DHT    |
| Resolver |    |      Service       |    | (libp2p)  |
+----------+    +----------+----------+    +-----------+
                           |
                           v
               +-----------+-----------+
               |                       |
               v                       v
       +---------------+      +----------------+
       | RPC Providers |      | Bootstrap Nodes|
       +---------------+      +----------------+
```

Discovery methods (in priority order):
1. **ENS**: Resolve `oarn-registry.eth` for contract address
2. **DHT**: Query Kademlia DHT for `/oarn/bootstrap` keys
3. **On-chain**: Query OARNRegistry for provider lists

#### Network Module

P2P networking using libp2p:

- **Transport**: TCP with Noise protocol encryption
- **Multiplexing**: Yamux
- **Discovery**: Kademlia DHT + mDNS
- **Pub/Sub**: Gossipsub for task announcements
- **Optional**: Tor integration via arti

#### Compute Module

AI inference execution:

```
+----------------+
|  Task Queue    |
+-------+--------+
        |
        v
+----------------+    +------------------+
| Can Handle?    |--->| Check Resources  |
| (Framework,    |    | (VRAM, RAM)      |
|  Resources)    |    +------------------+
+-------+--------+
        |
        v
+----------------+    +------------------+
| Execute        |--->| ONNX Runtime /   |
| (Sandboxed)    |    | PyTorch / TF     |
+----------------+    +------------------+
        |
        v
+----------------+
| Submit Result  |
+----------------+
```

Supported frameworks:
- ONNX Runtime
- PyTorch (via tch-rs)
- TensorFlow (planned)

### 3. Storage (IPFS)

Content-addressed storage for:

- **Model weights**: Large files stored with CID
- **Input data**: Task inputs
- **Results**: Inference outputs
- **Proofs**: Execution proofs (planned)

```
+---------------+
|  Local Cache  |
+-------+-------+
        |
        | cache miss
        v
+---------------+    +------------------+
|  IPFS Client  |--->|  IPFS Network    |
+---------------+    +------------------+
```

## Data Flow

### Task Execution Flow

```
1. User submits task
   |
   v
2. Task stored on-chain (TaskRegistry)
   |
   v
3. Node discovers task via Gossipsub
   |
   v
4. Node claims task on-chain
   |
   v
5. Node fetches model/input from IPFS
   |
   v
6. Node executes inference
   |
   v
7. Node uploads result to IPFS
   |
   v
8. Node submits result CID on-chain
   |
   v
9. (Optional) Validator verifies result
   |
   v
10. COMP tokens distributed to node
```

### Discovery Flow

```
1. Node starts
   |
   v
2. Load config (no hardcoded values)
   |
   v
3. Resolve oarn-registry.eth via ENS
   |
   v
4. Get OARNRegistry contract address
   |
   v
5. Query active RPC providers
   |
   v
6. Query active bootstrap nodes
   |
   v
7. Connect to P2P network
   |
   v
8. Join DHT and Gossipsub topics
```

## Security Architecture

### Network Security

- **E2E Encryption**: Noise protocol for all P2P connections
- **Privacy Options**: Tor support, traffic padding, peer rotation
- **No Central Points**: Fully decentralized discovery

### Smart Contract Security

- **Immutable Registry**: Core addresses set at deployment
- **Stake-based Security**: Providers stake tokens, slashed for misbehavior
- **OpenZeppelin**: Battle-tested contracts for tokens

### Node Security

- **Sandboxed Execution**: AI models run in isolated environment
- **Resource Limits**: Configurable VRAM/RAM limits
- **No Root Required**: Node runs as unprivileged user

## Network Topology

```
                    +-------------------+
                    |   Bootstrap Nodes |
                    |  (Registered on   |
                    |   OARNRegistry)   |
                    +---------+---------+
                              |
              +---------------+---------------+
              |               |               |
              v               v               v
        +----------+    +----------+    +----------+
        |  Node A  |<-->|  Node B  |<-->|  Node C  |
        +----------+    +----------+    +----------+
              |               |               |
              +-------+-------+-------+-------+
                      |               |
                      v               v
                +----------+    +----------+
                |  Node D  |<-->|  Node E  |
                +----------+    +----------+
```

- **Mesh topology**: Nodes connect to multiple peers
- **Kademlia DHT**: O(log n) routing
- **Gossipsub**: Efficient message propagation

## Deployment Architecture

### Testnet (Current)

```
Network: Arbitrum Sepolia (Chain ID: 421614)
ENS: Not yet registered
Discovery: Manual RPC URL for bootstrap
```

### Mainnet (Target)

```
Network: Arbitrum One (Chain ID: 42161)
ENS: oarn-registry.eth
Discovery: Full ENS + DHT + On-chain
```

## Configuration Modes

| Mode | Description | Use Case |
|------|-------------|----------|
| Local | Offline, local inference only | Development, testing |
| Standard | Full P2P network | Default production mode |
| ValidatorRouted | High-speed via validators | Low-latency requirements |
| Auto | Automatic selection | General use |

## Scalability Considerations

### Horizontal Scaling

- **More nodes**: Linear scaling of compute capacity
- **Sharded tasks**: Different nodes handle different task types
- **Geographic distribution**: Nodes worldwide for low latency

### Vertical Scaling

- **GPU clusters**: Nodes can have multiple GPUs
- **Concurrent tasks**: Configurable task parallelism
- **Batch processing**: Multiple inputs per model load

## Future Architecture

### Planned Components

1. **Validator Registry**: Stake-weighted result verification
2. **Reputation System**: Track node reliability over time
3. **Model Registry**: On-chain model metadata and versions
4. **Privacy Enhancements**: Zero-knowledge proofs for sensitive data
