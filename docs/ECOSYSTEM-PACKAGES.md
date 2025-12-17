# Mediationverse Package Ecosystem

**Last Updated:** 2025-12-17

This document provides a comprehensive summary of all packages in the mediationverse ecosystem.

---

## Quick Reference

| Package | Version | Status | Primary Function |
|---------|---------|--------|------------------|
| **medfit** | 0.1.0.9000 | Active | Foundation - model fitting & extraction |
| **probmed** | 0.0.0.9000 | Active | P_med probabilistic effect size |
| **RMediation** | 1.4.0 | Stable (CRAN) | CI methods (DOP, MBCO, MC) |
| **medrobust** | 0.1.0.9000 | Active | Sensitivity analysis & bounds |
| **medsim** | 0.0.0.9000 | Experimental | Simulation infrastructure |
| **mediationverse** | 0.0.0.9000 | Active | Meta-package coordinator |

---

## Package Details

### 1. medfit (Foundation)

**Purpose:** Core infrastructure for mediation model fitting and extraction.

**S7 Classes:**
- `MediationData` - Simple mediation (X → M → Y)
- `SerialMediationData` - Serial chains (X → M1 → M2 → ... → Y)
- `BootstrapResult` - Standardized bootstrap output

**Key Functions:**
| Function | Status | Description |
|----------|--------|-------------|
| `fit_mediation()` | Complete | Fit mediator & outcome models |
| `extract_mediation()` | Complete | S7 generic for lm/glm/lavaan |
| `bootstrap_mediation()` | Complete | Parametric/nonparametric/plugin |

**Methods:** print(), summary(), show() for all classes

**Engines:** GLM (complete), lmer (planned), brms (planned)

---

### 2. probmed

**Purpose:** Probabilistic effect sizes for causal mediation.

**Core Concept:** P_med = P(Y_{x*, M_x} > Y_{x, M_x})

**S7 Classes:**
- `PmedResult` - Full result with bootstrap distribution

**Key Functions:**
| Function | Status | Description |
|----------|--------|-------------|
| `pmed()` | Complete | S7 generic (formula & MediationData) |
| `extract_mediation()` | Re-export | From medfit |

**Inference Methods:**
- Plugin (point estimate only)
- Parametric bootstrap (default)
- Nonparametric bootstrap

**Output:** P_med estimate + CI, Indirect Effect + CI

---

### 3. RMediation (CRAN Stable)

**Purpose:** Confidence intervals for products of coefficients.

**S7 Classes:**
- `ProductNormal` - Distribution of product of normals
- `MBCOResult` - MBCO test results

**Key Functions:**
| Function | Status | Description |
|----------|--------|-------------|
| `ci()` | Complete | CIs for nonlinear functions |
| `medci()` | Complete | CIs for two-variable products |
| `cdf()` | Complete | CDF for ProductNormal |
| `dist_quantile()` | Complete | Quantile for ProductNormal |
| `mbco()` | Complete | Model-Based Constrained Optimization |
| `pprodnormal()` | Complete | CDF for product of two normals |
| `qprodnormal()` | Complete | Quantile for product of two normals |

**CI Methods:** DOP (distribution of product), MC (Monte Carlo), Asymptotic

---

### 4. medrobust

**Purpose:** Sensitivity analysis under differential misclassification.

**S7 Classes:**
- `sensitivity_region` - Parameter space specification
- `medrobust_bounds` - Partial identification bounds
- `bootstrap_results` - Inference results
- `power_analysis_result` - Design output

**Key Functions:**
| Function | Status | Description |
|----------|--------|-------------|
| `bound_ne()` | Complete | NDE/NIE bounds (exposure/mediator) |
| `sensitivity_plot()` | Complete | Bounds/heatmap/contour plots |
| `power_analysis()` | Complete | Simulation-based power |
| `simulate_dm_data()` | Complete | Generate misclassified data |

**Grid Search Methods:** LHS (default), Regular, Sobol, Adaptive, Binary

**Effect Scales:** OR (odds ratio), RR (risk ratio), RD (risk difference)

---

### 5. medsim

**Purpose:** Simulation infrastructure for Monte Carlo studies.

**Key Functions:**
| Function | Status | Description |
|----------|--------|-------------|
| `medsim_run()` | Complete | Execute simulation study |
| `medsim_config()` | Complete | Auto-detect local/cluster |
| `medsim_scenario()` | Complete | Define custom scenarios |
| `medsim_analyze()` | Complete | Compute accuracy metrics |
| `medsim_cache_*()` | Complete | Ground truth caching |
| `medsim_plot_*()` | Complete | Publication figures |

**Config Modes:** test (30s), local (15min), cluster (hours), auto

**Built-in Scenarios:** 6 standard (varying ρ, suppression, unequal variance)

**Metrics:** Bias, MAE, RMSE, coverage, power

---

### 6. mediationverse (Meta-Package)

**Purpose:** Coordinate ecosystem loading and updates.

**Key Functions:**
| Function | Description |
|----------|-------------|
| `mediationverse_packages()` | List all packages with status |
| `mediationverse_conflicts()` | Detect function collisions |
| `mediationverse_update()` | Update all packages |

**Loading Strategy:**
- `library(mediationverse)` loads **medfit** only
- Other packages require explicit `library()` calls
- Minimizes namespace pollution

---

## Dependency Graph

```
mediationverse (meta)
    │
    ├── medfit (foundation) ←── probmed
    │       ↑                      │
    │       └──────────────────────┘
    │
    ├── RMediation (CRAN stable, independent)
    │
    ├── medrobust (depends on medfit for data structures)
    │
    └── medsim (tests all packages)
```

---

## Current Generic Functions

### Implemented Across Packages

| Generic | medfit | probmed | RMediation | medrobust |
|---------|--------|---------|------------|-----------|
| `print()` | ✅ | ✅ | ✅ | ✅ |
| `summary()` | ✅ | ✅ | ✅ | ✅ |
| `plot()` | - | ✅ | - | ✅ |
| `tidy()` | - | - | ✅ | - |
| `extract_mediation()` | ✅ | re-export | - | - |
| `ci()` | - | - | ✅ | - |
| `pmed()` | - | ✅ | - | - |
| `cdf()` | - | - | ✅ | - |

### Proposed (from GENERIC-FUNCTIONS-STRATEGY.md)

| Generic | Purpose | Define In |
|---------|---------|-----------|
| `med()` | ADHD-friendly entry point | medfit |
| `quick()` | Instant answer | medfit |
| `nie()` | Natural Indirect Effect | medfit |
| `nde()` | Natural Direct Effect | medfit |
| `te()` | Total Effect | medfit |
| `pm()` | Proportion Mediated | medfit |
| `sens()` | Sensitivity analysis | medrobust |
| `bounds()` | Effect bounds | medrobust |

---

## Repository Locations

| Package | GitHub | Local Path |
|---------|--------|------------|
| medfit | Data-Wise/medfit | `~/projects/r-packages/active/medfit/` |
| probmed | Data-Wise/probmed | `~/projects/r-packages/active/probmed/` |
| RMediation | CRAN | `~/projects/r-packages/stable/rmediation/` |
| medrobust | Data-Wise/medrobust | `~/projects/r-packages/active/medrobust/` |
| medsim | Data-Wise/medsim | `~/projects/r-packages/active/medsim/` |
| mediationverse | Data-Wise/mediationverse | `~/projects/r-packages/active/mediationverse/` |

---

## Installation

```r
# Install all from GitHub
pak::pak("Data-Wise/mediationverse")

# Or individually
pak::pak("Data-Wise/medfit")
pak::pak("Data-Wise/probmed")
install.packages("RMediation")  # CRAN
pak::pak("Data-Wise/medrobust")
pak::pak("Data-Wise/medsim")
```

---

## Usage Example

```r
library(mediationverse)  # Loads medfit automatically

# Simple analysis with medfit
result <- fit_mediation(
  formula_y = Y ~ X + M,
  formula_m = M ~ X,
  data = mydata,
  treatment = "X",
  mediator = "M"
)

# Bootstrap inference
boot_result <- bootstrap_mediation(
  statistic_fn = function(d) nie(fit_mediation(..., data = d)),
  data = mydata,
  n_boot = 1000
)

# P_med effect size
library(probmed)
pmed_result <- pmed(result)

# Sensitivity analysis
library(medrobust)
bounds <- bound_ne(data = mydata, ...)

# CI via distribution of product
library(RMediation)
ci_result <- medci(mu.x = 0.3, mu.y = 0.4, se.x = 0.05, se.y = 0.04)
```
