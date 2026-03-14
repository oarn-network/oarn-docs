
# OARN Discord Server Setup Guide

## Server Creation

1. Go to https://discord.com/
2. Click "+" to create a new server
3. Choose "Create My Own" → "For a community"
4. Name: **OARN Network**
5. Upload logo (use project logo)

---

## Role Hierarchy (Top to Bottom)

### Leadership & Team
| Role | Color | Description | Permissions |
|------|-------|-------------|-------------|
| 👑 Founder | Gold (#FFD700) | Founder/CEO of OARN | Administrator (all rights) |
| 🔧 Core Team | Red (#E74C3C) | Core developers & co-founders | Administrator |
| 🛡️ Admin | Dark Red (#992D22) | Server administrators | Manage server, edit channels, manage roles, bans |

### Moderation
| Role | Color | Description | Permissions |
|------|-------|-------------|-------------|
| ⚔️ Moderator | Orange (#E67E22) | Community moderators | Delete messages, kick, ban, timeout, slowmode |
| 🔨 Trial Mod | Light Orange (#F39C12) | Moderators on probation | Delete messages, timeout (max 1h) |

### Special Roles
| Role | Color | Description | Permissions |
|------|-------|-------------|-------------|
| 💎 VIP | Diamond Blue (#00D4FF) | VIP members (investors, partners, etc.) | Access to VIP channels, early info |
| 🏆 OG | Purple (#9B59B6) | Early supporters (first 100 members) | Badge, exclusive channel access |
| 🤝 Partner | Turquoise (#1ABC9C) | Official partner projects | Partner channel access |

### Community Tiers (Earned through activity/time)
| Role | Color | Description | Requirement |
|------|-------|-------------|-------------|
| 🌟 Veteran | Dark Blue (#2C3E50) | Long-term members | 6+ months active, Level 30+ |
| 💫 Supporter | Blue (#3498DB) | Active members | 2+ months active, Level 15+ |
| ✨ Member | Light Blue (#5DADE2) | Verified members | 2+ weeks active, Level 5+ |
| 👋 Newcomer | Gray (#95A5A6) | New members | Automatic on join |

### Functional Roles
| Role | Color | Description | Purpose |
|------|-------|-------------|---------|
| 💻 Developer | Green (#27AE60) | Active developers/contributors | Dev channel access |
| ⚡ Node Operator | Light Green (#2ECC71) | Runs OARN nodes | Node operator channels |
| 🔬 Researcher | Violet (#8E44AD) | AI/ML researchers | Research channels |
| 🐛 Bug Hunter | Yellow (#F1C40F) | Found/reported bugs | Reward eligibility |
| 📝 Contributor | Cyan (#00BCD4) | Contributed to project | Contributor badge |

### Ping Roles (Self-assignable via Reaction Roles)
| Role | Description |
|------|-------------|
| 📢 Announcements | For project announcements |
| 🔔 Updates | For technical updates |
| 🎮 Events | For community events |
| 🗳️ Governance | For voting notifications |

### Bot Roles
| Role | Description |
|------|-------------|
| 🤖 Bot | All bots |
| 🔗 GitHub Bot | GitHub notifications |
| 📊 Stats Bot | Statistics bot |

---

## Channel Structure

### 📌 INFORMATION (Read-Only for regular users)
```
├── 📜 rules                    # Server rules (must accept to proceed)
├── 👋 welcome                  # Welcome message, server info
├── 📋 roles                    # Reaction roles for self-assignment
├── 🔗 links                    # Important links (website, docs, GitHub, etc.)
├── ❓ faq                      # Frequently asked questions
└── 📍 roadmap                  # Project roadmap and milestones
```

### 📢 ANNOUNCEMENTS (Admin/Mod only)
```
├── 📣 announcements            # Official project announcements
├── 📰 news                     # General news & updates
├── 🔄 changelog                # Technical changes & releases
├── 🤖 bot-announcements        # Bot notifications (GitHub, etc.)
└── 🗳️ polls                    # Surveys & community votes
```

### 💬 COMMUNITY
```
├── 🗨️ general                  # General chat
├── 🎉 introductions            # New members introduce themselves
├── 💡 ideas-suggestions        # Feature suggestions & ideas
├── 🖼️ media                    # Images, videos, content sharing
├── 🎮 off-topic                # Everything except OARN
├── 🔥 memes                    # Meme channel
└── 🌍 international            # Non-English discussions
```

### 💻 DEVELOPMENT
```
├── 💬 dev-general              # General dev discussions
├── 📜 smart-contracts          # Solidity & contract development
├── 🦀 rust-node                # Rust node software development
├── 🌐 frontend                 # Web frontend development
├── 🔧 infrastructure           # DevOps, CI/CD, deployment
├── 🔍 code-review              # PR reviews & code discussions
├── 🐛 bug-reports              # Bug reports
└── 📚 resources                # Dev resources, tutorials, docs
```

### ⚡ NODE OPERATORS
```
├── 📖 node-guide               # Node setup guide (Read-Only)
├── 💬 node-chat                # General node operator chat
├── 🛠️ node-setup               # Setup assistance
├── 🚨 node-troubleshooting     # Technical issues
├── 💾 hardware-specs           # Hardware recommendations
└── 📊 node-stats               # Node statistics & performance
```

### 🔬 RESEARCH
```
├── 🧠 research-general         # General research discussions
├── 📄 papers                   # Academic paper discussions
├── 🤖 ai-ml                    # AI/ML specific topics
├── 🔗 partnerships             # Research partnerships
└── 💼 grants                   # Research grants & funding
```

### 🏛️ GOVERNANCE
```
├── 📜 governance-info          # Governance explanation (Read-Only)
├── 💬 governance-discussion    # Governance discussions
├── 📝 proposals                # Proposal discussions
└── 🗳️ voting                   # Active votes
```

### 🎫 SUPPORT
```
├── 🎫 create-ticket            # Create ticket (bot-controlled)
├── ❓ help                     # General help
└── 💰 tokenomics               # Questions about COMP/GOV tokens
```

### 💎 VIP (Only visible for VIP role)
```
├── 💬 vip-lounge               # VIP chat
├── 📢 vip-announcements        # Exclusive VIP announcements
└── 🔮 alpha                    # Early info & alpha leaks
```

### 🏆 OG (Only visible for OG role)
```
└── 🏆 og-club                  # Exclusive OG channel
```

### 🔊 VOICE CHANNELS
```
├── 🔊 General Voice            # General voice chat
├── 🔊 Chill Lounge             # Relaxed voice chat
├── 🔊 Gaming                   # Gaming voice
├── 💼 Dev Meeting              # Developer meetings
├── 💼 Team Meeting             # Team meetings (Mod+ only)
├── 🎤 AMA / Events             # For AMAs and events
└── 🔇 AFK                      # AFK channel
```

### 🔒 STAFF (Only visible for Mods/Admins)
```
├── 📋 staff-info               # Staff info & guidelines
├── 💬 staff-chat               # Staff discussions
├── 📝 mod-logs                 # Moderation logs
├── 🚨 reports                  # User reports
├── 🗳️ staff-voting             # Internal votes
└── 🔊 Staff Voice              # Staff voice channel
```

---

## Bots

### Moderation & Management
| Bot | Function |
|-----|----------|
| **MEE6** | Leveling, auto-mod, welcome messages |
| **Carl-bot** | Reaction roles, logging, auto-mod |
| **Dyno** | Backup moderation, announcements |

### Utility
| Bot | Function |
|-----|----------|
| **GitHub Bot** | Repo notifications → #bot-announcements |
| **Statbot** | Server statistics |
| **Ticket Tool** | Support ticket system |

### Fun & Engagement
| Bot | Function |
|-----|----------|
| **Arcane** | Leveling with rewards |
| **YAGPDB** | Custom commands, reminders |

### Crypto/Web3 (Optional)
| Bot | Function |
|-----|----------|
| **Collab.Land** | Token-gated roles (for token holders) |
| **Guild.xyz** | Web3-based role assignment |

---

## Automations

### On Server Join
1. User automatically receives `👋 Newcomer` role
2. Welcome message in #welcome with ping
3. User must accept rules in #rules
4. After accepting: Access to community channels

### Level System (MEE6/Arcane)
| Level | Role | Bonus |
|-------|------|-------|
| 5 | ✨ Member | Post images, reactions |
| 15 | 💫 Supporter | GIFs, embeds, external emojis |
| 30 | 🌟 Veteran | Change nickname, screen share |

### Reaction Roles (#roles)
Users can choose these roles via reactions:
- 📢 Announcements Ping
- 🔔 Updates Ping
- 🎮 Events Ping
- 🗳️ Governance Ping
- 💻 Developer (for dev channel access)
- ⚡ Node Operator (for node channel access)
- 🔬 Researcher (for research channel access)

---

## Server Settings

### General
- **Community Server:** ✅ ON
- **Rules Screening:** ✅ ON
- **Membership Screening:** ✅ ON
- **2FA for Moderators:** ✅ ON
- **Explicit Media Filter:** Scan all messages
- **Default Notification:** Only @mentions

### Verification Level
- **Medium** (Email verified + 5 minutes on server)

### Auto-Mod
- Spam filter: ON
- Link filter: ON (whitelist for known sites)
- Caps filter: ON (max 70% caps)
- Invite filter: ON (no foreign Discord invites)
- Mention spam: Max 5 mentions per message

---

## Quick Setup Checklist

- [ ] Create server with name "OARN Network"
- [ ] Upload logo
- [ ] Create all roles (watch hierarchy!)
- [ ] Create channel categories
- [ ] Create all channels
- [ ] Set channel permissions
- [ ] Invite bots (MEE6, Carl-bot, etc.)
- [ ] Set up reaction roles
- [ ] Configure welcome messages
- [ ] Set up auto-mod
- [ ] Fill #rules with rules
- [ ] Fill #welcome with server info
- [ ] Fill #faq with frequent questions
- [ ] Fill #links with important links
- [ ] Set server to "Community"
- [ ] Set verification level
- [ ] Test run with fake account
