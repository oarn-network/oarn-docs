# OARN Network - Frequently Asked Questions

## General

### What is OARN Network?
OARN (Open AI Research Network) is a decentralized infrastructure for AI inference. It allows anyone to submit AI tasks and have them executed by multiple nodes with consensus verification.

### How is OARN different from centralized AI services?
- **Decentralized**: No single company controls the network
- **Verifiable**: Multi-node consensus ensures accurate results
- **Open**: Anyone can run a node or submit tasks
- **Trustless**: No need to trust any single party

### What blockchain does OARN use?
OARN runs on Arbitrum, an Ethereum Layer 2 network. This provides low fees and fast transactions while inheriting Ethereum's security.

---

## For Node Operators

### How do I run a node?
```bash
git clone https://github.com/oarn-network/oarn-node
cd oarn-node
cargo build --release
./target/release/oarn-node start
```

Or with Docker:
```bash
docker-compose up -d
```

### What are the hardware requirements?
- **Minimum**: 8GB RAM, modern CPU
- **Recommended**: 16GB+ RAM, GPU for faster inference
- **Storage**: 50GB+ for model caching

### How much can I earn?
Earnings depend on:
- Number of tasks available
- Your node's uptime and reliability
- Task reward amounts
- Whether you match consensus

### What tokens do I earn?
- **ETH**: Direct payment for task completion
- **COMP**: Bonus rewards for network participation

### What is consensus and how does it affect my earnings?
When you submit a result that matches the majority of other nodes, you receive full rewards. If your result doesn't match consensus, you may receive reduced or no rewards.

---

## For Task Submitters

### How do I submit an AI task?
```bash
oarn-node tasks submit \
  --model ./my_model.onnx \
  --input ./input_data.json \
  --reward 0.001 \
  --nodes 3 \
  --v2 --consensus majority
```

Or using the SDK:
```typescript
import { OARNClient } from '@oarnnetwork/sdk';

const client = new OARNClient({ privateKey: process.env.PRIVATE_KEY });
const { taskId } = await client.submitTask(model, input, reward, nodes);
```

### What model formats are supported?
OARN uses ONNX Runtime, which supports:
- ONNX models (native)
- PyTorch models (export to ONNX)
- TensorFlow models (export to ONNX)

### How much does it cost?
You set the reward per node. Total cost = reward x number of nodes required.

### What consensus types are available?
- **Majority (>50%)**: At least half the nodes must agree
- **SuperMajority (>66%)**: Two-thirds must agree
- **Unanimous (100%)**: All nodes must agree

---

## Tokens & Economics

### What is the COMP token?
COMP is the computation reward token. Node operators earn COMP for completing tasks, incentivizing long-term participation.

### What is the GOV token?
GOV is the governance token. Holders can vote on protocol proposals, upgrades, and parameter changes.

### How do I get tokens?
- **COMP**: Earn by running a node and completing tasks
- **GOV**: Distributed to early contributors and node operators

### Is there a token sale?
No public token sale is planned. Tokens are earned through network participation.

---

## Governance

### How does governance work?
1. Any GOV holder can create a proposal
2. Voting period begins (configurable, typically 3-7 days)
3. Voters cast For/Against/Abstain votes
4. If quorum and majority are reached, proposal passes
5. Timelock period before execution

### What can be governed?
- Protocol parameters (fees, rewards, thresholds)
- Contract upgrades
- Treasury spending
- Network policies

### How do I vote?
```bash
oarn-node governance vote --proposal-id 1 --choice for
```

---

## Technical

### What is multi-node consensus?
Multiple nodes execute the same task independently. Their results are compared on-chain. If enough nodes produce the same result, consensus is reached and the result is considered valid.

### Why use IPFS?
IPFS provides decentralized storage for models and data. No single server holds your files - they're distributed across the network.

### Is OARN open source?
Yes! All code is open source:
- Node software: https://github.com/oarn-network/oarn-node
- Smart contracts: https://github.com/oarn-network/oarn-contracts
- SDK: https://github.com/oarn-network/oarn-sdk

### How is privacy handled?
- Model inputs can be encrypted
- No account registration required
- All infrastructure discovered dynamically (no hardcoded servers)

---

## Security

### Has the code been audited?
Internal security review completed. External audit planned before mainnet.

### What happens if a node submits wrong results?
Nodes that consistently fail to match consensus may be flagged. In the future, staking and slashing mechanisms will penalize malicious behavior.

### Are smart contracts upgradeable?
Core contracts (TaskRegistry, Token) are immutable. The OARNRegistry allows controlled upgrades through governance.

---

## Troubleshooting

### My node isn't finding tasks
- Check your internet connection
- Ensure IPFS is running
- Verify your wallet has ETH for gas
- Check logs: `oarn-node --verbose start`

### Task submission failed
- Check wallet balance (need ETH for gas + rewards)
- Verify model is valid ONNX format
- Ensure deadline is in the future

### Where can I get help?
- Discord: https://discord.gg/RsrQwNvt
- GitHub Issues: https://github.com/oarn-network/oarn-node/issues
- Twitter: https://twitter.com/OARNNetwork
