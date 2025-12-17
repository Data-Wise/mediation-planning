# Mediationverse TODO List

**Last Updated:** 2025-12-17

Active tasks and pending items across the mediationverse ecosystem.

---

## Pending Decisions

### API Design Decisions (7 items)
Location: `specs/GENERIC-FUNCTIONS-STRATEGY.md`

| # | Decision | Recommendation | Status |
|---|----------|----------------|--------|
| 0 | ADHD-Friendly API Pattern | Hybrid (Option C) | PENDING |
| 1 | Rename `extract_mediation()`? | Keep both (alias) | PENDING |
| 2 | Effect extraction approach | Separate: `nie()`, `nde()` | PENDING |
| 3 | `boot()` naming conflict | Keep `bootstrap_mediation()` + alias | PENDING |
| 4 | Which aliases to provide | Short + long only | PENDING |
| 5 | `confint()` vs `ci()` | Keep both | PENDING |
| 6 | medrobust renames | Add aliases, keep originals | PENDING |

**Action:** Review and approve decisions to proceed with implementation.

---

## Immediate TODOs (This Week)

### medfit
- [ ] Complete extended testing (edge cases, integration)
- [ ] Merge dev → main to deploy pkgdown site
- [ ] Implement `med()` entry point (after Decision 0 approved)
- [ ] Implement `quick()` instant results function

### Coordination Hub
- [ ] Create `medstatus` aggregator script
- [ ] Create `/ecosystem` slash command
- [ ] Update PROJECT-HUB.md with unified progress view

---

## Short-term TODOs (This Month)

### medfit
- [ ] Add lmer engine for mixed-effects models
- [ ] Add brms engine for Bayesian inference
- [ ] Implement BCa bootstrap confidence intervals
- [ ] Add `nie()`, `nde()`, `te()`, `pm()` extractors

### probmed
- [ ] Add more distribution support
- [ ] Expand vignettes with examples
- [ ] Integration tests with medfit

### medrobust
- [ ] Add short aliases (`sens()`, `bounds()`, `power()`)
- [ ] Integration with medfit workflow

### Documentation
- [ ] Update API-CONTRACTS.md after decisions approved
- [ ] Create ecosystem-wide getting started guide

---

## Medium-term TODOs (Q1 2025)

### Governance
- [ ] Establish RFC system for cross-package changes
- [ ] Create RFC template and process documentation
- [ ] Convert GENERIC-FUNCTIONS-STRATEGY to RFC format

### Tooling
- [ ] Create `mednow` suggestion command
- [ ] Implement git hook propagation for status sync
- [ ] Daily focus card generator

### Packages
- [ ] CRAN submission prep for probmed
- [ ] medsim integration tests for all packages
- [ ] mediationverse unified vignette

---

## Package-Specific TODOs

### medfit (95% complete)
```
Priority: P0 (Foundation - blocks everything)
Status: Active
```
- [x] S7 class architecture
- [x] fit_mediation() with GLM engine
- [x] bootstrap_mediation() (3 methods)
- [x] 93 tests passing
- [ ] Extended testing
- [ ] lmer engine
- [ ] brms engine
- [ ] `med()` entry point

### probmed (55% complete)
```
Priority: P1
Status: Active
```
- [x] S7 class system
- [x] P_med computation
- [x] Bootstrap CIs
- [x] medfit integration
- [ ] More distribution support
- [ ] Expanded vignettes

### RMediation (100% complete)
```
Priority: Stable
Status: CRAN v1.4.0
```
- [x] S7 migration complete
- [x] 201 tests passing
- [ ] medfit integration (v1.5.0)

### medrobust (90% complete)
```
Priority: P2
Status: Active
```
- [x] bound_ne() implementation
- [x] sensitivity_plot()
- [x] power_analysis()
- [x] S7 class system
- [ ] Short aliases
- [ ] medfit integration

### medsim (85% complete)
```
Priority: P2
Status: Experimental
```
- [x] Core simulation infrastructure
- [x] Parallel processing
- [x] Caching system
- [ ] Standard scenarios for all packages
- [ ] Publication-ready tables

### mediationverse (70% complete)
```
Priority: P3
Status: Active
```
- [x] Meta-package structure
- [x] Selective loading
- [x] Coordination functions
- [ ] Unified vignette
- [ ] Awaiting all packages stable

---

## Blocked Items

| Item | Blocked By | Notes |
|------|------------|-------|
| probmed CRAN prep | medfit stability | Wait for medfit extended testing |
| mediationverse release | All packages | Meta-package waits for all |
| `med()` implementation | Decision 0 approval | Need API design approval |

---

## Completed Recently

### 2025-12-17
- [x] Reorganized mediation-planning into docs/, specs/, proposals/, tooling/
- [x] Created CHANGELOG.md
- [x] Created CLAUDE.md
- [x] Added ADHD-Friendly API Design options
- [x] Created ECOSYSTEM-PACKAGES.md
- [x] Created COORDINATION-HUB-BRAINSTORM.md

### 2025-12-16
- [x] medfit: fit_mediation() implementation
- [x] medfit: bootstrap_mediation() implementation
- [x] medfit: CI workflow fixes

---

## How to Update This File

1. Mark items complete with `[x]`
2. Add new items under appropriate section
3. Update "Last Updated" date
4. Commit with message: `docs: update TODOS.md`

---

*See also: `PROJECT-HUB.md` for dashboard view, `.STATUS` for quick status.*
