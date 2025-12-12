# MediationVerse Generic Function Strategy

**Status:** Draft for Review  
**Created:** December 11, 2024  
**Location:** ~/projects/research/mediation-planning/GENERIC-FUNCTIONS-STRATEGY.md

---

## Executive Summary

This document proposes a unified generic function strategy for the mediationverse R package ecosystem. The goal is to create a consistent, memorable API that leverages both base R generics and domain-specific functions while maintaining compatibility with tidyverse conventions.

**Key Principles:**
1. Short, memorable function names (2-4 characters for domain-specific)
2. Argument-modified behavior over function proliferation
3. Base R generic compatibility (`coef`, `vcov`, `confint`, `summary`)
4. Tidyverse integration (`tidy`, `glance`, `augment`)
5. Aliases for discoverability

---

## Table of Contents

1. [Current State Analysis](#current-state-analysis)
2. [Comprehensive Function Mapping](#comprehensive-function-mapping)
3. [Proposed Generic Functions](#proposed-generic-functions)
4. [Naming Conventions](#naming-conventions)
5. [Implementation Priority](#implementation-priority)
6. [Design Decisions Needed](#design-decisions-needed)

---

## Current State Analysis

### Existing Exports by Package

#### medfit (Foundation)
| Export | Type | Status |
|--------|------|--------|
| `extract_mediation()` | S7 generic | ✅ Implemented |
| `fit_mediation()` | Function | ⚠️ Stub |
| `bootstrap_mediation()` | Function | ⚠️ Stub |
| `MediationData` | S7 class | ✅ Implemented |
| `SerialMediationData` | S7 class | ✅ Implemented |
| `BootstrapResult` | S7 class | ✅ Implemented |

**Existing Methods:**
- `print()` for MediationData, SerialMediationData, BootstrapResult
- `summary()` for MediationData, BootstrapResult
- `show()` for MediationData, BootstrapResult

#### probmed
| Export | Type | Status |
|--------|------|--------|
| `pmed()` | S7 generic | ✅ Implemented |
| `extract_mediation()` | Re-export | ✅ |
| `PmedResult` | S7 class | ✅ Implemented |
| `MediationExtract` | S7 class | ✅ Implemented |

#### RMediation (CRAN stable)
| Export | Type | Status |
|--------|------|--------|
| `ci()` | S7 generic | ✅ Implemented |
| `medci()` | Function | ✅ Implemented |
| `cdf()` | S7 generic | ✅ Implemented |
| `dist_quantile()` | S7 generic | ✅ Implemented |
| `mbco()` | Function | ✅ Implemented |
| `pprodnormal()` | Function | ✅ Implemented |
| `qprodnormal()` | Function | ✅ Implemented |
| `tidy()` | S3 method | ✅ for logLik |
| `ProductNormal` | S7 class | ✅ Implemented |
| `MBCOResult` | S7 class | ✅ Implemented |

#### medrobust
| Export | Type | Status |
|--------|------|--------|
| `bound_ne()` | Function | ✅ Implemented |
| `sensitivity_region()` | Constructor | ✅ Implemented |
| `sensitivity_plot()` | Function | ✅ Implemented |
| `power_analysis()` | Function | ✅ Implemented |
| `bootstrap_results()` | Constructor | ✅ Implemented |
| `falsification_summary()` | Function | ✅ Implemented |

**Existing Methods:**
- `print()` for medrobust_bounds, compatibility_test
- `summary()` for medrobust_bounds, compatibility_test

#### medsim
| Export | Type | Status |
|--------|------|--------|
| `medsim_run()` | Function | ✅ Implemented |
| `medsim_scenario()` | Function | ✅ Implemented |
| `medsim_analyze()` | Function | ✅ Implemented |
| `medsim_config()` | Function | ✅ Implemented |
| `medsim_plot_*()` | Functions | ✅ Multiple |
| `medsim_cache_*()` | Functions | ✅ Multiple |

#### mediationverse (Meta)
| Export | Type | Status |
|--------|------|--------|
| `mediationverse_packages()` | Function | ✅ Implemented |
| `mediationverse_conflicts()` | Function | ✅ Implemented |
| `mediationverse_update()` | Function | ✅ Implemented |

---

## Comprehensive Function Mapping

### Master Table: Existing vs Proposed

| Purpose | Description | medfit | probmed | RMediation | medrobust | medsim | Base R | Tidy | **Proposed** |
|---------|-------------|--------|---------|------------|-----------|--------|--------|------|--------------|
| **DISPLAY** ||||||||||
| Compact display | One-line summary | `print()` ✅ | ❌ | ❌ | `print()` ✅ | ❌ | `print()` | — | `print()` |
| Detailed summary | Full results | `summary()` ✅ | ❌ | ❌ | `summary()` ✅ | ❌ | `summary()` | — | `summary()` |
| **EXTRACTION** ||||||||||
| Extract mediation | Get med structure | `extract_mediation()` ✅ | re-export ✅ | ❌ | ❌ | ❌ | — | — | `extract()` ❓ |
| All coefficients | Named vector | ❌ | ❌ | ❌ | ❌ | ❌ | `coef()` | — | `coef()` |
| Path coefficients | a, b, c' paths | `@a_path` | `@a_path` | ❌ | ❌ | ❌ | — | — | `paths()` |
| **EFFECTS** ||||||||||
| Indirect effect | NIE = a × b | in summary | `@estimate` | via `ci()` | `@NIE_*` | ❌ | — | — | `nie()` |
| Direct effect | NDE = c' | `@c_prime` | ❌ | ❌ | `@NDE_*` | ❌ | — | — | `nde()` |
| Controlled direct | CDE at fixed m | ❌ | ❌ | ❌ | ❌ | ❌ | — | — | `cde()` |
| Total effect | TE = nie + nde | computed | ❌ | ❌ | ❌ | ❌ | — | — | `te()` |
| Proportion mediated | NIE/TE | ❌ | ❌ | ❌ | ❌ | ❌ | — | — | `pm()` |
| **UNCERTAINTY** ||||||||||
| Variance-covariance | Cov matrix | `@vcov` | ❌ | `@Sigma` | ❌ | ❌ | `vcov()` | — | `vcov()` |
| Standard errors | SE vector | ❌ | ❌ | via `medci()` | ❌ | ❌ | — | — | `se()` |
| Confidence intervals | CI bounds | `@ci_*` | `@ci_*` | `ci()` ✅ | `@*_ci` | ❌ | `confint()` | — | `ci()` + `confint()` |
| P-values | Significance | ❌ | ❌ | ❌ | ❌ | ❌ | — | — | `pval()` |
| **INFERENCE** ||||||||||
| Bootstrap | Bootstrap CI | `bootstrap_mediation()` ⚠️ | internal | ❌ | `bootstrap_results` | ❌ | — | — | `boot()` ❓ |
| Prob of mediation | P(ab > 0) | ❌ | `pmed()` ✅ | `pprodnormal()` | ❌ | ❌ | — | — | `pmed()` ✅ |
| Distribution CDF | CDF of product | ❌ | ❌ | `cdf()` ✅ | ❌ | ❌ | — | — | `cdf()` ✅ |
| Distribution quantile | Quantile | ❌ | ❌ | `dist_quantile()` ✅ | ❌ | ❌ | — | — | `quantile()` |
| **SENSITIVITY** ||||||||||
| Sensitivity analysis | Unmeasured conf | ❌ | ❌ | ❌ | `sensitivity_region()` | ❌ | — | — | `sens()` |
| Effect bounds | Partial ID | ❌ | ❌ | ❌ | `bound_ne()` ✅ | ❌ | — | — | `bounds()` |
| Power analysis | Sample size | ❌ | ❌ | ❌ | `power_analysis()` ✅ | ❌ | — | — | `power()` |
| **TIDYVERSE** ||||||||||
| Tidy estimates | tibble output | ❌ | ❌ | `tidy()` ✅ | ❌ | ❌ | — | `tidy()` | `tidy()` |
| Model glance | 1-row summary | ❌ | ❌ | ❌ | ❌ | ❌ | — | `glance()` | `glance()` |
| Augmented data | Data + fitted | ❌ | ❌ | ❌ | ❌ | ❌ | — | `augment()` | `augment()` |
| **MODEL INFO** ||||||||||
| Sample size | n observations | `@n_obs` | ❌ | ❌ | ❌ | ❌ | `nobs()` | — | `nobs()` |
| Model formula | Extract formula | ❌ | ❌ | ❌ | ❌ | ❌ | `formula()` | — | `formula()` |
| Log-likelihood | Model log-lik | ❌ | ❌ | ❌ | ❌ | ❌ | `logLik()` | — | `logLik()` |
| **VISUALIZATION** ||||||||||
| Base plot | Base R graphics | ❌ | ❌ | in `medci` | `sensitivity_plot()` | `medsim_plot_*()` | `plot()` | — | `plot()` |
| ggplot2 | ggplot object | ❌ | ❌ | ❌ | ❌ | ❌ | — | `autoplot()` | `autoplot()` |
| Path diagram | DAG viz | ❌ | ❌ | ❌ | ❌ | ❌ | — | — | `diagram()` |
| **SIMULATION** ||||||||||
| Simulate data | Generate data | ❌ | ❌ | ❌ | `simulate_dm_data()` | `medsim_run()` | `simulate()` | — | `simulate()` |

**Legend:**
- ✅ = Implemented and exported
- ⚠️ = Stub/planned  
- ❌ = Not present
- ❓ = Decision needed
- `@property` = S7 property access

---

## Proposed Generic Functions

### Tier 1: Base R Generics (Must Have)

These are expected by all R users:

| Generic | Purpose | Defined In | Methods For |
|---------|---------|------------|-------------|
| `print()` | Compact display | base | All S7 classes |
| `summary()` | Detailed output | base | All S7 classes |
| `coef()` | Extract coefficients | stats | MediationData, PmedResult |
| `vcov()` | Variance-covariance | stats | MediationData, ProductNormal |
| `confint()` | Confidence intervals | stats | All estimable objects |
| `nobs()` | Sample size | stats | MediationData |
| `fitted()` | Fitted values | stats | MediationData |
| `residuals()` | Residuals | stats | MediationData |
| `predict()` | Predictions | stats | MediationData |
| `plot()` | Visualization | graphics | All result classes |
| `logLik()` | Log-likelihood | stats | MediationData |
| `formula()` | Extract formula | stats | MediationData |
| `update()` | Refit model | stats | MediationData |

### Tier 2: Tidyverse Generics (High Value)

Essential for modern R workflows:

| Generic | Package | Returns | Methods For |
|---------|---------|---------|-------------|
| `tidy()` | generics/broom | tibble: term, estimate, std.error, conf.low, conf.high | All result classes |
| `glance()` | generics/broom | 1-row tibble: n, method, converged | Result objects |
| `augment()` | generics/broom | tibble + .fitted, .resid | MediationData |

**Example `tidy()` output:**
```r
tidy(med_result)
# # A tibble: 5 × 7
#   term     estimate std.error statistic p.value conf.low conf.high
#   <chr>       <dbl>     <dbl>     <dbl>   <dbl>    <dbl>     <dbl>
# 1 a           0.500     0.050     10.0   <0.001    0.402     0.598
# 2 b           0.400     0.040     10.0   <0.001    0.322     0.478
# 3 nie         0.200     0.028      7.1   <0.001    0.145     0.255
# 4 nde         0.150     0.060      2.5    0.012    0.032     0.268
# 5 te          0.350     0.065      5.4   <0.001    0.223     0.477
```

### Tier 3: Domain-Specific Generics (New)

Short, memorable names for mediation-specific operations:

| Generic | Purpose | Arguments | Returns |
|---------|---------|-----------|---------|
| `nie()` | Natural Indirect Effect | `type = c("total", "pure")`, `ci = FALSE` | numeric + attrs |
| `nde()` | Natural Direct Effect | `type = c("total", "pure")`, `ci = FALSE` | numeric + attrs |
| `cde()` | Controlled Direct Effect | `m = NULL` (mediator value) | numeric + attrs |
| `te()` | Total Effect | `ci = FALSE` | numeric + attrs |
| `pm()` | Proportion Mediated | `ci = FALSE` | numeric [0,1] |
| `paths()` | Path Coefficients | `standardized = FALSE` | named vector |
| `se()` | Standard Errors | `type = c("delta", "boot")` | named vector |
| `pval()` | P-values | `alternative = "two.sided"` | named vector |
| `cf()` | Counterfactuals | `a = 1, astar = 0, m = NULL` | data.frame |
| `sens()` | Sensitivity Analysis | `type = c("uc", "me")` | sens object |
| `bounds()` | Effect Bounds | `rho = NULL` | bounds object |
| `diagram()` | Path Diagram | `layout = "auto"` | plot |

### Tier 4: Keep Existing (Brand Identity)

These are already implemented and should be preserved:

| Generic | Package | Keep As-Is |
|---------|---------|------------|
| `ci()` | RMediation | ✅ Core brand function |
| `pmed()` | probmed | ✅ Core brand function |
| `cdf()` | RMediation | ✅ Distribution function |
| `dist_quantile()` | RMediation | ✅ Distribution function |
| `extract_mediation()` | medfit | ✅ or shorten to `extract()` |

---

## Naming Conventions

### Short vs Long Names

| Short | Long Alias(es) | Purpose |
|-------|----------------|---------|
| `nie()` | `indirect()`, `indirect_effect()` | Natural Indirect Effect |
| `nde()` | `direct()`, `direct_effect()` | Natural Direct Effect |
| `cde()` | `controlled_direct()` | Controlled Direct Effect |
| `te()` | `total()`, `total_effect()` | Total Effect |
| `pm()` | `prop_med()`, `proportion_mediated()` | Proportion Mediated |
| `ci()` | `conf_int()` | Confidence Interval |
| `se()` | `std_error()` | Standard Error |
| `pval()` | `p_value()` | P-value |
| `paths()` | `path_coef()`, `path_coefficients()` | Path Coefficients |
| `sens()` | `sensitivity()` | Sensitivity Analysis |
| `bounds()` | `effect_bounds()` | Effect Bounds |
| `cf()` | `counterfactual()` | Counterfactual Outcomes |
| `pmed()` | `prob_mediation()` | Probability of Mediation |
| `diagram()` | `path_diagram()` | Path Diagram |

### Alias Implementation

```r
# In medfit/R/aab-generics.R

#' @rdname nie
#' @export
indirect_effect <- nie

#' @rdname nie
#' @export
indirect <- nie

#' @rdname nde
#' @export
direct_effect <- nde

#' @rdname nde
#' @export
direct <- nde

#' @rdname te
#' @export
total_effect <- te

#' @rdname te
#' @export
total <- te
```

Users can use either form:
```r
nie(med)              # Short (experts)
indirect_effect(med)  # Long (discoverable)
indirect(med)         # Medium (common usage)
```

### Argument-Modified Behavior

Instead of many separate functions, use `type` or `which` arguments:

```r
# Effects via coef()
coef(med)                      # c(a, b, cp)
coef(med, type = "paths")      # c(a, b, cp) - explicit
coef(med, type = "effects")    # c(nie, nde, te)
coef(med, type = "all")        # c(a, b, cp, nie, nde, te, pm)

# CI via ci()
ci(med)                        # Auto-select best method
ci(med, method = "delta")      # Delta method
ci(med, method = "boot")       # Bootstrap percentile
ci(med, method = "bca")        # Bootstrap BCa
ci(med, method = "mc")         # Monte Carlo (RMediation)
ci(med, method = "dop")        # Distribution of product
ci(med, which = "nie")         # CI for indirect only
ci(med, which = "all")         # CI for all effects
ci(med, level = 0.90)          # 90% CI

# Effect extraction with options
nie(med)                       # Point estimate only
nie(med, ci = TRUE)            # Include CI as attribute
nie(med, type = "pure")        # Pure NIE (PNIE)
nie(med, standardized = TRUE)  # Standardized effect
```

### Return Type Convention

Effects return numeric with attributes:

```r
x <- nie(med, ci = TRUE)
# x = 0.20
# attr(x, "se") = 0.028
# attr(x, "ci") = c(lower = 0.145, upper = 0.255)
# attr(x, "level") = 0.95
# attr(x, "method") = "delta"
# class(x) = c("mediation_effect", "numeric")
```

---

## Implementation Priority

### Phase 1: Core Infrastructure (P0)

**Timeline:** Immediate (medfit completion)

| Generic | Package | Action |
|---------|---------|--------|
| `coef()` | medfit | Add S7 method for MediationData |
| `vcov()` | medfit | Add S7 method for MediationData |
| `confint()` | medfit | Add S7 method for MediationData |
| `nobs()` | medfit | Add S7 method for MediationData |
| `print()` | medfit | ✅ Already implemented |
| `summary()` | medfit | ✅ Already implemented |

### Phase 2: Effect Extraction (P1)

**Timeline:** After medfit core complete

| Generic | Package | Action |
|---------|---------|--------|
| `nie()` | medfit | New S7 generic + methods |
| `nde()` | medfit | New S7 generic + methods |
| `te()` | medfit | New S7 generic + methods |
| `pm()` | medfit | New S7 generic + methods |
| `paths()` | medfit | New S7 generic + methods |
| `se()` | medfit | New S7 generic + methods |

### Phase 3: Tidyverse Integration (P2)

**Timeline:** Q1 2025

| Generic | Package | Action |
|---------|---------|--------|
| `tidy()` | medfit, probmed, medrobust | Add methods |
| `glance()` | medfit, probmed | Add methods |
| `augment()` | medfit | Add method |

### Phase 4: Advanced Features (P3)

**Timeline:** Q2 2025

| Generic | Package | Action |
|---------|---------|--------|
| `cde()` | medfit | New S7 generic + methods |
| `cf()` | medfit | New S7 generic + methods |
| `sens()` | medrobust | Wrapper for sensitivity_region |
| `bounds()` | medrobust | Wrapper for bound_ne |
| `diagram()` | medfit/mediationverse | New function |
| `plot()` | All packages | Add S7 methods |
| `autoplot()` | All packages | Add S7 methods |

---

## Design Decisions Needed

### Decision 1: Rename `extract_mediation()`?

**Options:**
- A) Keep `extract_mediation()` — explicit, brand identity
- B) Shorten to `extract()` — cleaner, may conflict
- C) Both (alias) — `extract()` calls `extract_mediation()`

**Recommendation:** Option C — keep both for compatibility

### Decision 2: Effect Extraction Approach

**Options:**
- A) Separate functions: `nie()`, `nde()`, `te()`, `cde()`, `pm()`
- B) Single function with type: `effect(type = "indirect")`
- C) Both — short functions call longer one internally

**Recommendation:** Option A — cleaner API, more memorable

### Decision 3: `boot()` Naming Conflict

**Options:**
- A) Use `boot()` — rely on namespace (`medfit::boot()`)
- B) Use `mboot()` — avoid conflict with boot package
- C) Use `bootstrap()` — no short form, explicit
- D) Keep `bootstrap_mediation()` — current name

**Recommendation:** Option D — keep current, add `boot()` as alias with warning

### Decision 4: Which Aliases to Provide

**Options:**
- A) Full set: short + medium + long (e.g., `nie`, `indirect`, `indirect_effect`)
- B) Minimal: short + long only (e.g., `nie`, `indirect_effect`)
- C) Short only: `nie`, `nde`, etc.

**Recommendation:** Option B — balance discoverability and simplicity

### Decision 5: `confint()` vs `ci()` Coexistence

Both should exist with different purposes:

| Function | Purpose | Input | Method |
|----------|---------|-------|--------|
| `confint()` | Base R generic | Model objects | Delta, bootstrap |
| `ci()` | RMediation brand | Distribution params | Analytical, MC, DOP |

**Recommendation:** Keep both — `confint()` for model objects, `ci()` for distributions

### Decision 6: Existing Function Renaming

Should existing medrobust functions be renamed for consistency?

| Current | Proposed | Decision |
|---------|----------|----------|
| `bound_ne()` | `bounds()` | Alias? |
| `sensitivity_region()` | `sens_region()` | Keep |
| `sensitivity_plot()` | `plot.sens()` | Keep |
| `power_analysis()` | `power()` | Alias? |

**Recommendation:** Add short aliases, keep originals for backward compatibility

---

## Cross-Package Method Registration

| Generic | Defined In | Methods In |
|---------|------------|------------|
| `extract_mediation()` | medfit ✅ | medfit (lm, glm, lavaan) |
| `nie()` | medfit | medfit, probmed, medrobust |
| `nde()` | medfit | medfit, probmed, medrobust |
| `cde()` | medfit | medfit, medrobust |
| `te()` | medfit | medfit, probmed |
| `pm()` | medfit | medfit, probmed |
| `ci()` | RMediation ✅ | RMediation, medfit, probmed |
| `pmed()` | probmed ✅ | probmed |
| `cdf()` | RMediation ✅ | RMediation |
| `sens()` | medrobust | medrobust |
| `bounds()` | medrobust | medrobust |
| `tidy()` | generics | medfit, probmed, RMediation, medrobust |
| `glance()` | generics | medfit, probmed, RMediation |

---

## Potential Conflicts

| Name | Conflict With | Resolution |
|------|---------------|------------|
| `ci()` | — | ✅ RMediation owns this |
| `se()` | Some packages informally | Define in medfit, document |
| `boot()` | `boot::boot()` | Keep `bootstrap_mediation()`, add alias with warning |
| `sens()` | — | ✅ Safe |
| `pm()` | — | ✅ Safe |
| `cf()` | — | ✅ Safe |
| `te()` | — | ✅ Safe |
| `diagram()` | DiagrammeR? | Check, likely safe |
| `extract()` | dplyr? tidyr? | Use `extract_mediation()` as primary |

---

## References

### Packages Reviewed for Naming Conventions

1. **lavaan** — `parameterEstimates()`, `standardizedSolution()`, `fitMeasures()`, `lavInspect()`
2. **CMAverse** — `cmest()`, `cmdag()`, `cmsens()`, `ggcmest()`
3. **mediation** — `mediate()`, `medsens()`, `mediations()`
4. **broom** — `tidy()`, `glance()`, `augment()`

### Key Insights from Other Packages

- **lavaan**: Separate extraction functions for different output types
- **CMAverse**: Prefix convention (`cm*`) for namespace clarity
- **mediation**: Simple naming, good S3 method coverage
- **broom**: Consistent tibble output structure across all packages

---

## Next Steps

1. [ ] Review and approve this strategy document
2. [ ] Finalize design decisions (6 items above)
3. [ ] Update API-CONTRACTS.md with approved generics
4. [ ] Implement Phase 1 (P0) generics in medfit
5. [ ] Create test cases for new generics
6. [ ] Document in pkgdown reference

---

## Appendix: Full Function Signatures

### medfit

```r
extract_mediation(object, ...)
fit_mediation(formula_y, formula_m, data, treatment, mediator, 
              engine = "glm", family_y = gaussian(), family_m = gaussian(), ...)
bootstrap_mediation(statistic_fn, method = c("parametric", "nonparametric", "plugin"),
                    mediation_data = NULL, data = NULL, n_boot = 1000, 
                    ci_level = 0.95, parallel = FALSE, ncores = NULL, seed = NULL, ...)
```

### probmed

```r
pmed(object, ...)
```

### RMediation

```r
ci(mu, ...)
medci(mu.x, mu.y, se.x, se.y, rho = 0, alpha = 0.05, 
      type = "dop", plot = FALSE, plotCI = FALSE, n.mc = 1e5, ...)
cdf(object, ...)
dist_quantile(object, ...)
mbco(h0, h1, ...)
pprodnormal(q, mu.x, mu.y, se.x = 1, se.y = 1, rho = 0, 
            lower.tail = TRUE, type = "dop", n.mc = 1e5)
qprodnormal(p, mu.x, mu.y, se.x, se.y, rho = 0, 
            lower.tail = TRUE, type = "dop", n.mc = 1e5)
```

### medrobust

```r
bound_ne(data, exposure, mediator, outcome, confounders = NULL,
         misclassified_variable = c("exposure", "mediator"),
         sensitivity_region = NULL, n_grid = 50, 
         effect_scale = c("OR", "RR", "RD"), confidence_level = 0.95,
         bootstrap = FALSE, bootstrap_reps = 1000, 
         bootstrap_method = c("percentile", "bca"),
         parallel = FALSE, n_cores = NULL, cache = FALSE, 
         cache_dir = NULL, verbose = TRUE, stratify_by = NULL,
         use_adaptive_grid = TRUE, 
         grid_method = c("lhs", "auto", "regular", "adaptive", "sobol", "binary"))

sensitivity_region(sn0_range, sp0_range, psi_sn_range, psi_sp_range)

power_analysis(true_params, dm_params, sensitivity_region, 
               misclass_type = c("exposure", "mediator"),
               sample_sizes = seq(100, 1000, by = 100),
               target_width = NULL, target_power = 0.8, alpha = 0.05,
               effect = c("NIE", "NDE"), n_sim = 100, n_grid = 30,
               confounders = 1, parallel = TRUE, n_cores = NULL,
               verbose = TRUE, seed = 12345)

sensitivity_plot(bounds_object, param = "psi_sn", 
                 effect = c("both", "NIE", "NDE"),
                 plot_type = c("bounds", "heatmap", "contour"),
                 show_naive = TRUE, show_null = TRUE,
                 color_scheme = "default", theme = "bw", ...)
```

### medsim

```r
medsim_run(method, scenarios, config, compute_truth = NULL, 
           parallel = TRUE, verbose = TRUE)
medsim_scenario(name, description = "", data_generator, params = list())
medsim_analyze(results, metrics = "all", by_scenario = TRUE)
medsim_config(mode = "auto", n_replications = NULL, n_cores = NULL,
              scenarios = NULL, output_dir = NULL, seed = 12345, ...)
```

---

*Document created: December 11, 2024*  
*Last updated: December 11, 2024*
