# MediationVerse Ecosystem Review Summary

**Date:** December 12, 2025  
**Purpose:** Strategic planning for unified generic function design

---

## Current State Overview

### Package Functions by Category

| Category | medfit | probmed | RMediation | medrobust | medsim |
|----------|--------|---------|------------|-----------|--------|
| **Extraction** | `extract_mediation()` | re-exports medfit | `ci_mediation_data()` | - | - |
| **Fitting** | `fit_mediation()` ⚠️ | - | - | - | - |
| **Bootstrap** | `bootstrap_mediation()` ⚠️ | internal | - | `bootstrap_results()` | - |
| **CI Computation** | - | - | `ci()`, `medci()` | - | - |
| **Effect Size** | - | `pmed()` | - | `bound_ne()` | - |
| **Sensitivity** | - | - | - | `sensitivity_region()` | - |
| **Simulation** | - | - | - | - | `medsim_run()` |
| **Visualization** | - | - | - | `sensitivity_plot()` | `medsim_plot_*()` |

⚠️ = Not yet implemented (stub only)

---

### S7 Classes Across Ecosystem

| Package | Classes | Purpose |
|---------|---------|---------|
| **medfit** | `MediationData`, `SerialMediationData`, `BootstrapResult` | Core data structures |
| **probmed** | `PmedResult` | P_med effect size results |
| **RMediation** | `ProductNormal`, `MBCOResult` | Distribution + hypothesis test |
| **medrobust** | `medrobust_bounds`, `sensitivity_region`, `power_analysis_result`, `falsification_summary` | Sensitivity analysis |
| **medsim** | S3 classes only (`medsim_*`) | Simulation infrastructure |

---

### Current Generic Functions

**medfit (foundation):**
- `extract_mediation(object, ...)` — S7 generic, dispatches on model type
- `fit_mediation()` — regular function (not generic)
- `bootstrap_mediation()` — regular function (not generic)

**probmed:**
- `pmed(object, ...)` — S7 generic, dispatches on formula vs MediationData

**RMediation:**
- `ci(mu, ...)` — S7 generic for confidence intervals
- `cdf(object, ...)` — S7 generic for CDF
- `dist_quantile(object, ...)` — S7 generic for quantiles

**medrobust:**
- No S7 generics (all regular functions)

**medsim:**
- No S7 generics (all regular functions)

---

## Gaps & Opportunities Identified

### 1. **Inconsistent Generic Design**
- medfit uses S7 generic for `extract_mediation()` but regular functions for `fit_mediation()` and `bootstrap_mediation()`
- medrobust has similar functionality (`bootstrap_results()`) but no shared interface

### 2. **Missing Common Operations**
No unified generics for:
- `summarize()` or `summary()` — each package implements S3 methods separately
- `plot()` — scattered implementations
- `print()` — each package does its own S3 methods
- `coef()` — could extract path coefficients consistently
- `vcov()` — extract variance-covariance matrix
- `confint()` — compute confidence intervals

### 3. **Parallel Bootstrap Implementations**
- medfit plans `bootstrap_mediation()`
- medrobust has `bootstrap_results()`
- probmed uses internal bootstrap
- RMediation uses Monte Carlo simulation (similar concept)

### 4. **Effect Size Extraction**
No generic way to:
- Get indirect effect: `indirect_effect(object)`
- Get direct effect: `direct_effect(object)`
- Get total effect: `total_effect(object)`

### 5. **Sensitivity Analysis Interface**
- medrobust: `bound_ne()`, `sensitivity_region()`
- RMediation: `mbco()` (different purpose but related)
- No unified `sensitivity()` generic

---

## Proposed Unified Generics Strategy

### Tier 1: Core Generics (medfit defines)

```r
# Model extraction (already exists)
extract_mediation(object, ...) → MediationData

# Effect extraction (NEW)
indirect_effect(object, ...) → numeric + CI
direct_effect(object, ...) → numeric + CI
total_effect(object, ...) → numeric + CI

# Bootstrap inference (NEW as generic)
bootstrap(object, statistic, ...) → BootstrapResult

# Confidence intervals (NEW, unified)
mediation_ci(object, ...) → list(estimate, ci_lower, ci_upper)
```

### Tier 2: Enhanced Base Generics (S7 methods)

```r
# Standard R generics with S7 methods
coef.MediationData() → named vector of path coefficients
vcov.MediationData() → variance-covariance matrix
confint.MediationData() → CI for all paths
summary.MediationData() → formatted summary
print.MediationData() → compact display
plot.MediationData() → path diagram
```

### Tier 3: Domain-Specific Generics

```r
# probmed
pmed(object, ...) → P_med effect size (already exists)

# RMediation  
ci(mu, ...) → CI via product-of-normals (already exists)

# medrobust
sensitivity(object, ...) → sensitivity analysis results (NEW)
power_analysis(object, ...) → power analysis (partially exists)

# medsim
simulate(object, ...) → simulation results (use stats::simulate)
```

---

## Implementation Recommendations

### Option A: Minimal (Low Effort)
- Keep current design
- Add S7 methods for base generics (`coef`, `vcov`, `confint`)
- Document cross-package patterns in vignette

### Option B: Moderate (Medium Effort) ⭐ RECOMMENDED
- Create unified effect extraction generics in medfit
- Standardize bootstrap interface
- Add base generic methods to all S7 classes
- Create `mediation_ci()` generic that packages extend

### Option C: Comprehensive (High Effort)
- Full refactor with shared generic definitions
- New `mediationverse-core` package with all generics
- All packages depend on core
- Most flexible but highest coordination cost

---

## Questions for Discussion

1. **Should `bootstrap_mediation()` be a generic?**
   - Pro: Different packages could extend
   - Con: Complex dispatch, statistic function varies

2. **Should effect extraction (`indirect_effect()`, etc.) be generics?**
   - Pro: Unified interface across ecosystem
   - Con: Implementation varies significantly

3. **Where should shared generics live?**
   - medfit (current foundation)
   - New mediationverse-core package
   - Each package defines its own + re-exports

4. **S7 vs S3 for base generic methods?**
   - S7: More powerful, better type safety
   - S3: Simpler, better CRAN compatibility currently

5. **How to handle CI computation uniformly?**
   - RMediation: Analytical (product-of-normals)
   - medfit: Bootstrap
   - probmed: Bootstrap
   - Need unified interface or accept diversity?
