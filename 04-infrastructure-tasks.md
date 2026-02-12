# Infrastructure Tasks

## Overview
Core infrastructure components: P2P networking, storage, bootstrap nodes, and supporting services.

---

## 1. P2P Network Infrastructure

### Bootstrap Nodes (Decentralized Discovery - No Hardcoding)
- [ ] Deploy 5+ geographically distributed bootstrap nodes
  - [ ] US East (Virginia)
  - [ ] US West (Oregon)
  - [ ] Europe (Frankfurt)
  - [ ] Asia (Singapore)
  - [ ] South America (Sao Paulo)
- [ ] Each node runs as Tor hidden service (.onion)
- [ ] Each node runs as I2P eepsite
- [ ] Register nodes in DHT with well-known keys
- [ ] Register nodes in ENS (oarn-bootstrap.eth TXT records)
- [ ] NO static peer IDs hardcoded in client
- [ ] Peer IDs discoverable via DHT/ENS only
- [ ] Set up monitoring and alerting
- [ ] Implement automatic restart on failure
- [ ] Rotate node identities periodically

### DHT (Distributed Hash Table)
- [ ] Configure Kademlia DHT parameters
- [ ] Set up DHT crawlers for network visibility
- [ ] Implement content routing for tasks
- [ ] Test DHT convergence time

### Relay Nodes (NAT Traversal)
- [ ] Deploy relay nodes for NAT traversal
- [ ] Configure circuit relay v2
- [ ] Set bandwidth limits per relay
- [ ] Monitor relay usage

---

## 2. IPFS Infrastructure

### Pinning Service
- [ ] Deploy dedicated IPFS pinning nodes
- [ ] Implement pinning strategy:
  - Models: Permanent pin
  - Task inputs: Pin until task completion + 7 days
  - Results: Pin until claimed + 30 days
- [ ] Set up garbage collection schedule
- [ ] Monitor storage usage

### IPFS Cluster
- [ ] Deploy IPFS Cluster for coordination
- [ ] Configure replication factor (3x minimum)
- [ ] Set up automatic rebalancing
- [ ] Implement health checks

### IPFS Gateway
- [ ] Deploy public IPFS gateway
- [ ] Configure caching (Cloudflare/nginx)
- [ ] Rate limiting
- [ ] Access logging

---

## 3. Blockchain Infrastructure

### RPC Endpoints (Decentralized - No Hardcoding)
- [ ] Deploy RPCRegistry.sol on-chain
  - [ ] Staked RPC providers register on-chain
  - [ ] Slashing for downtime/bad responses
  - [ ] Reputation scoring
- [ ] Register RPC providers in ENS (oarn-rpc.eth)
- [ ] Register RPC providers in DHT (/oarn/rpc/providers)
- [ ] NO hardcoded RPC URLs in any client code
- [ ] Clients discover RPCs via: ENS → DHT → On-chain registry
- [ ] Set up private Arbitrum node (optional, for reliability)
- [ ] Health checking with automatic failover
- [ ] Response verification (compare multiple RPCs)
- [ ] Load balancing across providers
- [ ] Tor-accessible RPC endpoints (.onion)

### Indexing (The Graph)
- [ ] Create subgraph for TaskRegistry events
- [ ] Create subgraph for TokenReward events
- [ ] Create subgraph for Governance events
- [ ] Deploy to The Graph hosted service
- [ ] Test query performance

### Event Monitoring
- [ ] Set up event listener service
- [ ] Configure WebSocket connections
- [ ] Implement event caching/replay
- [ ] Alert on critical events (large transfers, slashing)

---

## 4. Web Infrastructure

### API Gateway
- [ ] Deploy REST API for:
  - Task submission
  - Node registration
  - Statistics
  - Leaderboards
- [ ] Implement authentication (wallet signature)
- [ ] Rate limiting per wallet
- [ ] API documentation (OpenAPI/Swagger)

### Web Application
- [ ] Task submission interface
- [ ] Results viewer
- [ ] Node dashboard
- [ ] Wallet integration (MetaMask, WalletConnect)
- [ ] Network statistics page
- [ ] Governance voting interface

### Hosting
- [ ] Deploy to decentralized hosting (IPFS + ENS)
- [ ] Fallback: Traditional CDN (Cloudflare Pages)
- [ ] SSL/TLS configuration
- [ ] DDoS protection

---

## 5. Monitoring & Observability

### Metrics Stack
- [ ] Deploy Prometheus for metrics collection
- [ ] Deploy Grafana for visualization
- [ ] Create dashboards:
  - [ ] Network health (nodes, tasks, latency)
  - [ ] Token economics (supply, velocity)
  - [ ] Node performance
  - [ ] Infrastructure health

### Logging
- [ ] Centralized logging (Loki or ELK)
- [ ] Log retention policy (90 days)
- [ ] Log search and alerting

### Alerting
- [ ] PagerDuty/Opsgenie integration
- [ ] Alert rules:
  - [ ] Bootstrap node down
  - [ ] High error rate
  - [ ] Smart contract anomalies
  - [ ] Token price volatility (optional)
- [ ] Escalation policies

### Uptime Monitoring
- [ ] External uptime checks (UptimeRobot, Pingdom)
- [ ] Synthetic transaction testing
- [ ] Status page (status.oarn.network)

---

## 6. Development Infrastructure

### CI/CD
- [ ] GitHub Actions for all repos
- [ ] Automated testing on PR
- [ ] Automated deployment (testnet)
- [ ] Release automation (semantic versioning)

### Development Environment
- [ ] Docker Compose for local development
- [ ] Local testnet (Hardhat node)
- [ ] Mock IPFS (js-ipfs)
- [ ] Test data generators

### Code Quality
- [ ] Linting (Rust: clippy, Python: ruff, Solidity: solhint)
- [ ] Formatting (rustfmt, black, prettier)
- [ ] Security scanning (cargo-audit, npm audit, slither)
- [ ] Coverage reporting

---

## 7. Security Infrastructure

### Secrets Management
- [ ] HashiCorp Vault or AWS Secrets Manager
- [ ] Rotate keys regularly
- [ ] Audit secret access

### Network Security
- [ ] Firewall rules (allow only necessary ports)
- [ ] VPN for admin access
- [ ] SSH key management (no passwords)
- [ ] Intrusion detection (fail2ban, etc.)

### DDoS Protection
- [ ] Cloudflare for web properties
- [ ] Rate limiting on all public endpoints
- [ ] Geographic blocking (if necessary)

---

## 8. Backup & Disaster Recovery

### Backups
- [ ] Bootstrap node configurations
- [ ] Smart contract deployment artifacts
- [ ] Database backups (if any centralized components)
- [ ] Documentation and runbooks

### Disaster Recovery
- [ ] Document recovery procedures
- [ ] Test recovery quarterly
- [ ] Multi-region redundancy
- [ ] Incident response plan

---

## 9. Cost Estimation

### Monthly Infrastructure Costs (Estimated)

| Component | Provider | Est. Cost |
|-----------|----------|-----------|
| Bootstrap Nodes (5x) | Hetzner/OVH | $200 |
| IPFS Pinning Nodes (3x) | Hetzner/OVH | $150 |
| RPC Node (optional) | AWS/GCP | $500 |
| Monitoring Stack | Self-hosted | $50 |
| Web Hosting | Cloudflare | $20 |
| Domain Names | Various | $50/year |
| **Total** | | **~$1,000/month** |

### Funding Sources
- [ ] DAO treasury (post-mainnet)
- [ ] Ethereum Foundation grant
- [ ] Gitcoin grants
- [ ] Community donations

---

## 10. Infrastructure Timeline

### Phase 1 (Months 0-3)
- [ ] 3 bootstrap nodes (dev/test)
- [ ] Local IPFS for testing
- [ ] Basic monitoring

### Phase 2 (Months 3-6)
- [ ] 5 bootstrap nodes (production-grade)
- [ ] IPFS cluster deployed
- [ ] Full monitoring stack
- [ ] API gateway

### Phase 3 (Months 6-9)
- [ ] All production infrastructure live
- [ ] Redundancy in place
- [ ] DR tested

### Phase 4 (Months 9-12)
- [ ] Scale for 10,000+ nodes
- [ ] Performance optimization
- [ ] Hand off to DAO for funding
