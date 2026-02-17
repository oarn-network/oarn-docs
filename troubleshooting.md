# OARN Troubleshooting Guide

This guide helps diagnose and resolve common issues with OARN.

## Quick Diagnostics

Run these commands first to identify the issue:

```bash
# Check node status
oarn-node status

# Check connectivity
oarn-node check-rpc
oarn-node check-ipfs
oarn-node peers

# View recent logs
journalctl -u oarn-node -n 100 --no-pager
```

## Network Issues

### Cannot Connect to Peers

**Symptoms:**
- Node shows 0 connected peers
- "No bootstrap nodes found" error

**Solutions:**

1. **Check firewall settings:**
   ```bash
   # Linux
   sudo ufw allow 4001/tcp

   # Check if port is open
   nc -zv localhost 4001
   ```

2. **Verify bootstrap configuration:**
   ```toml
   # In ~/.oarn/config.toml
   [network.discovery]
   method = "auto"
   # Or try manual bootstrap for testing:
   method = "manual"
   manual_bootstrap = [
       "/ip4/NODE_IP/tcp/4001/p2p/PEER_ID"
   ]
   ```

3. **Check NAT traversal:**
   - Enable UPnP on your router
   - Or manually forward port 4001

4. **Try with verbose logging:**
   ```bash
   RUST_LOG=libp2p=debug oarn-node start
   ```

### "No RPC Providers Discovered"

**Symptoms:**
- Cannot submit/claim tasks
- Blockchain operations fail

**Solutions:**

1. **Check ENS resolution:**
   - Verify `oarn-registry.eth` resolves correctly
   - Try a public ENS resolver

2. **Use manual RPC temporarily:**
   ```toml
   [blockchain]
   rpc_discovery = "manual"
   manual_rpc_url = "https://sepolia-rollup.arbitrum.io/rpc"
   ```

3. **Check chain ID:**
   ```toml
   [blockchain]
   chain_id = 421614  # Arbitrum Sepolia
   # or
   chain_id = 42161   # Arbitrum Mainnet
   ```

### High Latency / Slow Connections

**Solutions:**

1. **Increase peer limit:**
   ```toml
   [network]
   max_peers = 100
   ```

2. **Adjust connection timeout:**
   ```toml
   [network]
   connection_timeout = 60  # seconds
   ```

3. **Enable multiple listen addresses:**
   ```toml
   [network]
   listen_addresses = [
       "/ip4/0.0.0.0/tcp/4001",
       "/ip6/::/tcp/4001",
       "/ip4/0.0.0.0/udp/4001/quic"
   ]
   ```

## IPFS Issues

### "Could Not Connect to IPFS"

**Symptoms:**
- "Failed to create IPFS client" error
- Cannot fetch models or submit results

**Solutions:**

1. **Check IPFS daemon:**
   ```bash
   # Start IPFS
   ipfs daemon

   # Check if running
   ipfs swarm peers
   ```

2. **Verify API endpoint:**
   ```toml
   [storage]
   ipfs_api = "http://127.0.0.1:5001"
   ```

3. **Check IPFS config:**
   ```bash
   # Enable API access
   ipfs config Addresses.API /ip4/127.0.0.1/tcp/5001
   ```

### "CID Not Found" / Timeout

**Solutions:**

1. **Check if content is pinned somewhere:**
   ```bash
   ipfs dht findprovs QmCID
   ```

2. **Increase timeout:**
   ```bash
   ipfs config Datastore.GCPeriod "24h"
   ```

3. **Connect to known gateways:**
   ```bash
   ipfs swarm connect /dnsaddr/bootstrap.libp2p.io
   ```

### Cache Full

**Symptoms:**
- Disk space warnings
- Slow model loading

**Solutions:**

1. **Increase cache limit:**
   ```toml
   [storage]
   max_cache_mb = 20480  # 20 GB
   ```

2. **Clean cache manually:**
   ```bash
   rm -rf ~/.oarn/cache/*
   ```

3. **Run IPFS garbage collection:**
   ```bash
   ipfs repo gc
   ```

## Blockchain Issues

### "Transaction Failed"

**Common causes and solutions:**

1. **Insufficient gas:**
   - Increase gas limit in wallet settings
   - Wait for lower gas prices

2. **Nonce issues:**
   ```bash
   # Reset nonce in wallet
   oarn-node wallet reset-nonce
   ```

3. **Contract reverted:**
   - Check revert reason in transaction details
   - Common: "Insufficient stake", "Already registered"

### "Insufficient Funds"

**Solutions:**

1. **For testnet:**
   - Get Arbitrum Sepolia ETH from faucets:
     - https://www.alchemy.com/faucets/arbitrum-sepolia
     - https://faucet.quicknode.com/arbitrum/sepolia

2. **Check balance:**
   ```bash
   oarn-node wallet balance
   ```

### "Contract Not Found"

**Solutions:**

1. **Verify network:**
   - Ensure you're on the correct network (testnet vs mainnet)

2. **Check contract addresses:**
   - Query OARNRegistry for correct addresses
   - Verify ENS resolution

## Compute Issues

### "Cannot Handle Task Requirements"

**Symptoms:**
- Tasks not being claimed
- "Insufficient resources" logs

**Solutions:**

1. **Check supported frameworks:**
   ```toml
   [compute]
   frameworks = ["onnx", "pytorch", "tensorflow"]
   ```

2. **Verify resource detection:**
   ```bash
   oarn-node status | grep -A 5 "Resources"
   ```

3. **Manually set limits:**
   ```toml
   [compute]
   max_vram_mb = 8192
   max_ram_mb = 32768
   ```

### Model Execution Fails

**Common errors:**

1. **"Framework not supported":**
   - Install required runtime (ONNX Runtime, PyTorch)
   - Add to `frameworks` list in config

2. **"Out of memory":**
   - Reduce `concurrent_tasks` to 1
   - Close other applications
   - Reduce batch size

3. **"Model load failed":**
   - Check model file integrity
   - Verify framework version compatibility

### Slow Inference

**Solutions:**

1. **Enable GPU acceleration:**
   - Install CUDA/ROCm
   - Rebuild with GPU support

2. **Adjust concurrent tasks:**
   ```toml
   [compute]
   concurrent_tasks = 1  # Reduce for large models
   ```

3. **Check thermal throttling:**
   ```bash
   # Linux
   sensors
   # Or check nvidia-smi for GPUs
   nvidia-smi
   ```

## Wallet Issues

### "Keystore Not Found"

**Solutions:**

1. **Create new wallet:**
   ```bash
   oarn-node wallet create
   ```

2. **Import existing:**
   ```bash
   oarn-node wallet import --mnemonic "your words here"
   ```

3. **Check keystore path:**
   ```toml
   [wallet]
   keystore_path = "~/.oarn/keystore.json"
   ```

### "Invalid Password"

**Solutions:**

1. **Reset wallet:** (WARNING: Backup first!)
   ```bash
   rm ~/.oarn/keystore.json
   oarn-node wallet create
   ```

2. **Check for special characters:**
   - Some terminals escape characters differently

## Configuration Issues

### "Failed to Parse Config"

**Solutions:**

1. **Validate TOML syntax:**
   ```bash
   # Online validator or:
   cat ~/.oarn/config.toml | python3 -c "import sys,toml; toml.load(sys.stdin)"
   ```

2. **Reset to defaults:**
   ```bash
   mv ~/.oarn/config.toml ~/.oarn/config.toml.bak
   oarn-node init
   ```

3. **Check for typos in field names:**
   - Field names are case-sensitive
   - Use underscores, not hyphens

### Node Won't Start

**Solutions:**

1. **Check for port conflicts:**
   ```bash
   lsof -i :4001
   netstat -tulpn | grep 4001
   ```

2. **Check permissions:**
   ```bash
   ls -la ~/.oarn/
   # Should be owned by current user
   ```

3. **Run with verbose output:**
   ```bash
   RUST_LOG=trace oarn-node start 2>&1 | head -100
   ```

## Logging and Debugging

### Enable Debug Logging

```bash
# All modules
RUST_LOG=debug oarn-node start

# Specific modules
RUST_LOG=oarn_node::network=debug,oarn_node::compute=info oarn-node start

# Very verbose (including dependencies)
RUST_LOG=trace oarn-node start
```

### Log Files

```bash
# Systemd logs
journalctl -u oarn-node -f

# Save to file
oarn-node start 2>&1 | tee oarn.log
```

### Profiling

```bash
# CPU profiling
cargo build --release --features profiling
RUST_LOG=info perf record ./target/release/oarn-node start

# Memory profiling
cargo build --release --features memory-profiling
```

## Getting Help

If you've tried the above solutions and still have issues:

1. **Search existing issues:**
   https://github.com/oarn-network/oarn-node/issues

2. **Open a new issue with:**
   - Operating system and version
   - Node version (`oarn-node --version`)
   - Relevant config (remove sensitive data)
   - Full error message and logs
   - Steps to reproduce

3. **Join Discord:**
   Real-time community support

4. **Email:**
   oarn@proton.me for security issues (not general support)
