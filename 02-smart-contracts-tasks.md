# Smart Contracts Development Tasks

## Overview
All smart contracts deployed on Arbitrum (Ethereum L2). Development uses Hardhat + Solidity.

---

## 1. Development Environment Setup

- [ ] Initialize Hardhat project
- [ ] Configure Arbitrum testnet (Goerli/Sepolia)
- [ ] Set up deployment scripts
- [ ] Configure environment variables (.env template)
- [ ] Set up GitHub Actions for CI/CD
- [ ] Create test wallet with testnet ETH

---

## 2. TaskRegistry.sol

Core contract for task management.

### Data Structures
- [ ] Define `Task` struct:
  - `address requester`
  - `string modelRequirements`
  - `bytes32 dataHash` (IPFS CID)
  - `uint256 rewardPerNode`
  - `uint256 requiredNodes`
  - `uint256 deadline`
  - `TaskStatus status` (enum: Pending, Active, Completed, Cancelled)
  - `TaskMode mode` (enum: Standard, ValidatorRouted)
- [ ] Define `TaskResult` struct for submissions
- [ ] Create task ID mapping

### Functions
- [ ] `submitTask()` - Create new task with token deposit
- [ ] `submitTaskValidatorRouted()` - Create high-speed task (higher fee)
- [ ] `claimTask(taskId)` - Node claims a task
- [ ] `submitResult(taskId, resultHash)` - Node submits result
- [ ] `cancelTask(taskId)` - Requester cancels (before completion)
- [ ] `getTask(taskId)` - View task details
- [ ] `getTasksByRequester(address)` - List user's tasks
- [ ] `getAvailableTasks()` - List claimable tasks

### Events
- [ ] `TaskCreated(taskId, requester, reward)`
- [ ] `TaskClaimed(taskId, nodeAddress)`
- [ ] `ResultSubmitted(taskId, nodeAddress, resultHash)`
- [ ] `TaskCompleted(taskId)`
- [ ] `TaskCancelled(taskId)`

### Tests
- [ ] Unit tests for all functions
- [ ] Edge cases (deadline passed, insufficient payment, etc.)
- [ ] Gas optimization tests
- [ ] Integration tests with TokenReward contract

---

## 3. TokenReward.sol

Handles COMP token minting and distribution.

### Token Mechanics
- [ ] ERC-20 implementation for COMP token
- [ ] Emission schedule:
  - Year 1: 100M tokens
  - Year 2: 80M tokens
  - Year 3: 64M tokens
  - Halving every 2 years
- [ ] Distribution logic:
  - 80% to compute nodes
  - 10% to validators
  - 5% to DAO treasury
  - 5% to protocol development (first 2 years)

### Functions
- [ ] `mint(address, amount)` - Internal minting (called by TaskRegistry)
- [ ] `distributeRewards(taskId)` - Pay nodes for completed task
- [ ] `slash(nodeAddress, amount)` - Penalty for bad behavior
- [ ] `burn(amount)` - 2% burn on transactions
- [ ] `getEmissionRate()` - Current emission rate
- [ ] `getTotalSupply()` - Circulating supply

### Tests
- [ ] Emission curve accuracy over simulated years
- [ ] Distribution ratios
- [ ] Slashing mechanics
- [ ] Burn mechanism

---

## 4. ValidatorRegistry.sol

Manages validator staking and routing for high-speed mode.

### Data Structures
- [ ] Define `Validator` struct:
  - `address validatorAddress`
  - `uint256 stakedAmount`
  - `uint256 reputation`
  - `bool isActive`
  - `uint256 tasksRouted`
  - `address[] premiumNodes`

### Functions
- [ ] `stake(amount)` - Stake GOV tokens to become validator
- [ ] `unstake(amount)` - Withdraw stake (with cooldown)
- [ ] `registerPremiumNode(nodeAddress)` - Add high-performance node
- [ ] `routeTask(taskId, nodeAddress)` - Assign task to premium node
- [ ] `attestResult(taskId, resultHash)` - Sign result for fast delivery
- [ ] `getActiveValidators()` - List all active validators
- [ ] `getValidatorStats(address)` - Performance metrics

### Staking Requirements
- [ ] Minimum stake: 10,000 GOV
- [ ] Cooldown period: 7 days for unstaking
- [ ] Slashing conditions defined

### Tests
- [ ] Staking/unstaking flow
- [ ] Task routing to premium nodes
- [ ] Slashing for failed attestations
- [ ] Cooldown enforcement

---

## 5. Governance.sol

DAO voting and treasury management.

### Data Structures
- [ ] Define `Proposal` struct
- [ ] Define `Vote` struct
- [ ] Proposal types enum (Protocol, Treasury, Parameter, Emergency)

### Functions
- [ ] `createProposal(type, description, calldata)` - Submit proposal
- [ ] `vote(proposalId, support)` - Cast vote
- [ ] `executeProposal(proposalId)` - Execute passed proposal
- [ ] `cancelProposal(proposalId)` - Cancel (only proposer)
- [ ] `getProposal(proposalId)` - View proposal
- [ ] `getVotingPower(address)` - Calculate quadratic voting power

### Voting Rules
- [ ] Quadratic voting: power = sqrt(tokens)
- [ ] Maximum 5% of total vote per address
- [ ] Time-lock: 30 days stake before voting
- [ ] Quorum: 10% of circulating GOV
- [ ] Voting period: 7 days
- [ ] Execution delay: 24 hours (except emergency)

### Approval Thresholds
- [ ] Protocol Upgrades: 66%
- [ ] Treasury Spending: 51%
- [ ] Parameter Changes: 51%
- [ ] Emergency Actions: 75%

### Tests
- [ ] Full proposal lifecycle
- [ ] Quadratic voting calculations
- [ ] Quorum checks
- [ ] Timelock enforcement

---

## 6. GOVToken.sol

Governance token (fixed supply).

### Token Mechanics
- [ ] ERC-20 implementation
- [ ] Fixed supply: 100M tokens
- [ ] Initial distribution:
  - 40% to early compute contributors (airdrop)
  - 30% to public genesis sale
  - 20% to DAO treasury (vesting)
  - 10% to core contributors (4-year vesting)

### Functions
- [ ] Standard ERC-20 functions
- [ ] `delegate(address)` - Delegate voting power
- [ ] `getVotes(address)` - Get voting power
- [ ] Snapshot functionality for governance

### Tests
- [ ] Distribution accuracy
- [ ] Delegation mechanics
- [ ] Snapshot functionality

---

## 7. OARNRegistry.sol (Decentralized Discovery)

**CRITICAL: This contract enables NO hardcoded values in clients.**

### Purpose
Single entry point for discovering all OARN infrastructure. Clients query this contract (via ENS: oarn-registry.eth) to find everything else.

### Data Structures
```solidity
contract OARNRegistry {
    // Immutable core contract addresses (set once at deployment)
    address public immutable taskRegistry;
    address public immutable tokenReward;
    address public immutable governance;
    address public immutable govToken;

    // Registered RPC providers (staked)
    struct RPCProvider {
        string endpoint;        // RPC URL
        string onionEndpoint;   // .onion URL (Tor)
        address owner;
        uint256 stake;
        uint256 uptime;         // Percentage * 100
        uint256 reportCount;    // Bad behavior reports
        bool isActive;
    }

    // Registered bootstrap nodes
    struct BootstrapNode {
        string peerId;          // libp2p peer ID
        string onionAddress;    // .onion address
        string i2pAddress;      // .i2p address
        address owner;
        uint256 stake;
        bool isActive;
    }
}
```

### Functions
- [ ] `registerRPCProvider(endpoint, onionEndpoint)` - Stake to register RPC
- [ ] `unregisterRPCProvider()` - Withdraw (with cooldown)
- [ ] `reportRPCProvider(providerId, proof)` - Report bad behavior
- [ ] `getActiveRPCProviders()` - List all active RPCs
- [ ] `getRandomRPCProviders(count)` - Get N random healthy RPCs
- [ ] `registerBootstrapNode(peerId, onion, i2p)` - Stake to register node
- [ ] `unregisterBootstrapNode()` - Withdraw (with cooldown)
- [ ] `getActiveBootstrapNodes()` - List all active bootstrap nodes
- [ ] `getRandomBootstrapNodes(count)` - Get N random nodes

### Staking Requirements
- [ ] RPC Provider minimum stake: 5,000 GOV
- [ ] Bootstrap Node minimum stake: 1,000 GOV
- [ ] Slashing for downtime/bad responses
- [ ] Cooldown: 7 days for withdrawal

### ENS Integration
- [ ] oarn-registry.eth → This contract address
- [ ] oarn-rpc.eth → TXT records with RPC endpoints
- [ ] oarn-bootstrap.eth → TXT records with bootstrap nodes
- [ ] Same address on all chains via CREATE2

### Tests
- [ ] Registration/unregistration flow
- [ ] Slashing mechanics
- [ ] Random selection distribution
- [ ] ENS resolution

---

## 8. Upgradability & Security

- [ ] Implement OpenZeppelin upgradeable proxy pattern
- [ ] Add emergency pause functionality
- [ ] Multi-sig admin for initial deployment (3-of-5)
- [ ] Timelock for all admin functions
- [ ] Re-entrancy guards on all external calls
- [ ] Access control (roles: Admin, Validator, Node)
- [ ] OARNRegistry is NON-UPGRADEABLE (immutable core addresses)

---

## 8. Deployment Checklist

### Testnet (Arbitrum Goerli/Sepolia)
- [ ] Deploy all contracts
- [ ] Verify on Arbiscan
- [ ] Test full workflow end-to-end
- [ ] Load testing (1000+ transactions)

### Mainnet (Arbitrum One)
- [ ] Final security audit passed
- [ ] Multi-sig configured
- [ ] Deployment script reviewed
- [ ] Emergency procedures documented
- [ ] Contract addresses published
- [ ] Verify all contracts on Arbiscan

---

## 9. Documentation

- [ ] NatSpec comments on all public functions
- [ ] Architecture diagram
- [ ] Deployment guide
- [ ] Integration guide for developers
- [ ] Admin operations runbook
