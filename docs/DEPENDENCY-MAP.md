# MediationVerse Ecosystem Dependency Map

**Last Updated:** December 12, 2025  
**Coordinator:** Claude (automated monitoring)

---

## Dependency Graph

```
                    ┌─────────────────────────────────────┐
                    │         mediationverse              │
                    │  (meta-package: imports all)        │
                    └──────────────┬──────────────────────┘
                                   │
           ┌───────────┬───────────┼───────────┬───────────┐
           ▼           ▼           ▼           ▼           ▼
      ┌────────┐  ┌────────┐  ┌────────┐  ┌────────┐  ┌──────────┐
      │probmed │  │medsim  │  │medrobust│ │RMediation│ │  medfit  │
      └───┬────┘  └───┬────┘  └────────┘  └────┬─────┘ │(FOUNDATION)│
          │           │                        │        └─────┬─────┘
          │           │                        │              │
          └───────────┴────────────────────────┴──────────────┘
                              │
                              ▼
                         [ medfit ]
                    (S7 classes, bootstrap,
                     extraction, fitting)
```

---

## Package Roles & Dependencies

| Package | Role | Hard Deps | Soft Deps | Blocks |
|---------|------|-----------|-----------|--------|
| **medfit** | Foundation | S7, checkmate | lavaan | probmed, medsim, mediationverse |
| **probmed** | P_med effect | medfit, S7 | lavaan, mediation | mediationverse |
| **RMediation** | CIs (CRAN) | S7, lavaan, MASS | medfit | mediationverse |
| **medrobust** | Sensitivity | S7, dplyr, ggplot2 | EValue, boot | mediationverse |
| **medsim** | Simulation | parallel, pbapply | medfit, probmed, medrobust | mediationverse |
| **mediationverse** | Meta | ALL above | - | (end user) |
| **missingmed** | Missing data (MI) | S4, mice, lavaan, OpenMx | RMediation | — *(standalone; not yet wired into meta)* |

> **missingmed** (adopted 2026-06): companion package to the `missing-effect` manuscript (`~/projects/research/Missing Effect/`). Currently **standalone** — not yet a `mediationverse` dependency. Note it is built on **S4**, unlike the S7 standard adopted by `medfit`/`probmed`/`medrobust`; an S4→S7 alignment is a candidate follow-up before wiring it into the meta-package.

---

## Upstream/Downstream Flow

### Data Flow
```
User Data → medfit (fit model)
         → probmed (calculate P_med)
         → RMediation (confidence intervals)
         → medrobust (sensitivity analysis)
         → medsim (simulations)
```

### Code Flow (changes propagate DOWN)
```
S7 (external) ────┬──→ medfit ──→ probmed ──→ mediationverse
                  │           └──→ medsim  ──→ mediationverse
                  │           └──→ RMediation ──→ mediationverse
                  └──→ medrobust ──→ mediationverse
```

---

## Critical Paths

### 🔴 BLOCKER: medfit changes affect EVERYTHING
- Any S7 class changes in medfit → must update probmed, rmediation
- API changes in `extract_*()` → breaks downstream packages
- Version bump in medfit → must update Remotes in probmed, mediationverse

### 🟡 CAUTION: Shared S7 classes
- `mediation_model` class defined in medfit
- Used by: probmed, rmediation, medsim
- Changes require coordinated updates

### 🟢 SAFE: medrobust is independent
- No internal package dependencies (only external: dplyr, ggplot2)
- Can be developed in parallel

---

## Version Compatibility Matrix

| mediationverse | medfit | probmed | RMediation | medrobust | medsim |
|----------------|--------|---------|------------|-----------|--------|
| dev | ≥0.1.0 | ≥0.1.0 | ≥1.4.0 | ≥0.1.0 | ≥0.1.0 |

---

## CRAN Release Order (CRITICAL)

Must release in this order due to dependencies:

1. **medfit** (Q1 2025) ← BLOCKER
2. **RMediation** (Q2 2025) - already on CRAN, update to use medfit
3. **probmed** (Q2 2025) - depends on medfit CRAN release
4. **medrobust** (Q3 2025) - independent, can release anytime
5. **medsim** (Q3 2025) - Suggests ecosystem packages
6. **mediationverse** (Q4 2025) - requires all above on CRAN

---

## Monitoring Checkpoints

Run `medcheck` to verify:
- [ ] All packages install from GitHub
- [ ] Cross-package function calls work
- [ ] S7 class compatibility
- [ ] No circular dependencies
- [ ] All tests pass across ecosystem

---

## Change Protocol

### When modifying medfit:
1. Check which downstream packages use the modified function
2. Run tests in affected packages BEFORE committing
3. Update API-CONTRACTS.md if interface changes
4. Bump version if breaking change
5. Update Remotes in downstream packages

### When adding new feature:
1. Decide which package it belongs to
2. Check if it needs cross-package data
3. Document in API-CONTRACTS.md
4. Add integration tests

### When fixing bug:
1. Identify all packages where bug manifests
2. Fix in lowest-level package (usually medfit)
3. Verify fix propagates correctly
4. Add regression test
