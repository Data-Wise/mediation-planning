# API Contracts for Mediation Ecosystem

**Last Updated**: December 3, 2025
**Maintainers**: medfit, probmed, RMediation, medrobust teams

## Purpose

This document defines the shared interfaces and contracts between packages in the mediation analysis ecosystem. It ensures compatibility and reduces breaking changes across package boundaries.

---

## Package Dependency Graph

```
mediationverse (meta-package)
├── medfit (foundation)
├── probmed (depends on medfit)
├── RMediation (suggests medfit)
└── medrobust (suggests medfit)
```

---

## Core S7 Classes (medfit)

All packages in the ecosystem use these standardized data structures.

### MediationData

**Package**: `medfit`
**Purpose**: Simple mediation (X → M → Y)
**Stability**: Stable (v0.1.0+)

**Properties Contract**:

| Property | Type | Description | Required |
|----------|------|-------------|----------|
| `a_path` | numeric scalar | X → M effect | Yes |
| `b_path` | numeric scalar | M → Y effect | Yes |
| `c_prime` | numeric scalar | X → Y direct effect | Yes |
| `estimates` | numeric vector | Full parameter vector | Yes |
| `vcov` | numeric matrix | Variance-covariance matrix | Yes |
| `treatment` | character scalar | Treatment variable name | Yes |
| `mediator` | character scalar | Mediator variable name | Yes |
| `outcome` | character scalar | Outcome variable name | Yes |
| `data` | data.frame or NULL | Original data | Optional |
| `n` | integer scalar | Sample size | Yes |
| `sigma_m` | numeric scalar | Mediator residual SD | Optional |
| `sigma_y` | numeric scalar | Outcome residual SD | Optional |
| `converged` | logical scalar | Convergence flag | Yes |

**Compatibility Promise**:
- Adding new properties is non-breaking (existing code continues to work)
- Removing or renaming properties requires major version bump
- Type changes require major version bump

**Usage Examples**:
```r
# probmed: Extract for P_med computation
med_data <- medfit::extract_mediation(fit_m, model_y = fit_y,
                                       treatment = "X", mediator = "M")
p_med <- probmed::compute_pmed(med_data)

# RMediation: Extract for DOP confidence intervals
med_data <- medfit::extract_mediation(fit)
ci <- RMediation::medci(med_data@a_path, med_data@b_path,
                        vcov = med_data@vcov)
```

---

### SerialMediationData

**Package**: `medfit`
**Purpose**: Serial mediation (X → M1 → M2 → ... → Y)
**Stability**: Experimental (v0.1.0), Stable (v0.2.0+)

**Properties Contract**:

| Property | Type | Description | Required |
|----------|------|-------------|----------|
| `a_path` | numeric scalar | X → M1 effect | Yes |
| `d_path` | numeric scalar or vector | M1 → M2 (or M_i → M_{i+1}) effects | Yes |
| `b_path` | numeric scalar | M_final → Y effect | Yes |
| `c_prime` | numeric scalar | X → Y direct effect | Yes |
| `estimates` | numeric vector | Full parameter vector | Yes |
| `vcov` | numeric matrix | Variance-covariance matrix | Yes |
| `treatment` | character scalar | Treatment variable name | Yes |
| `mediators` | character vector | Mediator variable names (ordered) | Yes |
| `outcome` | character scalar | Outcome variable name | Yes |
| `data` | data.frame or NULL | Original data | Optional |
| `n` | integer scalar | Sample size | Yes |
| `sigma_mediators` | numeric vector | Mediator residual SDs | Optional |
| `sigma_y` | numeric scalar | Outcome residual SD | Optional |
| `mediator_predictors` | list | Predictors for each mediator model | Optional |
| `converged` | logical scalar | Convergence flag | Yes |

**Compatibility Promise**: Same as MediationData

**Usage Examples**:
```r
# probmed: Product-of-three indirect effect
serial_data <- medfit::extract_mediation(fit, type = "serial")
indirect <- serial_data@a_path * serial_data@d_path * serial_data@b_path
```

---

### BootstrapResult

**Package**: `medfit`
**Purpose**: Bootstrap inference results
**Stability**: Stable (v0.1.0+)

**Properties Contract**:

| Property | Type | Description | Required |
|----------|------|-------------|----------|
| `estimate` | numeric scalar | Point estimate | Yes |
| `ci_lower` | numeric scalar | Lower CI bound | Yes |
| `ci_upper` | numeric scalar | Upper CI bound | Yes |
| `boot_estimates` | numeric vector | Bootstrap replicates | Yes |
| `method` | character scalar | "parametric", "nonparametric", "plugin" | Yes |
| `ci_level` | numeric scalar | Confidence level (e.g., 0.95) | Yes |
| `n_boot` | integer scalar | Number of bootstrap samples | Yes |

**Compatibility Promise**: Same as MediationData

---

## Core Functions (medfit)

### extract_mediation()

**Signature**:
```r
extract_mediation(object, ...)

# lm/glm method
extract_mediation(
  object,                    # lm/glm model for mediator
  model_y = NULL,           # lm/glm model for outcome
  treatment,                # character: treatment variable name
  mediator,                 # character or character vector: mediator(s)
  outcome = NULL,           # character: outcome variable name (optional)
  ...
)

# lavaan method
extract_mediation(
  object,                    # lavaan fit object
  treatment,                # character: treatment variable name
  mediator,                 # character or character vector: mediator(s)
  outcome,                  # character: outcome variable name
  ...
)
```

**Return Type**:
- `MediationData` for simple mediation (single mediator)
- `SerialMediationData` for serial mediation (multiple mediators)

**Stability**: Stable (v0.1.0+)

**Contract Guarantees**:
1. Always returns S7 object (MediationData or SerialMediationData)
2. `@converged` property reliably indicates model convergence
3. `@vcov` matches dimension of `@estimates`
4. Variable names in properties match data column names

**Breaking Change Policy**:
- Adding new methods: non-breaking (minor version bump)
- Changing return type structure: breaking (major version bump)
- Adding optional arguments: non-breaking
- Changing required arguments: breaking

**Cross-Package Usage**:
```r
# probmed wraps this for user convenience
probmed::fit_pmed(...) {
  med_data <- medfit::extract_mediation(...)
  # ... P_med computation
}

# RMediation uses directly
med_data <- medfit::extract_mediation(fit)
RMediation::medci(med_data@a_path, med_data@b_path)
```

---

### bootstrap_mediation()

**Signature**:
```r
bootstrap_mediation(
  object,                    # MediationData or SerialMediationData
  statistic,                # function(med_data) -> numeric
  method = "parametric",    # "parametric", "nonparametric", "plugin"
  n_boot = 5000,           # integer: number of bootstrap samples
  ci_level = 0.95,         # numeric: confidence level
  parallel = FALSE,        # logical: use parallel processing
  seed = NULL,             # integer: random seed for reproducibility
  ...
)
```

**Return Type**: `BootstrapResult`

**Stability**: Stable (v0.1.0+)

**Contract Guarantees**:
1. `statistic` function receives same class as `object` input
2. Seed ensures reproducibility across parallel/sequential
3. CI coverage matches `ci_level` in long-run (asymptotically)
4. All three methods return identical structure (BootstrapResult)

**Cross-Package Usage**:
```r
# probmed: Bootstrap P_med
med_data <- medfit::extract_mediation(...)
boot_result <- medfit::bootstrap_mediation(
  med_data,
  statistic = function(x) probmed::compute_pmed(x),
  n_boot = 5000
)

# medrobust: Bootstrap sensitivity bounds
boot_bounds <- medfit::bootstrap_mediation(
  med_data,
  statistic = function(x) medrobust::compute_bounds(x, rho = 0.3),
  method = "nonparametric"
)
```

---

## Cross-Package Interfaces

### probmed → medfit

**Dependency Type**: Imports (hard dependency)

**Interface Points**:

1. **Model Extraction**:
   ```r
   # probmed wraps medfit extraction
   probmed::fit_pmed <- function(formula_m, formula_y, data, ...) {
     # Fit models internally
     fit_m <- stats::glm(formula_m, data = data, ...)
     fit_y <- stats::glm(formula_y, data = data, ...)

     # Extract via medfit
     med_data <- medfit::extract_mediation(
       fit_m, model_y = fit_y,
       treatment = ..., mediator = ...
     )

     # Compute P_med
     result <- compute_pmed_internal(med_data)
     return(result)
   }
   ```

2. **Bootstrap Inference**:
   ```r
   # probmed uses medfit bootstrap for CI
   boot_result <- medfit::bootstrap_mediation(
     med_data,
     statistic = function(x) probmed:::compute_pmed_internal(x),
     n_boot = n_boot,
     ci_level = ci_level
   )
   ```

**Compatibility Requirements**:
- probmed requires medfit >= 0.1.0
- probmed tests must pass with medfit development version
- Breaking changes in medfit require probmed update

---

### RMediation → medfit

**Dependency Type**: Suggests (optional dependency)

**Interface Points**:

1. **Optional Integration**:
   ```r
   # RMediation checks for medfit availability
   medci_from_fit <- function(fit, ...) {
     if (!requireNamespace("medfit", quietly = TRUE)) {
       stop("Package 'medfit' required for this functionality")
     }

     med_data <- medfit::extract_mediation(fit, ...)

     # Use RMediation's core methods
     medci(med_data@a_path, med_data@b_path,
           type = "dop", ...)
   }
   ```

**Compatibility Requirements**:
- RMediation remains functional without medfit
- When medfit available, integration is seamless
- RMediation tests run both with and without medfit

---

### medrobust → medfit

**Dependency Type**: Suggests (optional dependency)

**Interface Points**:

1. **Naive Estimates**:
   ```r
   # medrobust optionally uses medfit for naive estimates
   sensitivity_analysis <- function(fit, rho, ...) {
     naive <- if (requireNamespace("medfit", quietly = TRUE)) {
       med_data <- medfit::extract_mediation(fit, ...)
       med_data@a_path * med_data@b_path
     } else {
       # Fallback extraction
       extract_naive_internal(fit)
     }

     # Compute bounds
     bounds <- compute_bounds_internal(naive, rho, ...)
     return(bounds)
   }
   ```

**Compatibility Requirements**:
- medrobust works without medfit (uses internal extraction)
- medfit integration is convenience feature only
- No version lock (uses latest available)

---

## Version Compatibility Matrix

| medfit | probmed | RMediation | medrobust | mediationverse |
|--------|---------|------------|-----------|----------------|
| 0.1.0  | 0.2.0   | 1.5.0      | 0.2.0     | 0.1.0          |
| 0.2.0  | 0.3.0   | 1.5.0+     | 0.2.0+    | 0.2.0          |

**Update Policy**:
- medfit minor version bump → probmed should update within 1 month
- medfit major version bump → probmed must update before medfit release
- Suggested dependencies (RMediation, medrobust) have looser coupling

---

## Testing Contracts

### Unit Test Requirements

Each package must test its integration with medfit:

**probmed**:
```r
test_that("probmed works with medfit extraction", {
  # Create test data
  fit_m <- lm(M ~ X, data = test_data)
  fit_y <- lm(Y ~ X + M, data = test_data)

  # Extract via medfit
  med_data <- medfit::extract_mediation(
    fit_m, model_y = fit_y,
    treatment = "X", mediator = "M"
  )

  # Compute P_med
  p_med <- probmed::compute_pmed(med_data)

  # Verify
  expect_s3_class(p_med, "pmed_result")
  expect_true(p_med$estimate >= 0 && p_med$estimate <= 1)
})
```

**RMediation**:
```r
test_that("RMediation integrates with medfit when available", {
  skip_if_not_installed("medfit")

  fit <- lm(...)
  med_data <- medfit::extract_mediation(fit, ...)

  # Should work seamlessly
  ci <- RMediation::medci(med_data@a_path, med_data@b_path)
  expect_type(ci, "list")
})
```

### Cross-Package Integration Tests

**Location**: `/Users/dt/mediation-planning/integration-tests/`

**Run Frequency**: Before any package release

**Coverage**:
- medfit extraction → probmed P_med computation
- medfit extraction → RMediation DOP CI
- medfit bootstrap → medrobust bounds CI

---

## Deprecation Policy

### Deprecation Timeline

1. **Soft Deprecation** (1 release cycle):
   - Add `.Deprecated()` warning
   - Document replacement in help
   - Update examples to use new interface

2. **Hard Deprecation** (2 release cycles):
   - Add `.Defunct()` error
   - Remove from documentation
   - Keep implementation for error message

3. **Removal** (3+ release cycles):
   - Remove code entirely
   - Update dependent packages first

### Example Deprecation

```r
# Old function (deprecated in v0.2.0)
extract_paths <- function(...) {
  .Deprecated("extract_mediation",
              msg = "extract_paths() is deprecated. Use extract_mediation() instead.")
  extract_mediation(...)
}

# In v0.3.0
extract_paths <- function(...) {
  .Defunct("extract_mediation",
           msg = "extract_paths() is defunct. Use extract_mediation().")
}

# In v0.4.0 - function removed entirely
```

---

## Communication Protocols

### Breaking Changes

When a package plans breaking changes:

1. **Announce** in GitHub issue (tag dependent package maintainers)
2. **Document** in NEWS.md under "Breaking Changes" section
3. **Coordinate** release timeline (2-week minimum notice)
4. **Test** with dependent package development versions
5. **Release** with major version bump

### New Features

When adding new features to shared interfaces:

1. **Propose** in GitHub discussion
2. **Prototype** in feature branch
3. **Test** with dependent packages (optional features)
4. **Document** in API-CONTRACTS.md before release
5. **Release** with minor version bump

### Bug Fixes

Bug fixes are non-breaking by definition:

1. **Report** in GitHub issue
2. **Fix** in patch branch
3. **Test** with existing dependent package releases
4. **Release** with patch version bump
5. **Notify** dependent packages (optional, for critical fixes)

---

## Claude Code Integration

### Multi-Package Awareness

When working across packages, Claude Code should:

1. **Check API-CONTRACTS.md** before making interface changes
2. **Update VERSION-MATRIX.md** when bumping versions
3. **Run integration tests** from `/Users/dt/mediation-planning/integration-tests/`
4. **Consult ECOSYSTEM.md** for dependency graph
5. **Update multiple package CLAUDE.md files** when architecture changes

### Worktree Setup for Development

```bash
# Set up git worktrees for simultaneous multi-package work
cd ~/packages/medfit
git worktree add ../medfit-feature feature-branch

cd ~/packages/probmed
git worktree add ../probmed-feature feature-branch

# Work across both simultaneously
# Claude Code can read from both worktrees in single context
```

### Cross-Package Testing Workflow

```r
# In medfit worktree
devtools::load_all("~/packages/medfit-feature")

# In probmed worktree (uses dev medfit)
devtools::load_all("~/packages/probmed-feature")

# Test integration
med_data <- medfit::extract_mediation(...)
p_med <- probmed::compute_pmed(med_data)
```

---

## Appendix: Function Signature Quick Reference

### medfit

```r
# Extraction
extract_mediation(object, ...)

# Bootstrap
bootstrap_mediation(object, statistic, method = "parametric",
                    n_boot = 5000, ci_level = 0.95, ...)

# Fitting (planned)
fit_mediation(formula_m, formula_y, data, engine = "glm", ...)
```

### probmed

```r
# Main interface
fit_pmed(formula_m, formula_y, data, n_boot = 5000, ...)

# Direct computation
compute_pmed(med_data)  # med_data is MediationData
```

### RMediation

```r
# Core methods (unchanged)
medci(mu.x, mu.y, type = "dop", ...)

# medfit integration (new)
medci_from_fit(fit, ...) # extracts via medfit internally
```

### medrobust

```r
# Main interface
sensitivity_analysis(fit, rho, method = "bounds", ...)

# Bounds computation
compute_bounds(indirect_effect, rho, ...)
```

---

**Document Status**: Living document - update before any interface change
**Review Cycle**: Before each package release
**Maintainers**: Coordinate via GitHub issues in medfit repository
