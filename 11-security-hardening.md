# Security Hardening for Permissionless Architecture

## Overview

For OARN to be truly permissionless and censorship-resistant, we must eliminate ALL centralized dependencies, hardcoded values, and traffic analysis vectors. This document outlines the security hardening required.

---

## 1. Zero Hardcoded Values Policy

### NEVER Hardcode These:

| Item | Problem | Solution |
|------|---------|----------|
| RPC URLs | Single point of failure, can be blocked | Decentralized RPC discovery |
| Bootstrap node IPs | Can be blocked/seized | DHT-based discovery, onion addresses |
| Contract addresses | Could be used to block | ENS resolution, on-chain registry |
| API endpoints | Centralization point | P2P only, no central API |
| DNS names | Can be seized/blocked | ENS, .onion, .i2p addresses |
| API keys | Security risk, tracking | No API keys ever |
| Wallet addresses | Privacy leak | Generate fresh per session |
| Model URLs | Centralization | IPFS CIDs only |

### Implementation Requirements

```rust
// BAD - Never do this
const RPC_URL: &str = "https://arb1.arbitrum.io/rpc";
const BOOTSTRAP_NODE: &str = "12D3KooW...";

// GOOD - Dynamic discovery
fn get_rpc_endpoints() -> Vec<String> {
    // 1. Query ENS for oarn-rpc.eth
    // 2. Query DHT for RPC providers
    // 3. Fallback to user-configured list
    // 4. Rotate randomly
}

fn get_bootstrap_nodes() -> Vec<PeerId> {
    // 1. Query DHT with well-known key
    // 2. Use onion addresses
    // 3. mDNS for local discovery
    // 4. User can add custom nodes
}
```

---

## 2. Decentralized Discovery Mechanisms

### RPC Endpoint Discovery

```
┌─────────────────────────────────────────────────────────┐
│                   RPC Discovery Flow                     │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  1. ENS Lookup ──────> oarn-rpc.eth                     │
│         │               (returns list of RPC providers)  │
│         v                                                │
│  2. DHT Query ───────> /oarn/rpc/providers              │
│         │               (decentralized provider list)    │
│         v                                                │
│  3. On-Chain Registry ──> RPCRegistry.sol               │
│         │               (staked RPC providers)           │
│         v                                                │
│  4. User Config ─────> ~/.oarn/rpc-providers.json       │
│         │               (user-provided fallbacks)        │
│         v                                                │
│  5. Random Selection + Health Check                      │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

### Bootstrap Node Discovery

- **Primary:** DHT with well-known keys (no hardcoded nodes)
- **Secondary:** ENS TXT records (oarn-bootstrap.eth)
- **Tertiary:** Tor hidden services (.onion addresses)
- **Quaternary:** I2P eepsites
- **Local:** mDNS for LAN discovery

### Contract Address Discovery

```solidity
// On-chain registry that can never be changed
contract OARNRegistry {
    // Immutable after deployment
    address public immutable taskRegistry;
    address public immutable tokenReward;
    address public immutable governance;

    // ENS: oarn-registry.eth points here
    // Multiple chains: same address via CREATE2
}
```

---

## 3. Anonymous Communication Layer

### Tor Integration

```
┌─────────────────────────────────────────────────────────┐
│                  Network Privacy Modes                   │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  Mode 1: Standard (Default)                              │
│  ├── Direct P2P connections                              │
│  ├── TLS encryption                                      │
│  └── Peer ID visible to network                          │
│                                                          │
│  Mode 2: Tor-Enhanced                                    │
│  ├── All traffic over Tor                                │
│  ├── .onion addresses for nodes                          │
│  ├── No IP address exposure                              │
│  └── Higher latency, maximum privacy                     │
│                                                          │
│  Mode 3: I2P-Enhanced                                    │
│  ├── All traffic over I2P                                │
│  ├── Garlic routing                                      │
│  └── Alternative to Tor                                  │
│                                                          │
│  Mode 4: Hybrid                                          │
│  ├── Sensitive operations over Tor                       │
│  ├── Bulk data over clearnet                             │
│  └── Configurable per-operation                          │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

### Implementation Tasks

- [ ] Integrate `arti` (Rust Tor implementation)
- [ ] Support .onion addresses for all nodes
- [ ] Tor-only mode for maximum privacy
- [ ] I2P support as Tor alternative
- [ ] Automatic Tor bootstrap (no external dependency)
- [ ] Hidden service for local node
- [ ] Circuit rotation for long-running connections

---

## 4. Traffic Analysis Resistance

### Metadata Protection

| Threat | Mitigation |
|--------|------------|
| Timing analysis | Random delays, batching |
| Packet size analysis | Padding to fixed sizes |
| Connection patterns | Decoy connections |
| Request frequency | Request batching |
| Peer correlation | Peer rotation |

### Implementation

```rust
struct PrivacyConfig {
    // Randomize timing
    min_delay_ms: u64,      // e.g., 100
    max_delay_ms: u64,      // e.g., 5000

    // Pad messages
    fixed_message_size: usize,  // e.g., 4096 bytes

    // Decoy traffic
    enable_decoy_traffic: bool,
    decoy_rate_per_minute: u32,

    // Connection management
    max_peer_connection_time: Duration,  // Rotate peers
    min_peers: usize,
    max_peers: usize,
}
```

### Padding Protocol

```
┌────────────────────────────────────────┐
│         Padded Message Format          │
├────────────────────────────────────────┤
│ [4 bytes] Actual message length        │
│ [N bytes] Encrypted message            │
│ [M bytes] Random padding               │
│ [32 bytes] HMAC                        │
├────────────────────────────────────────┤
│ Total: Always 4096 bytes (configurable)│
└────────────────────────────────────────┘
```

---

## 5. Secure Configuration

### No Secrets in Config Files

```toml
# ~/.oarn/config.toml

[node]
name = "my-node"
# Wallet path only, never the key
wallet_path = "~/.oarn/wallet.encrypted"

[network]
mode = "standard"  # standard, tor, i2p, hybrid
listen_port = 0    # Random port by default

# NO hardcoded bootstrap nodes
# Discovery is automatic via DHT/ENS
# User can ADD nodes, but defaults are discovered
[network.custom_bootstrap]
# Optional user-provided nodes (not required)

[privacy]
tor_enabled = false
i2p_enabled = false
padding_enabled = true
decoy_traffic = false
rotate_peers = true

[rpc]
# NO hardcoded RPC URLs
# Discovery via ENS/DHT
discovery_method = "auto"  # auto, ens, dht, manual
# Only if discovery_method = "manual"
# manual_endpoints = ["..."]
```

### Environment Variable Handling

```rust
// NEVER read secrets from env vars directly into memory
// Use secure memory allocation

use secrecy::{Secret, ExposeSecret};

fn load_wallet_password() -> Secret<String> {
    // 1. Prompt user interactively (preferred)
    // 2. Read from secure keyring (OS keychain)
    // 3. NEVER from env var or config file

    // Memory is zeroed when Secret is dropped
    Secret::new(password)
}
```

---

## 6. Decentralized RPC Strategy

### Multi-RPC Architecture

```
┌─────────────────────────────────────────────────────────┐
│                  RPC Redundancy Layer                    │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐               │
│  │ RPC #1   │  │ RPC #2   │  │ RPC #3   │  ...          │
│  │(Infura)  │  │(Alchemy) │  │(Ankr)    │               │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘               │
│       │             │             │                      │
│       v             v             v                      │
│  ┌─────────────────────────────────────────────────┐    │
│  │            RPC Load Balancer                     │    │
│  │  - Health checking                               │    │
│  │  - Automatic failover                            │    │
│  │  - Response verification (compare multiple)      │    │
│  │  - Rate limit management                         │    │
│  │  - Cost optimization                             │    │
│  └─────────────────────────────────────────────────┘    │
│                         │                                │
│                         v                                │
│                   Application                            │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

### RPC Provider Staking (On-Chain)

```solidity
contract RPCRegistry {
    struct RPCProvider {
        string endpoint;      // The RPC URL
        address owner;        // Provider address
        uint256 stake;        // Staked tokens
        uint256 uptime;       // Historical uptime
        uint256 latency;      // Average latency
        bool isActive;
    }

    mapping(uint256 => RPCProvider) public providers;
    uint256 public providerCount;

    // Stake to register as RPC provider
    function registerProvider(string calldata endpoint) external payable;

    // Slash for downtime or bad responses
    function reportProvider(uint256 providerId, bytes calldata proof) external;

    // Get random healthy providers
    function getProviders(uint256 count) external view returns (RPCProvider[] memory);
}
```

---

## 7. Key Management

### Hierarchical Deterministic Wallets

```
Master Seed (BIP-39)
       │
       ├── m/44'/60'/0'/0  ─── Main wallet (long-term storage)
       │
       ├── m/44'/60'/1'/0  ─── Task submission wallet (hot)
       │
       ├── m/44'/60'/2'/0  ─── Node rewards wallet
       │
       └── m/44'/60'/3'/x  ─── Ephemeral wallets (per-task privacy)
```

### Key Security Requirements

- [ ] Master seed encrypted with Argon2id
- [ ] Hardware wallet support mandatory for validators
- [ ] Automatic key rotation for ephemeral wallets
- [ ] Secure memory handling (mlock, zero on drop)
- [ ] No key material in logs or crash dumps
- [ ] Time-limited key decryption (re-prompt after timeout)

---

## 8. Zero-Knowledge Task Submission

### Private Task Protocol

For maximum privacy, tasks can be submitted without revealing content to validators:

```
┌─────────────────────────────────────────────────────────┐
│              Zero-Knowledge Task Flow                    │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  1. Researcher encrypts task with node's public key      │
│                                                          │
│  2. Task submitted to blockchain (encrypted)             │
│     - Only hash visible on-chain                         │
│     - Encrypted payload on IPFS                          │
│                                                          │
│  3. Node decrypts and executes privately                 │
│                                                          │
│  4. Result encrypted with researcher's public key        │
│                                                          │
│  5. Commitment scheme for validation                     │
│     - Hash of result submitted                           │
│     - Reveal phase for disputes only                     │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

### Implementation

- [ ] ECIES encryption for task content
- [ ] Commit-reveal scheme for results
- [ ] Optional full encryption mode
- [ ] Validator blind signing (attest without seeing)

---

## 9. Censorship Resistance Checklist

### Network Layer
- [ ] No DNS dependency (ENS, .onion, .i2p)
- [ ] No single bootstrap nodes (DHT discovery)
- [ ] Tor/I2P support
- [ ] Traffic looks like normal HTTPS
- [ ] Domain fronting capability
- [ ] Multiple transport protocols (QUIC, TCP, WebSocket)

### Blockchain Layer
- [ ] Multiple RPC providers (discovered, not hardcoded)
- [ ] Self-hosted node option
- [ ] Light client mode
- [ ] Transaction submission via multiple routes
- [ ] Flashbots/private mempool option

### Storage Layer
- [ ] IPFS with multiple gateways
- [ ] Filecoin backup
- [ ] Arweave for permanent storage
- [ ] No single pinning service dependency

### Application Layer
- [ ] No accounts (wallet = identity)
- [ ] No email/phone requirements
- [ ] No KYC for basic usage
- [ ] Ephemeral identities supported
- [ ] No rate limiting by identity

---

## 10. Build & Distribution Security

### Reproducible Builds

```bash
# Anyone can verify the binary matches source
$ oarn verify-build v1.0.0
Fetching source from IPFS: Qm...
Building in deterministic container...
SHA256 of built binary: abc123...
SHA256 of distributed binary: abc123...
✓ Build is reproducible
```

### Distribution Channels

| Channel | Verification |
|---------|--------------|
| GitHub Releases | GPG signed, reproducible |
| IPFS | Content-addressed (CID) |
| Torrent | Hash verified |
| Package managers | Repository signatures |

### No Auto-Update Without Consent

```toml
[updates]
# User must explicitly enable
auto_update = false
# Notify only
notify_new_version = true
# Verify signatures
require_signature = true
# Multiple signers required
min_signatures = 2
```

---

## 11. Threat Model Summary

### Adversary Capabilities Assumed

| Adversary | Capabilities | Mitigations |
|-----------|--------------|-------------|
| ISP | Traffic analysis, blocking | Tor, encryption, domain fronting |
| Government | Subpoena, seizure, blocking | No central servers, no logs, encryption |
| Competitor | DDoS, Sybil attacks | PoW, staking, rate limiting |
| Malicious node | Result manipulation | Redundancy, slashing, reputation |
| Nation state | Full network monitoring | Tor, traffic analysis resistance |

### Security Properties Guaranteed

1. **Confidentiality:** Task content encrypted end-to-end
2. **Integrity:** Results verified by multiple nodes
3. **Availability:** No single point of failure
4. **Anonymity:** Optional Tor mode, no identity required
5. **Censorship resistance:** Multiple discovery mechanisms
6. **Forward secrecy:** Session keys rotated

---

## 12. Updated Security Tasks

### Critical (Must Have for Launch)

- [ ] Remove ALL hardcoded URLs/addresses from codebase
- [ ] Implement decentralized RPC discovery
- [ ] Implement DHT-based bootstrap discovery
- [ ] Add ENS resolution for all network resources
- [ ] Secure memory handling for all secrets
- [ ] Message padding to fixed sizes
- [ ] No secrets in config files

### High Priority (Should Have)

- [ ] Tor integration (arti)
- [ ] Reproducible builds
- [ ] Hardware wallet support
- [ ] Traffic analysis resistance
- [ ] Private task submission mode

### Medium Priority (Nice to Have)

- [ ] I2P support
- [ ] Domain fronting
- [ ] Zero-knowledge task validation
- [ ] Decoy traffic generation
