# OARN Testnet Deployment

## Network: Arbitrum Sepolia

- **Chain ID:** 421614
- **RPC:** https://sepolia-rollup.arbitrum.io/rpc
- **Explorer:** https://sepolia.arbiscan.io

## Deployed Contracts

| Contract | Address | Explorer |
|----------|---------|----------|
| **COMP Token** | `0x24249A523A251E38CB0001daBd54DD44Ea8f1838` | [View](https://sepolia.arbiscan.io/address/0x24249A523A251E38CB0001daBd54DD44Ea8f1838) |
| **GOV Token** | `0xB97eDD49C225d2c43e7203aB9248cAbED2B268d3` | [View](https://sepolia.arbiscan.io/address/0xB97eDD49C225d2c43e7203aB9248cAbED2B268d3) |
| **TaskRegistry** | `0x4Dc9dD73834E94545cF041091e1A743FBD09a60f` | [View](https://sepolia.arbiscan.io/address/0x4Dc9dD73834E94545cF041091e1A743FBD09a60f) |
| **OARNRegistry** | `0xa122518Cb6E66A804fc37EB26c8a7aF309dCF04C` | [View](https://sepolia.arbiscan.io/address/0xa122518Cb6E66A804fc37EB26c8a7aF309dCF04C) |

## Deployment Details

- **Deployed:** 2026-02-18
- **Deployer:** `0x7379651E169e63272ec57Ce14f2BfC023e28382E`

## Contract Configuration

### COMP Token
- Minter: TaskRegistry (authorized)
- Burn: Disabled (can be enabled via governance)
- Burn Rate: 2% (when enabled)

### GOV Token
- Treasury: Deployer (placeholder for testnet)
- Team Vesting: Deployer (placeholder for testnet)
- Genesis Distribution: Not yet executed

### TaskRegistry
- Reward Token: COMP Token
- Task Submission: Open

### OARNRegistry
- Core contracts configured:
  - TaskRegistry: `0x4Dc9dD73834E94545cF041091e1A743FBD09a60f`
  - TokenReward: `0x24249A523A251E38CB0001daBd54DD44Ea8f1838` (COMP)
  - GOV Token: `0xB97eDD49C225d2c43e7203aB9248cAbED2B268d3`

## Interacting with Contracts

### Using Hardhat Console

```bash
cd oarn-contracts
npx hardhat console --network arbitrumSepolia
```

```javascript
// Get contract instances
const comp = await ethers.getContractAt("COMPToken", "0x24249A523A251E38CB0001daBd54DD44Ea8f1838");
const gov = await ethers.getContractAt("GOVToken", "0xB97eDD49C225d2c43e7203aB9248cAbED2B268d3");
const tasks = await ethers.getContractAt("TaskRegistry", "0x4Dc9dD73834E94545cF041091e1A743FBD09a60f");
const registry = await ethers.getContractAt("OARNRegistry", "0xa122518Cb6E66A804fc37EB26c8a7aF309dCF04C");

// Check COMP token info
await comp.name(); // "OARN Compute Token"
await comp.symbol(); // "COMP"

// Check GOV token info
await gov.name(); // "OARN Governance Token"
await gov.symbol(); // "GOV"

// Check registry
await registry.taskRegistry();
await registry.govToken();
```

### Using Ethers.js

```javascript
const { ethers } = require("ethers");

const provider = new ethers.JsonRpcProvider("https://sepolia-rollup.arbitrum.io/rpc");

const REGISTRY_ADDRESS = "0xa122518Cb6E66A804fc37EB26c8a7aF309dCF04C";
const REGISTRY_ABI = [...]; // Import from artifacts

const registry = new ethers.Contract(REGISTRY_ADDRESS, REGISTRY_ABI, provider);
const coreContracts = await registry.getCoreContracts();
```

## Next Steps

1. **Verify contracts** on Arbiscan (requires API key)
2. **Execute GOV genesis distribution** (when ready)
3. **Register RPC providers** (stake required)
4. **Register bootstrap nodes** (stake required)
5. **Test task submission flow**
