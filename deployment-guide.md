# OARN Deployment Guide

This guide covers deploying OARN smart contracts and running compute nodes.

## Prerequisites

- Node.js v18+ and npm
- Rust 1.75+ (for node software)
- Git
- An Ethereum wallet with funds
- IPFS daemon (optional but recommended)

## Smart Contract Deployment

### 1. Setup

```bash
# Clone the contracts repository
git clone https://github.com/oarn-network/oarn-contracts.git
cd oarn-contracts

# Install dependencies
npm install

# Copy environment template
cp .env.example .env
```

### 2. Configure Environment

Edit `.env` with your deployment wallet:

```env
# Deployment wallet private key (without 0x prefix)
PRIVATE_KEY=your_private_key_here

# RPC URLs
ARBITRUM_SEPOLIA_RPC=https://sepolia-rollup.arbitrum.io/rpc
ARBITRUM_MAINNET_RPC=https://arb1.arbitrum.io/rpc

# Optional: Etherscan API for verification
ARBISCAN_API_KEY=your_api_key
```

### 3. Compile Contracts

```bash
npm run compile
```

### 4. Run Tests

```bash
npm test

# With coverage
npm run coverage
```

### 5. Deploy to Testnet

```bash
# Check wallet balance
npx hardhat run scripts/check-and-deploy.ts --network arbitrum-sepolia
```

The deployment script will:
1. Check wallet balance (minimum 0.005 ETH required)
2. Deploy contracts in order: GOVToken, COMPToken, OARNRegistry, TaskRegistry
3. Configure permissions
4. Output deployed addresses

### 6. Verify Contracts

```bash
npx hardhat verify --network arbitrum-sepolia CONTRACT_ADDRESS "constructor" "args"
```

### 7. Deploy to Mainnet

```bash
# Ensure sufficient funds (~0.01 ETH for gas)
npx hardhat run scripts/deploy.ts --network arbitrum-mainnet
```

## Node Deployment

### 1. Build from Source

```bash
# Clone the node repository
git clone https://github.com/oarn-network/oarn-node.git
cd oarn-node

# Build release binary
cargo build --release

# Binary located at target/release/oarn-node
```

### 2. Configure Node

Create configuration file at `~/.oarn/config.toml`:

```toml
# Node operational mode
mode = "standard"

[network]
# Addresses to listen on
listen_addresses = [
    "/ip4/0.0.0.0/tcp/4001",
    "/ip6/::/tcp/4001"
]
max_peers = 50

[network.discovery]
method = "auto"
ens_registry = "oarn-registry.eth"

[blockchain]
chain_id = 421614  # Arbitrum Sepolia
rpc_discovery = "registry"
# For testing only:
# manual_rpc_url = "https://sepolia-rollup.arbitrum.io/rpc"

[storage]
ipfs_api = "http://127.0.0.1:5001"
cache_dir = "~/.oarn/cache"
max_cache_mb = 10240  # 10 GB

[compute]
frameworks = ["onnx", "pytorch"]
concurrent_tasks = 1
# Optionally limit resources:
# max_vram_mb = 8192
# max_ram_mb = 16384

[privacy]
tor_enabled = false
padding_enabled = true
rotate_peers = true
rotation_interval_mins = 30

[wallet]
keystore_path = "~/.oarn/keystore.json"
use_hd_wallet = true
derivation_path = "m/44'/60'/0'/0"
```

### 3. Create Wallet

```bash
./target/release/oarn-node wallet create
# Follow prompts to set password
# Backup the mnemonic phrase!
```

Or import existing wallet:

```bash
./target/release/oarn-node wallet import
```

### 4. Start IPFS Daemon

```bash
# Install IPFS
# macOS: brew install ipfs
# Linux: snap install ipfs

# Initialize and start
ipfs init
ipfs daemon
```

### 5. Run Node

```bash
# Start the node
./target/release/oarn-node start

# With debug logging
RUST_LOG=debug ./target/release/oarn-node start

# Check status
./target/release/oarn-node status
```

## Production Deployment

### Systemd Service

Create `/etc/systemd/system/oarn-node.service`:

```ini
[Unit]
Description=OARN Compute Node
After=network.target ipfs.service

[Service]
Type=simple
User=oarn
Group=oarn
WorkingDirectory=/home/oarn
ExecStart=/usr/local/bin/oarn-node start
Restart=always
RestartSec=10
Environment=RUST_LOG=info

[Install]
WantedBy=multi-user.target
```

Enable and start:

```bash
sudo systemctl daemon-reload
sudo systemctl enable oarn-node
sudo systemctl start oarn-node
```

### Docker Deployment

```dockerfile
FROM rust:1.75 as builder
WORKDIR /app
COPY . .
RUN cargo build --release

FROM debian:bookworm-slim
RUN apt-get update && apt-get install -y ca-certificates && rm -rf /var/lib/apt/lists/*
COPY --from=builder /app/target/release/oarn-node /usr/local/bin/
EXPOSE 4001
CMD ["oarn-node", "start"]
```

Build and run:

```bash
docker build -t oarn-node .
docker run -d \
  --name oarn-node \
  -p 4001:4001 \
  -v ~/.oarn:/root/.oarn \
  oarn-node
```

### Docker Compose

```yaml
version: '3.8'

services:
  ipfs:
    image: ipfs/kubo:latest
    ports:
      - "4001:4001"
      - "5001:5001"
    volumes:
      - ipfs_data:/data/ipfs

  oarn-node:
    build: .
    depends_on:
      - ipfs
    ports:
      - "4002:4001"
    volumes:
      - oarn_data:/root/.oarn
    environment:
      - RUST_LOG=info

volumes:
  ipfs_data:
  oarn_data:
```

## Network Registration

### Register as RPC Provider

Requires 5000 GOV tokens stake:

```javascript
const { ethers } = require("ethers");

const registry = new ethers.Contract(REGISTRY_ADDRESS, REGISTRY_ABI, signer);

await registry.registerRPCProvider(
  "https://your-rpc.example.com",
  "http://yourrpc.onion",  // Optional Tor endpoint
  { value: ethers.parseEther("5000") }
);
```

### Register as Bootstrap Node

Requires 1000 GOV tokens stake:

```javascript
await registry.registerBootstrapNode(
  "12D3KooW...",  // Your libp2p peer ID
  "/ip4/1.2.3.4/tcp/4001/p2p/12D3KooW...",
  "http://yournode.onion",  // Optional
  "",  // Optional i2p
  { value: ethers.parseEther("1000") }
);
```

### Send Heartbeats

Keep your registration active:

```javascript
// Call every hour
await registry.heartbeat();
```

## Monitoring

### Node Metrics

The node exposes metrics at `http://localhost:9090/metrics`:

- `oarn_tasks_total`: Total tasks processed
- `oarn_tasks_active`: Currently active tasks
- `oarn_peers_connected`: Number of connected peers
- `oarn_uptime_seconds`: Node uptime

### Prometheus Configuration

```yaml
scrape_configs:
  - job_name: 'oarn-node'
    static_configs:
      - targets: ['localhost:9090']
```

### Grafana Dashboard

Import the provided dashboard from `docs/grafana-dashboard.json`.

## Troubleshooting

### Common Issues

| Issue | Solution |
|-------|----------|
| "No RPC providers" | Check discovery config, try manual RPC URL |
| "IPFS connection failed" | Ensure IPFS daemon is running |
| "Insufficient stake" | Fund wallet with GOV tokens |
| "Network unreachable" | Check firewall, ensure port 4001 is open |

### Logs

```bash
# View logs
journalctl -u oarn-node -f

# Debug logging
RUST_LOG=debug oarn-node start
```

### Health Check

```bash
# Check node status
oarn-node status

# Check blockchain connection
oarn-node check-rpc

# Check IPFS connection
oarn-node check-ipfs
```

## Upgrading

### Smart Contracts

Contracts are immutable by design. Upgrades require:
1. Deploy new contracts
2. Migrate state via governance proposal
3. Update ENS to point to new registry

### Node Software

```bash
# Pull latest changes
git pull origin main

# Rebuild
cargo build --release

# Restart service
sudo systemctl restart oarn-node
```

## Security Checklist

- [ ] Never commit private keys to git
- [ ] Use hardware wallet for mainnet deployments
- [ ] Enable firewall, only expose necessary ports
- [ ] Run node as unprivileged user
- [ ] Keep system and dependencies updated
- [ ] Monitor for suspicious activity
- [ ] Backup wallet and config
