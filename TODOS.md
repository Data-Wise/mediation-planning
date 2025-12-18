# Mediationverse TODO List

**Last Updated:** 2025-12-17

Active tasks and pending items across the mediationverse ecosystem.

---

## Ready to Implement (No Blocking Decisions)

### ✅ COMPLETED: medfit Phase 1-7 (2025-12-17) 🎉

| Phase | Task | Status |
|-------|------|--------|
| **Phase 1-3** | Package setup, S7 architecture, extraction | ✅ Complete |
| **Phase 4** | fit_mediation() with GLM engine | ✅ Complete |
| **Phase 5** | bootstrap_mediation() (3 methods) | ✅ Complete |
| **Phase 6** | Generic functions (coef, vcov, confint, nobs, nie, nde, te, pm, paths) | ✅ Complete |
| **Phase 6.5** | ADHD API (med, quick) + Tidyverse (tidy, glance) | ✅ Complete |
| **Phase 7** | Polish & CRAN prep (R CMD check, spell check) | ✅ Complete (99%) |

**Stats:** 427 tests passing, R CMD check clean
**PR:** https://github.com/Data-Wise/medfit/pull/10 (merged)
**Next:** Final dev → main merge, tag v0.1.0

---

### READY NOW: Phase 2 Integration (3 months ahead of schedule!)

| Priority | Task | Effort | Notes |
|----------|------|--------|-------|
| **P0** | Re-export medfit generics in probmed | 30 min | `nie()`, `nde()` etc. |
| **P1** | Add `pmed()` extractor in probmed | 30 min | For P_med effect size |
| **P2** | Integration tests probmed + medfit | 1 hr | Workflow tests |

### Enhancement: Delta Method SEs for Indirect Effects

| Task | Effort | Notes |
|------|--------|-------|
| Add delta method SE for `nie` in `tidy()` | 1 hr | Currently shows NA |
| Add `se_nie()` extractor function | 30 min | Optional convenience |
| Update documentation with formula | 30 min | Sobel test formula |

**Formula:** SE(ab) = sqrt(a²σ²_b + b²σ²_a)

---

## Pending Decisions (Can Implement While Deciding)

### API Design Decisions (7 items)
Location: `specs/GENERIC-FUNCTIONS-STRATEGY.md`

| # | Decision | Recommendation | Can Implement? |
|---|----------|----------------|----------------|
| 0 | ADHD-Friendly API Pattern | Hybrid (Option C) | Yes - start with `med()` |
| 1 | Rename `extract_mediation()`? | Keep both (alias) | Yes - add alias later |
| 2 | Effect extraction approach | Separate: `nie()`, `nde()` | Yes - implement separate |
| 3 | `boot()` naming conflict | Keep `bootstrap_mediation()` | Already done |
| 4 | Which aliases to provide | Short + long only | Yes - add after core |
| 5 | `confint()` vs `ci()` | Keep both | Yes - implement both |
| 6 | medrobust renames | Add aliases, keep originals | Later (medrobust) |

**Note:** All recommendations are safe to implement. Decisions mostly affect aliases/naming.

---

## Immediate TODOs (This Week)

### medfit (100% Complete! 🎉)
- [x] Add `coef()` method for MediationData
- [x] Add `vcov()` method for MediationData
- [x] Add `confint()` method for MediationData
- [x] Add `nobs()` method for MediationData
- [x] Add `nie()`, `nde()`, `te()`, `pm()` generics + methods
- [x] Add `paths()` generic + method
- [x] Add `med()` and `quick()` ADHD entry points
- [x] Add `tidy()` and `glance()` tidyverse methods
- [x] Update pkgdown reference
- [x] R CMD check clean
- [x] Spell check (inst/WORDLIST)
- [x] Simplify ecosystem docs
- [ ] Final merge dev → main
- [ ] Tag v0.1.0

### probmed Integration
- [ ] Re-export medfit generics (`nie`, `nde`, `te`, `pm`, `paths`)
- [ ] Add `pmed()` extractor for P_med
- [ ] Test workflow with medfit objects

### Coordination Hub
- [x] ✅ Establish mediation-planning as coordination hub
- [x] ✅ Create master ROADMAP.md
- [x] ✅ Update PROJECT-HUB.md with current status
- [x] ✅ Update mediationverse roadmap to reference mediation-planning
- [ ] Create `medstatus` aggregator script
- [ ] Create `/ecosystem` slash command

---

## Short-term TODOs (This Month)

### medfit
- [ ] Add delta method SE for indirect effect in `tidy()`
- [ ] Add lmer engine for mixed-effects models
- [ ] Add brms engine for Bayesian inference
- [ ] Implement BCa bootstrap confidence intervals

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

### medfit (100% complete! 🎉)
```
Priority: P0 (Foundation - COMPLETE!)
Status: Ready for v0.1.0 release
```
- [x] S7 class architecture
- [x] fit_mediation() with GLM engine
- [x] bootstrap_mediation() (3 methods)
- [x] 427 tests passing (100% pass rate)
- [x] Generic functions (`coef`, `vcov`, `confint`, `nobs`)
- [x] Effect extractors (`nie`, `nde`, `te`, `pm`, `paths`)
- [x] ADHD entry points (`med()`, `quick()`)
- [x] Tidyverse methods (`tidy()`, `glance()`)
- [x] R CMD check clean
- [x] Phase 7 Polish complete
- [ ] Final dev → main merge
- [ ] Tag v0.1.0

**Future Enhancements:**
- [ ] Delta method SE for indirect effects
- [ ] lmer engine
- [ ] brms engine

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

---

## Completed Recently

### 2025-12-17 (Integration Update)
- [x] **Established mediation-planning as ecosystem coordination hub**
  - Created master ROADMAP.md (extracted from mediationverse, updated)
  - Updated PROJECT-HUB.md to 90% progress
  - Updated CHANGELOG.md with integration details
  - Updated TODOS.md to reflect medfit completion
  - Clear separation: mediation-planning (planning) vs mediationverse (meta-package)
- [x] **Updated mediationverse to reference coordination hub**
  - Updated vignettes/roadmap.Rmd with mediation-planning links
  - Timeline adjusted: 3 months ahead of schedule!
  - Phase 2 Integration ready NOW (was Q1 2026)
- [x] **medfit: Phase 7 Polish complete** (99%)
  - R CMD check clean (1 NOTE expected)
  - Spell check with inst/WORDLIST
  - DESCRIPTION updates (Language, Date)
  - Simplified ecosystem docs (only RMediation, mediationverse)

### 2025-12-17 (Earlier)
- [x] **medfit: Generic functions complete** (427 tests, PR #10 merged)
  - Base R: `coef()`, `vcov()`, `confint()`, `nobs()`
  - Effects: `nie()`, `nde()`, `te()`, `pm()`, `paths()`
  - ADHD: `med()`, `quick()`
  - Tidyverse: `tidy()`, `glance()`
- [x] Updated pkgdown reference sections
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
