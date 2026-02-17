# OARN API Reference

This document provides a comprehensive reference for all OARN smart contract interfaces and node RPC methods.

## Smart Contract Interfaces

### OARNRegistry

The central registry for all OARN infrastructure discovery.

**Contract Address:** Discovered via ENS (`oarn-registry.eth`)

#### Read Functions

| Function | Parameters | Returns | Description |
|----------|------------|---------|-------------|
| `taskRegistry()` | - | `address` | Address of the TaskRegistry contract |
| `tokenReward()` | - | `address` | Address of the TokenReward contract |
| `validatorRegistry()` | - | `address` | Address of the ValidatorRegistry contract |
| `governance()` | - | `address` | Address of the Governance contract |
| `govToken()` | - | `address` | Address of the GOV token contract |
| `getCoreContracts()` | - | `tuple` | Returns all core contract addresses in one call |
| `getActiveRPCProviders()` | - | `RPCProvider[]` | Returns all active RPC providers |
| `getRandomRPCProviders(count)` | `uint256` | `RPCProvider[]` | Returns N random active providers |
| `getActiveBootstrapNodes()` | - | `BootstrapNode[]` | Returns all active bootstrap nodes |
| `getRandomBootstrapNodes(count)` | `uint256` | `BootstrapNode[]` | Returns N random active nodes |
| `isActiveProvider(addr)` | `address` | `(bool, bool)` | Check if address is RPC provider and/or bootstrap node |
| `rpcProviders(id)` | `uint256` | `RPCProvider` | Get RPC provider by ID |
| `bootstrapNodes(id)` | `uint256` | `BootstrapNode` | Get bootstrap node by ID |

#### Write Functions

| Function | Parameters | Description |
|----------|------------|-------------|
| `registerRPCProvider(endpoint, onionEndpoint)` | `string, string` | Register as RPC provider (requires stake) |
| `updateRPCProvider(endpoint, onionEndpoint)` | `string, string` | Update RPC provider endpoints |
| `registerBootstrapNode(peerId, multiaddr, onion, i2p)` | `string, string, string, string` | Register as bootstrap node (requires stake) |
| `updateBootstrapNode(peerId, multiaddr, onion, i2p)` | `string, string, string, string` | Update bootstrap node info |
| `heartbeat()` | - | Send heartbeat to prove liveness |
| `initiateUnstake()` | - | Begin unstake process (7 day cooldown) |
| `completeUnstake()` | - | Complete unstake after cooldown |

#### Admin Functions (Owner Only)

| Function | Parameters | Description |
|----------|------------|-------------|
| `slashRPCProvider(id, amount, reason)` | `uint256, uint256, string` | Slash misbehaving RPC provider |
| `slashBootstrapNode(id, amount, reason)` | `uint256, uint256, string` | Slash misbehaving bootstrap node |

#### Events

```solidity
event RPCProviderRegistered(uint256 indexed id, address indexed owner, string endpoint);
event RPCProviderUpdated(uint256 indexed id, string endpoint, string onionEndpoint);
event RPCProviderDeactivated(uint256 indexed id, address indexed owner);
event RPCProviderSlashed(uint256 indexed id, uint256 amount, string reason);
event BootstrapNodeRegistered(uint256 indexed id, address indexed owner, string peerId);
event BootstrapNodeUpdated(uint256 indexed id, string peerId, string multiaddr);
event BootstrapNodeDeactivated(uint256 indexed id, address indexed owner);
event BootstrapNodeSlashed(uint256 indexed id, uint256 amount, string reason);
event UnstakeInitiated(address indexed owner, uint256 amount, uint256 availableAt);
event UnstakeCompleted(address indexed owner, uint256 amount);
event Heartbeat(address indexed provider, uint256 timestamp);
```

#### Structs

```solidity
struct RPCProvider {
    string endpoint;          // HTTPS RPC URL
    string onionEndpoint;     // .onion URL for Tor access
    address owner;
    uint256 stake;
    uint256 registeredAt;
    uint256 lastHeartbeat;
    uint256 uptime;           // Percentage * 100 (e.g., 9950 = 99.50%)
    uint256 reportCount;
    bool isActive;
}

struct BootstrapNode {
    string peerId;            // libp2p peer ID
    string multiaddr;         // Primary multiaddress
    string onionAddress;      // .onion address for Tor
    string i2pAddress;        // .i2p address (optional)
    address owner;
    uint256 stake;
    uint256 registeredAt;
    uint256 lastHeartbeat;
    bool isActive;
}
```

---

### TaskRegistry

Manages compute task submission, claiming, and completion.

#### Read Functions

| Function | Parameters | Returns | Description |
|----------|------------|---------|-------------|
| `tasks(id)` | `uint256` | `Task` | Get task by ID |
| `getTask(id)` | `uint256` | `Task` | Get full task details |
| `getTaskCount()` | - | `uint256` | Total number of tasks |
| `getActiveTasks()` | - | `Task[]` | Get all active (unclaimed) tasks |
| `getTasksBySubmitter(addr)` | `address` | `uint256[]` | Get task IDs submitted by address |
| `getTasksByNode(addr)` | `address` | `uint256[]` | Get task IDs claimed by node |

#### Write Functions

| Function | Parameters | Description |
|----------|------------|-------------|
| `submitTask(modelCid, inputCid, requirements, reward)` | `string, string, string, uint256` | Submit a new compute task |
| `claimTask(taskId)` | `uint256` | Claim a task for execution |
| `submitResult(taskId, resultCid)` | `uint256, string` | Submit task result |
| `verifyResult(taskId, isValid)` | `uint256, bool` | Verify result (validator only) |
| `cancelTask(taskId)` | `uint256` | Cancel submitted task (submitter only) |

#### Events

```solidity
event TaskSubmitted(uint256 indexed taskId, address indexed submitter, string modelCid);
event TaskClaimed(uint256 indexed taskId, address indexed node);
event TaskCompleted(uint256 indexed taskId, address indexed node, string resultCid);
event TaskVerified(uint256 indexed taskId, bool isValid);
event TaskCancelled(uint256 indexed taskId);
event RewardDistributed(uint256 indexed taskId, address indexed node, uint256 amount);
```

---

### GOVToken

ERC-20 governance token with voting capabilities.

#### Read Functions

| Function | Parameters | Returns | Description |
|----------|------------|---------|-------------|
| `name()` | - | `string` | "OARN Governance Token" |
| `symbol()` | - | `string` | "GOV" |
| `decimals()` | - | `uint8` | 18 |
| `totalSupply()` | - | `uint256` | Total supply (100M after genesis) |
| `balanceOf(account)` | `address` | `uint256` | Token balance |
| `getVotes(account)` | `address` | `uint256` | Current voting power |
| `getPastVotes(account, blockNumber)` | `address, uint256` | `uint256` | Historical voting power |
| `getQuadraticVotingPower(account)` | `address` | `uint256` | Quadratic voting power |
| `delegates(account)` | `address` | `address` | Current delegate |
| `mintingComplete()` | - | `bool` | Whether genesis distribution done |

#### Write Functions

| Function | Parameters | Description |
|----------|------------|-------------|
| `transfer(to, amount)` | `address, uint256` | Standard ERC-20 transfer |
| `approve(spender, amount)` | `address, uint256` | Approve spending |
| `delegate(delegatee)` | `address` | Delegate voting power |
| `permit(...)` | EIP-2612 params | Gasless approval via signature |

#### Admin Functions

| Function | Parameters | Description |
|----------|------------|-------------|
| `executeGenesisDistribution(early, public)` | `address, address` | One-time genesis distribution |

---

### COMPToken

ERC-20 compute reward token with emission schedule.

#### Read Functions

| Function | Parameters | Returns | Description |
|----------|------------|---------|-------------|
| `name()` | - | `string` | "OARN Compute Token" |
| `symbol()` | - | `string` | "COMP" |
| `totalMinted()` | - | `uint256` | Total tokens minted |
| `launchTime()` | - | `uint256` | Contract deployment timestamp |
| `getYearlyEmissionCap()` | - | `uint256` | Current year's emission cap |
| `getCurrentYearStart()` | - | `uint256` | Start timestamp of current year |
| `burnRate()` | - | `uint256` | Transfer burn rate (basis points) |
| `burnEnabled()` | - | `bool` | Whether burn on transfer is enabled |
| `minters(address)` | `address` | `bool` | Check if address can mint |

#### Write Functions

| Function | Parameters | Description |
|----------|------------|-------------|
| `mint(to, amount)` | `address, uint256` | Mint tokens (authorized minters only) |
| `burn(amount)` | `uint256` | Burn own tokens |
| `burnFrom(account, amount)` | `address, uint256` | Burn with approval |

#### Admin Functions

| Function | Parameters | Description |
|----------|------------|-------------|
| `addMinter(minter)` | `address` | Authorize new minter |
| `removeMinter(minter)` | `address` | Remove minter authorization |
| `setBurnRate(rate)` | `uint256` | Set burn rate (max 1000 = 10%) |
| `enableBurn(enabled)` | `bool` | Enable/disable burn on transfer |

---

## Node RPC API

The OARN node exposes a JSON-RPC API for local management.

### Methods

| Method | Parameters | Returns | Description |
|--------|------------|---------|-------------|
| `oarn_version` | - | `string` | Node version |
| `oarn_status` | - | `NodeStatus` | Current node status |
| `oarn_peers` | - | `Peer[]` | Connected peers list |
| `oarn_tasks` | - | `Task[]` | Current tasks |
| `oarn_submitTask` | `TaskParams` | `string` | Submit task, returns task ID |
| `oarn_getResult` | `taskId` | `Result` | Get task result |

### Example Requests

```bash
# Get node status
curl -X POST http://localhost:8545 \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"oarn_status","params":[],"id":1}'

# Submit a task
curl -X POST http://localhost:8545 \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc":"2.0",
    "method":"oarn_submitTask",
    "params":[{
      "modelCid": "QmModel...",
      "inputCid": "QmInput...",
      "requirements": {"framework": "onnx", "min_ram": "4GB"}
    }],
    "id":1
  }'
```

---

## Error Codes

### Smart Contract Errors

| Code | Name | Description |
|------|------|-------------|
| `INSUFFICIENT_STAKE` | - | Stake amount below minimum |
| `NOT_REGISTERED` | - | Caller not registered |
| `ALREADY_REGISTERED` | - | Caller already registered |
| `COOLDOWN_NOT_COMPLETE` | - | Unstake cooldown not finished |
| `TASK_NOT_FOUND` | - | Task ID does not exist |
| `TASK_ALREADY_CLAIMED` | - | Task already claimed by another node |
| `NOT_AUTHORIZED` | - | Caller not authorized for action |

### Node RPC Errors

| Code | Message | Description |
|------|---------|-------------|
| -32600 | Invalid Request | Malformed request |
| -32601 | Method not found | Unknown method |
| -32602 | Invalid params | Invalid parameters |
| -32603 | Internal error | Node internal error |
| -32000 | Not connected | No network connection |
| -32001 | No providers | No RPC/bootstrap providers |
