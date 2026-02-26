# OARN Quick Start Guide

Get up and running with OARN in 10 minutes.

## For Users (Submit AI Tasks)

### Option 1: Web Interface (Coming Soon)

Visit https://oarn.network to submit tasks through the web UI.

### Option 2: Command Line

```bash
# Install the OARN CLI
npm install -g @oarn/cli

# Create a wallet (if you don't have one)
oarn wallet create

# Fund your wallet with COMP tokens
# (Get testnet tokens from https://oarn.network/faucet)

# Submit a task
oarn task submit \
  --model QmModelCID... \
  --input QmInputCID... \
  --reward 100

# Check task status
oarn task status TASK_ID

# Get result
oarn task result TASK_ID
```

## For Node Operators (Earn COMP)

### Step 1: Install

**macOS/Linux:**
```bash
curl -sSL https://oarn.network/install.sh | bash
```

**Windows:**
```powershell
iwr https://oarn.network/install.ps1 -UseBasicParsing | iex
```

**From source:**
```bash
git clone https://github.com/oarn-network/oarn-node.git
cd oarn-node
cargo build --release
```

### Step 2: Initialize

```bash
# Create default config and wallet
oarn-node init

# This creates:
# - ~/.oarn/config.toml
# - ~/.oarn/keystore.json (encrypted wallet)
```

### Step 3: Start Node

```bash
oarn-node start
```

That's it! Your node will:
1. Discover the network via ENS/DHT
2. Connect to peers
3. Start accepting compute tasks
4. Earn COMP tokens for completed work

### Step 4: Check Status

```bash
oarn-node status
```

Output:
```
OARN Node v0.1.0
Status: Running
Mode: Standard
Peers: 12 connected
Tasks: 3 completed, 1 active
Wallet: 0x1234...5678
Balance: 150.5 COMP
```

## Configuration

The default config works for most users. To customize:

```bash
# Open config file
nano ~/.oarn/config.toml
```

Key settings:

```toml
# Mode: local, standard, validatorrouted, auto
mode = "standard"

# Resource limits (optional)
[compute]
max_vram_mb = 8192   # Limit GPU memory usage
max_ram_mb = 16384   # Limit RAM usage
concurrent_tasks = 2  # Run 2 tasks simultaneously

# Privacy (optional)
[privacy]
tor_enabled = true   # Route all traffic through Tor
```

## Common Tasks

### View Earnings

```bash
oarn-node wallet balance
```

### Withdraw Tokens

```bash
oarn-node wallet send 0xRecipient 100 COMP
```

### View Connected Peers

```bash
oarn-node peers
```

### View Logs

```bash
oarn-node logs -f
```

### Stop Node

```bash
oarn-node stop
# or Ctrl+C in terminal
```

## For Developers

### Smart Contract Interaction

```javascript
import { ethers } from "ethers";

// Connect to OARN Registry
const registry = await getOARNRegistry();

// Submit a task
const taskId = await registry.taskRegistry.submitTask(
  modelCid,
  inputCid,
  requirements,
  { value: reward }
);

// Wait for result
const result = await waitForTaskResult(taskId);
```

### SDK Installation

```bash
npm install @oarn/sdk
```

```typescript
import { OARNClient } from "@oarn/sdk";

const client = new OARNClient({
  network: "testnet",
  wallet: "your-private-key"
});

// Submit inference task
const result = await client.inference({
  model: "QmModelCID...",
  input: { prompt: "Hello, world!" }
});

console.log(result);
```

## Networks

| Network | Chain ID | Status |
|---------|----------|--------|
| Arbitrum Sepolia (Testnet) | 421614 | Active |
| Arbitrum One (Mainnet) | 42161 | Coming Soon |

## Resources

- Website: https://oarn-network.github.io/oarn-website/
- Documentation: https://github.com/oarn-network/oarn-docs
- GitHub: https://github.com/oarn-network
- Discord: https://discord.gg/RsrQwNvt
- Twitter: https://twitter.com/OARNNetwork

## Troubleshooting

### "No peers found"

1. Check firewall (port 4001 must be open)
2. Wait a few minutes for peer discovery
3. Try restarting the node

### "IPFS connection failed"

1. Install and start IPFS: `ipfs daemon`
2. Or run without IPFS (limited functionality)

### "Insufficient funds"

1. Get testnet COMP from faucet
2. Ensure your wallet has ETH for gas

See full [Troubleshooting Guide](troubleshooting.md) for more help.

## Next Steps

1. **Stake tokens** to become an RPC provider or bootstrap node
2. **Join governance** to vote on network decisions
3. **Build integrations** using the SDK
4. **Contribute code** to the open-source project
