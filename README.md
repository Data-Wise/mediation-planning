# Mediation Analysis Ecosystem - Coordination Hub

> **Ecosystem coordination, planning, and cross-package documentation for the mediationverse**

This repository serves as the **coordination hub** for the mediation analysis R package ecosystem. For user-facing R package documentation, see [mediationverse](https://github.com/Data-Wise/mediationverse).

---

## 🎯 Quick Start

| Document | Purpose |
|----------|---------|
| **[PROJECT-HUB.md](PROJECT-HUB.md)** | 📊 **Start here** - Front door: links, focus, doc map |
| [ECOSYSTEM-MANIFEST.yaml](ECOSYSTEM-MANIFEST.yaml) | 📦 The 7 packages, paths, roles, CRAN state |
| [docs/RFORGE-COMMANDS.md](docs/RFORGE-COMMANDS.md) | ⚙️ Live status/deps/CRAN-order — generated, not hand-maintained |
| [ROADMAP.md](docs/ROADMAP.md) | 🗺️ Master roadmap - Detailed timeline, phases, milestones |
| [TODOS.md](TODOS.md) | ✅ Active tasks across all packages |
| [IDEAS.md](IDEAS.md) | 💡 Future enhancements and proposals |
| [CHANGELOG.md](CHANGELOG.md) | 📋 Ecosystem-wide changes |

> **Hybrid model:** this hub holds *prose* (roadmap, decisions, standards, specs).
> Anything computable — live status, dependency order, CRAN sequence — is generated
> by **rforge** (`cd ~/projects/r-packages && /rforge:status`), not tabulated here.

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
| [ECOSYSTEM-COORDINATION.md](docs/ECOSYSTEM-COORDINATION.md) | Version matrix, change propagation, S7 stability, API compatibility |
| [DEPENDENCY-MAP.md](docs/DEPENDENCY-MAP.md) | Dependency narrative (live graph via `/rforge:deps`) |
| [INTEGRATION-PLAN.md](docs/INTEGRATION-PLAN.md) | Cross-package integration |
| [RFORGE-COMMANDS.md](docs/RFORGE-COMMANDS.md) | The dynamic layer (rforge command cheat-sheet) |
| [design/](docs/design/) | API & naming design rationale (relocated from medfit 2026-06-10) |

**`specs/`** - Technical Specifications (normative)
| File | Purpose |
|------|---------|
| [API-CONTRACTS.md](specs/API-CONTRACTS.md) | S7 class definitions, shared parameters |
| [DEVELOPMENT-STANDARDS.md](specs/DEVELOPMENT-STANDARDS.md) | Code style, testing, documentation |
| [BRANCHING-STRATEGY.md](specs/BRANCHING-STRATEGY.md) | Git workflow for ecosystem |
| [GENERIC-FUNCTIONS-STRATEGY.md](specs/GENERIC-FUNCTIONS-STRATEGY.md) | API design, S7/S3 interop |

**`proposals/`** - Ideas Awaiting Decisions
| File | Purpose |
|------|---------|
| [MEDIATIONVERSE-PROPOSAL.md](proposals/MEDIATIONVERSE-PROPOSAL.md) | Meta-package specification (relocated from medfit 2026-06-10) |
| [MEDSIM-PROPOSAL.md](proposals/MEDSIM-PROPOSAL.md) | Simulation infrastructure design |
| [ECOSYSTEM-NEXT-STEPS-2026-05-30.md](proposals/ECOSYSTEM-NEXT-STEPS-2026-05-30.md) | Post-hardening next steps |

**`standards/`** - Universal cross-project conventions (vendored; not mediation-specific)

**`archive/`** - Superseded docs ([index](archive/README.md))

**`tooling/`** - Claude/MCP Integration; Claude configuration and skill plans

---

## 📦 Package Ecosystem

```
mediationverse (meta-package)
├── medfit (foundation)
├── probmed
├── RMediation (CRAN)
├── medrobust
├── medsim
└── missingmed (adopted 2026-06)
```

The authoritative package registry is [ECOSYSTEM-MANIFEST.yaml](ECOSYSTEM-MANIFEST.yaml).

**Live per-package status is not tabulated here** (it drifts). Generate it:

```bash
cd ~/projects/r-packages && /rforge:status      # or :thorough for the full rollup
```

**See [PROJECT-HUB.md](PROJECT-HUB.md) for the focus narrative and document map.**

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
| **missingmed** | Missing-data mediation (S4, MI) | [Data-Wise/missingmed](https://github.com/Data-Wise/missingmed) |
| **mediationverse** | Meta-package | [Data-Wise/mediationverse](https://github.com/Data-Wise/mediationverse) |

**Websites:**
- [medfit](https://data-wise.github.io/medfit/)
- [mediationverse](https://data-wise.github.io/mediationverse/)

---

## 📞 Contact

**Project Lead**: Davood Tofighi (dtofighi@gmail.com)

For detailed planning and coordination, see [PROJECT-HUB.md](PROJECT-HUB.md)
