# MediationVerse Technical Coordination

Technical documentation for the MediationVerse R package ecosystem.

---

## 🔗 Planning Ecosystem

| Document | Location | Purpose |
|----------|----------|---------|
| [PROJECT-BOARD.md](https://github.com/Data-Wise/mediation-planning) | r-packages/ | Daily ADHD tasks |
| [NOW.md](https://github.com/Data-Wise/data-wise) | data-wise/planning/ | Current focus |
| **This repo** | mediation-planning/ | Technical coordination |
| [GitHub Projects](https://github.com/orgs/Data-Wise/projects) | GitHub | Issue tracking |

---

## 📁 Contents

| File | Purpose |
|------|---------|
| [API-CONTRACTS.md](./API-CONTRACTS.md) | S7 class definitions, shared parameters |
| [BRANCHING-STRATEGY.md](./BRANCHING-STRATEGY.md) | Git workflow for ecosystem |
| [DEVELOPMENT-STANDARDS.md](./DEVELOPMENT-STANDARDS.md) | Code style, testing, docs |
| [ECOSYSTEM-COORDINATION.md](./ECOSYSTEM-COORDINATION.md) | Package dependencies, release order |
| [MONTHLY-CHECKLIST.md](./MONTHLY-CHECKLIST.md) | Recurring maintenance tasks |

---

## 📦 Package Ecosystem

```
mediationverse (meta-package)
├── medfit (foundation) ← START HERE
├── probmed
├── RMediation  
├── medrobust
└── medsim
```

**CRAN Queue:** medfit → probmed → rmediation → medrobust/medsim → mediationverse

---

## 🚀 Quick Start

```bash
# Clone all packages
cd ~/projects/r-packages/active
for pkg in medfit probmed medrobust medsim mediationverse; do
  gh repo clone Data-Wise/$pkg
done

# Or open VS Code workspace
code ~/projects/r-packages/mediationverse-dev.code-workspace
```
