# Mediation Analysis Ecosystem - Coordination Hub

> **Ecosystem coordination, planning, and cross-package documentation for the mediationverse**

This repository serves as the **coordination hub** for the mediation analysis R package ecosystem. For user-facing R package documentation, see [mediationverse](https://github.com/Data-Wise/mediationverse).

---

## 🎯 Quick Start

| Document | Purpose |
|----------|---------|
| **[PROJECT-HUB.md](PROJECT-HUB.md)** | 📊 **Start here** - Dashboard, progress, next actions |
| [ROADMAP.md](docs/ROADMAP.md) | 🗺️ Master roadmap - Detailed timeline, phases, milestones |
| [TODOS.md](TODOS.md) | ✅ Active tasks across all packages |
| [IDEAS.md](IDEAS.md) | 💡 Future enhancements and proposals |
| [CHANGELOG.md](CHANGELOG.md) | 📋 Ecosystem-wide changes |

---

## 📁 Documentation Structure

### Root Files
| File | Purpose |
|------|---------|
| [PROJECT-HUB.md](PROJECT-HUB.md) | Central dashboard and coordination |
| [TODOS.md](TODOS.md) | Cross-package task tracking |
| [IDEAS.md](IDEAS.md) | Future enhancements |
| [CHANGELOG.md](CHANGELOG.md) | Change history |
| [.STATUS](.STATUS) | Quick status summary |

### Directories

**`docs/`** - Planning & Coordination
| File | Purpose |
|------|---------|
| [ROADMAP.md](docs/ROADMAP.md) | Master development roadmap |
| [ECOSYSTEM-PACKAGES.md](docs/ECOSYSTEM-PACKAGES.md) | Package summaries |
| [ECOSYSTEM-COORDINATION.md](docs/ECOSYSTEM-COORDINATION.md) | Package dependencies, release order |
| [DEPENDENCY-MAP.md](docs/DEPENDENCY-MAP.md) | Dependency visualization |
| [INTEGRATION-PLAN.md](docs/INTEGRATION-PLAN.md) | Cross-package integration |

**`specs/`** - Technical Specifications
| File | Purpose |
|------|---------|
| [API-CONTRACTS.md](specs/API-CONTRACTS.md) | S7 class definitions, shared parameters |
| [DEVELOPMENT-STANDARDS.md](specs/DEVELOPMENT-STANDARDS.md) | Code style, testing, documentation |
| [BRANCHING-STRATEGY.md](specs/BRANCHING-STRATEGY.md) | Git workflow for ecosystem |
| [GENERIC-FUNCTIONS-STRATEGY.md](specs/GENERIC-FUNCTIONS-STRATEGY.md) | API design, S7/S3 interop |

**`proposals/`** - Ideas Awaiting Decisions
| File | Purpose |
|------|---------|
| [COORDINATION-HUB-BRAINSTORM.md](proposals/COORDINATION-HUB-BRAINSTORM.md) | Coordination tooling ideas |

**`tooling/`** - Claude/MCP Integration
- Claude configuration and MCP server setup

---

## 📦 Package Ecosystem

```
mediationverse (meta-package)
├── medfit (foundation) ✅ 100% complete - ready for v0.1.0
├── probmed (next)
├── RMediation (CRAN stable)
├── medrobust
└── medsim
```

**Current Status**: Phase 2 Integration Ready (3 months ahead of schedule!)

| Package | Status | Progress | Next Action |
|---------|--------|----------|-------------|
| **medfit** | ✅ Complete | 100% | Final merge, tag v0.1.0 |
| **probmed** | 🚧 Ready | 55% | Integrate with medfit (4-6 hrs) |
| **RMediation** | ✅ CRAN | Stable | Plan medfit integration |
| **medrobust** | 🔧 Active | 90% | Plan medfit integration |
| **medsim** | ✅ Complete | 85% | Standard scenarios |
| **mediationverse** | 🔧 Active | 70% | Meta-package coordination |

**See [PROJECT-HUB.md](PROJECT-HUB.md) for detailed progress and next steps**

---

## 🚀 Development Workflow

```bash
# Clone all packages
cd ~/projects/r-packages/active
for pkg in medfit probmed medrobust medsim mediationverse; do
  gh repo clone Data-Wise/$pkg
done

# Check status
cd ~/projects/r-packages/mediation-planning
bat PROJECT-HUB.md
```

---

## 🔗 Related Repositories

| Repository | Purpose | Link |
|------------|---------|------|
| **medfit** | Foundation package | [Data-Wise/medfit](https://github.com/Data-Wise/medfit) |
| **probmed** | Probabilistic mediation (P_med) | [Data-Wise/probmed](https://github.com/Data-Wise/probmed) |
| **RMediation** | Distribution methods (CRAN) | [Data-Wise/rmediation](https://github.com/Data-Wise/rmediation) |
| **medrobust** | Sensitivity analysis | [Data-Wise/medrobust](https://github.com/Data-Wise/medrobust) |
| **medsim** | Simulation infrastructure | [Data-Wise/medsim](https://github.com/Data-Wise/medsim) |
| **mediationverse** | Meta-package | [Data-Wise/mediationverse](https://github.com/Data-Wise/mediationverse) |

**Websites:**
- [medfit](https://data-wise.github.io/medfit/)
- [mediationverse](https://data-wise.github.io/mediationverse/)

---

## 📞 Contact

**Project Lead**: Davood Tofighi (dtofighi@gmail.com)

For detailed planning and coordination, see [PROJECT-HUB.md](PROJECT-HUB.md)
