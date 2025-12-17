# Mediationverse Integration Plan

**Created**: 2025-12-03
**Status**: Planning Phase

---

## Executive Summary

This document outlines the plan to integrate the mediationverse packages, removing redundancies and establishing medfit as the foundation package.

---

## Current State Analysis

### Package Exports Overview

| Package | Classes | Key Functions | Unique Contribution |
|---------|---------|---------------|---------------------|
| **medfit** | MediationData, SerialMediationData, BootstrapResult | extract_mediation(), fit_mediation(), bootstrap_mediation() | Foundation infrastructure |
| **probmed** | MediationExtract, PmedResult | extract_mediation(), pmed() | P_med effect size |
| **RMediation** | ProductNormal, MBCOResult | ci(), medci(), mbco(), pprodnormal(), qprodnormal() | CI methods (DOP, MC, MBCO) |
| **medrobust** | (TBD) | (TBD) | Sensitivity analysis |
| **medsim** | (none) | run_simulation(), analyze_results() | Simulation infrastructure |

### Identified Redundancies

#### 1. `extract_mediation()` Generic (CRITICAL)
- **probmed**: Defines own `extract_mediation()` generic and `MediationExtract` class
- **medfit**: Defines `extract_mediation()` generic and `MediationData` class
- **Resolution**: probmed should import and re-export from medfit

#### 2. `MediationExtract` vs `MediationData` Classes
- **probmed**: `MediationExtract` S7 class
- **medfit**: `MediationData` S7 class (more comprehensive)
- **Resolution**: probmed should use medfit's `MediationData`, deprecate `MediationExtract`

#### 3. Bootstrap Infrastructure
- **probmed**: Has internal bootstrap functions
- **medfit**: Has `bootstrap_mediation()` with multiple methods
- **Resolution**: probmed should use medfit's bootstrap infrastructure

---

## Target Architecture

```
User Code
    │
    ▼
┌─────────────────────────────────────────────────────────────┐
│                    mediationverse                            │
│  library(mediationverse)  # Loads all packages              │
└─────────────────────────────────────────────────────────────┘
    │
    ├──────────────────┬──────────────────┬──────────────────┐
    ▼                  ▼                  ▼                  ▼
┌─────────┐      ┌──────────┐      ┌──────────┐      ┌──────────┐
│ probmed │      │RMediation│      │medrobust │      │  medsim  │
│ ─────── │      │ ──────── │      │ ──────── │      │ ──────── │
│ pmed()  │      │ ci()     │      │ bounds() │      │ simulate │
│         │      │ mbco()   │      │ falsify()│      │ analyze  │
└────┬────┘      └────┬─────┘      └────┬─────┘      └────┬─────┘
     │                │                 │                 │
     └────────────────┼─────────────────┼─────────────────┘
                      ▼
            ┌──────────────────┐
            │     medfit       │
            │ ──────────────── │
            │ MediationData    │
            │ extract_mediation│
            │ fit_mediation    │
            │ bootstrap_mediation│
            └──────────────────┘
```

---

## Integration Tasks by Package

### Phase 1: medfit Stabilization (Current)

medfit provides the foundation. Ensure:
- [x] `MediationData` class complete
- [x] `SerialMediationData` class complete
- [x] `BootstrapResult` class complete
- [x] `extract_mediation()` methods for lm/glm, lavaan
- [x] `fit_mediation()` working
- [x] `bootstrap_mediation()` working
- [ ] R CMD check passes on all platforms

### Phase 2: probmed Integration

**Goal**: probmed focuses solely on P_med computation, uses medfit for infrastructure.

#### 2.1 Remove Redundant Code
```r
# REMOVE from probmed:
# - R/extract.R (extract_mediation generic)
# - R/classes.R (MediationExtract class)
# - Internal bootstrap functions
```

#### 2.2 Add medfit Dependency
```r
# In DESCRIPTION:
Imports:
    medfit (>= 0.1.0)

# In NAMESPACE:
importFrom(medfit, MediationData)
importFrom(medfit, extract_mediation)
importFrom(medfit, bootstrap_mediation)
```

#### 2.3 Update pmed() to Accept MediationData
```r
# Current:
pmed(object)  # where object is MediationExtract

# New:
S7::method(pmed, MediationData) <- function(object, ...) {
  # Extract a, b, se_a, se_b, rho from MediationData
  a <- object@a_path
  b <- object@b_path
  # ... compute P_med
}
```

#### 2.4 Deprecate Old Classes
```r
# Add deprecation warning
MediationExtract <- function(...) {
  .Deprecated("medfit::MediationData",
              package = "probmed",
              msg = "MediationExtract is deprecated. Use medfit::MediationData instead.")
  # ... for backward compatibility
}
```

### Phase 3: RMediation Integration

**Goal**: RMediation's `ci()` works with medfit's `MediationData`.

#### 3.1 Add medfit to Suggests
```r
# In DESCRIPTION:
Suggests:
    medfit,
    testthat (>= 3.0.0)
```

#### 3.2 Add ci() Method for MediationData
```r
# In R/ci_medfit.R (new file):
#' @export
S7::method(ci, MediationData) <- function(mu, level = 0.95, type = "dop", ...) {
  # Validate medfit is available
  if (!requireNamespace("medfit", quietly = TRUE)) {
    stop("Package 'medfit' required for this method")
  }

  # Extract parameters for product of two
  a <- mu@a_path
  b <- mu@b_path

  # Get standard errors from vcov
  vcov <- mu@vcov
  se_a <- sqrt(vcov["a", "a"])  # Adjust index names as needed
  se_b <- sqrt(vcov["b", "b"])
  cov_ab <- vcov["a", "b"]

  # Create ProductNormal and compute CI
  rho <- cov_ab / (se_a * se_b)

  # Use existing ci() for ProductNormal
  pn <- ProductNormal(
    mu = c(a, b),
    Sigma = matrix(c(se_a^2, cov_ab, cov_ab, se_b^2), nrow = 2)
  )

  ci(pn, level = level, type = type, ...)
}
```

#### 3.3 Register Method Dynamically
```r
# In R/zzz.R:
.onLoad <- function(libname, pkgname) {
  # Register ci method for MediationData if medfit available
  if (requireNamespace("medfit", quietly = TRUE)) {
    S7::method(ci, medfit::MediationData) <- ci_mediation_data
  }
  S7::methods_register()
}
```

### Phase 4: medrobust Integration

**Goal**: medrobust uses medfit for naive estimates when available.

#### 4.1 Add medfit to Suggests
```r
# In DESCRIPTION:
Suggests:
    medfit
```

#### 4.2 Use MediationData for Input
```r
# Sensitivity functions accept MediationData
sensitivity_bounds <- function(med_data, ...) {
  checkmate::assert_class(med_data, "MediationData")

  # Extract naive estimates
  a <- med_data@a_path
  b <- med_data@b_path
  indirect <- a * b

  # Compute bounds...
}
```

### Phase 5: medsim Integration

**Goal**: medsim uses medfit classes for standardized data generation and analysis.

Already designed to work with ecosystem - no major changes needed.

---

## API Contract: MediationData

All packages should expect these properties from `MediationData`:

```r
MediationData@a_path      # Numeric: X -> M effect
MediationData@b_path      # Numeric: M -> Y effect (controlling X)
MediationData@c_prime     # Numeric: X -> Y direct effect
MediationData@estimates   # Named numeric: all parameter estimates
MediationData@vcov        # Matrix: variance-covariance
MediationData@data        # Data frame or NULL
MediationData@n           # Integer: sample size
MediationData@sigma_m     # Numeric: residual SD of M
MediationData@sigma_y     # Numeric: residual SD of Y
```

For serial mediation, use `SerialMediationData`:
```r
SerialMediationData@a_path      # Numeric: X -> M1
SerialMediationData@d_path      # Numeric vector: M1 -> M2 -> ...
SerialMediationData@b_path      # Numeric: Mk -> Y
SerialMediationData@mediators   # Character vector: mediator names
```

---

## Migration Guide for Users

### Before (Current State)
```r
# Using probmed
library(probmed)
fit_m <- lm(M ~ X, data = d)
fit_y <- lm(Y ~ X + M, data = d)
extract <- extract_mediation(fit_m, fit_y, treatment = "X", mediator = "M")
pmed_result <- pmed(extract)
```

### After (Integrated)
```r
# Using mediationverse
library(mediationverse)

# Step 1: Extract with medfit
fit_m <- lm(M ~ X, data = d)
fit_y <- lm(Y ~ X + M, data = d)
med_data <- extract_mediation(fit_m, model_y = fit_y,
                               treatment = "X", mediator = "M")

# Step 2: Choose your analysis
# P_med effect size (probmed)
pmed_result <- pmed(med_data)

# Confidence intervals (RMediation)
ci_result <- ci(med_data, type = "dop")

# Sensitivity analysis (medrobust)
sens_result <- sensitivity_bounds(med_data)

# Bootstrap inference (medfit)
boot_result <- bootstrap_mediation(
  statistic_fn = function(d) {
    md <- extract_mediation(...)
    md@a_path * md@b_path
  },
  data = d, n_boot = 2000
)
```

---

## Implementation Timeline

### Week 1-2: Foundation
- [ ] Ensure medfit R CMD check passes (0/0/0)
- [ ] Document MediationData API contract
- [ ] Create test fixtures for integration testing

### Week 3-4: probmed Integration
- [ ] Add medfit to Imports
- [ ] Migrate pmed() to accept MediationData
- [ ] Add deprecation warnings for MediationExtract
- [ ] Update tests
- [ ] Update documentation

### Week 5-6: RMediation Integration
- [ ] Add medfit to Suggests
- [ ] Add ci() method for MediationData
- [ ] Test with lavaan and lm/glm models
- [ ] Update documentation

### Week 7-8: Testing & Polish
- [ ] Cross-package integration tests
- [ ] Update all vignettes
- [ ] Ensure mediationverse loads all correctly
- [ ] Performance testing

---

## Backward Compatibility

### probmed
- `MediationExtract` deprecated but still works (with warning)
- Old `extract_mediation()` re-exports medfit version
- `pmed()` accepts both old and new classes

### RMediation
- No breaking changes (new method added)
- Existing code continues to work

### medrobust
- No breaking changes (new integration added)

---

## Testing Strategy

### Unit Tests (per package)
- Each package tests its unique functionality
- medfit: extraction, fitting, bootstrap
- probmed: P_med computation
- RMediation: CI methods, MBCO
- medrobust: bounds, falsification

### Integration Tests (mediationverse)
```r
test_that("full workflow works", {
  library(mediationverse)

  # Simulate data
  d <- medsim::simulate_mediation(n = 200, a = 0.5, b = 0.4)

  # Extract
  med_data <- extract_mediation(lm(M ~ X, d),
                                 model_y = lm(Y ~ X + M, d),
                                 treatment = "X", mediator = "M")

  # All packages work with same object
  expect_s7_class(med_data, "MediationData")
  expect_no_error(pmed(med_data))
  expect_no_error(ci(med_data))
})
```

---

## Open Questions

1. **Version requirements**: What minimum medfit version should others require?
2. **CRAN coordination**: How to coordinate CRAN submissions?
3. **SerialMediationData**: Should RMediation support product-of-three?
4. **OpenMx support**: Keep in RMediation, add to medfit, or both?

---

## Next Steps

1. Review and approve this plan
2. Create GitHub issues for each task
3. Begin Phase 1 (medfit stabilization)
4. Proceed sequentially through phases

---

**Document Owner**: Davood Tofighi
**Last Updated**: 2025-12-03
