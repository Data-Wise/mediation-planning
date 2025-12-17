# medsim: Simulation Infrastructure for Mediation Ecosystem

**Date**: December 3, 2025
**Status**: Proposed
**Package Name**: `medsim`
**Target Release**: Q3 2025 (after medfit MVP)

---

## Executive Summary

Create a unified simulation package (`medsim`) that provides:

1. **Standardized simulation framework** for Monte Carlo studies
2. **Environment-aware execution** (local laptop, cluster, HPC)
3. **Parallel processing** with progress reporting
4. **Automated result analysis** and visualization
5. **Reproducibility infrastructure** (caching, seeds, renv integration)
6. **Shared across all mediation packages** (medfit, probmed, RMediation, medrobust)

**Inspiration**: Product-of-three simulation (`/prod3/code/R/`) demonstrates excellent patterns for:
- Environment auto-detection
- Config-driven execution (test/local/cluster)
- Ground truth caching
- Parallel processing with progress bars
- Publication-ready output generation

---

## Motivation

### Current State: Fragmented Simulation Code

Each package currently has simulation code scattered across:

```
medfit/
├── tests/testthat/helper-sim.R        # Test data generators
└── vignettes/simulation-study.qmd     # Ad-hoc simulation

probmed/
├── inst/simulations/bootstrap_sim.R   # Custom bootstrap simulation
└── tests/testthat/test-accuracy.R     # Accuracy testing

RMediation/
└── tests/accuracy/                    # Legacy simulation scripts

medrobust/
└── sensitivity_sims/                  # Sensitivity simulations
```

**Problems**:
- Code duplication (each package reimplements parallel processing, progress bars, etc.)
- Inconsistent APIs and output formats
- No standard way to run on HPC
- Difficult to compare results across packages
- No caching or reproducibility infrastructure

### Proposed State: Unified Infrastructure

```
medsim/                                 # Unified simulation package
├── R/
│   ├── config.R                       # Environment detection, mode selection
│   ├── scenarios.R                    # Standard scenario definitions
│   ├── runner.R                       # Parallel execution engine
│   ├── cache.R                        # Ground truth caching
│   ├── progress.R                     # Progress reporting
│   ├── analyze.R                      # Result analysis
│   └── visualize.R                    # Publication-ready figures
└── inst/
    └── templates/                     # Templates for new simulations

# Used by all packages:
medfit/simulations/accuracy_study.R → uses medsim
probmed/simulations/pmed_coverage.R → uses medsim
RMediation/simulations/dop_power.R → uses medsim
```

**Benefits**:
- Write simulation once, run anywhere (local/cluster)
- Consistent output formats enable cross-package comparisons
- Shared infrastructure reduces bugs and maintenance
- Publication-ready figures with single function call

---

## Package Architecture

### Core Design Principles

1. **Environment Agnostic**: Auto-detect local vs cluster, adjust parameters accordingly
2. **Config-Driven**: Separate configuration from simulation logic
3. **Functional Style**: Pure functions, no global state
4. **Parallel-Ready**: Built-in parallel processing with proper progress reporting
5. **Caching**: Ground truth caching to avoid recomputation
6. **Reproducibility**: Seed management, renv integration, version tracking

### Inspired by Product-of-Three Simulation

The product-of-three simulation demonstrates excellent patterns:

**1. Environment Detection** (`config.R`):
```r
get_config <- function(mode = "auto") {
  if (mode == "auto") {
    if (Sys.getenv("SLURM_JOB_ID") != "") {
      mode <- "cluster"
    } else {
      mode <- "local"
    }
  }
  # Return appropriate parameters for mode
}
```

**2. Three Execution Modes**:

| Mode | Replications | Quantiles | Runtime | Use Case |
|------|--------------|-----------|---------|----------|
| test | 20 | 5 | ~30s | Quick validation |
| local | 100 | 19 | ~15m | Development |
| cluster | 1000 | 99 | hours | Production |

**3. Ground Truth Caching**:
```r
compute_ground_truth <- function(scenario, cache_file = NULL) {
  if (!is.null(cache_file) && file.exists(cache_file)) {
    return(read.csv(cache_file))
  }
  # Expensive computation...
  if (!is.null(cache_file)) {
    write.csv(truth, cache_file)
  }
}
```

**4. Parallel Processing**:
```r
cl <- makeCluster(n_cores)
registerDoParallel(cl)
results <- pblapply(param_grid, FUN = simulate_one, cl = cl)
stopCluster(cl)
```

**5. Automated Workflow**:
```r
# complete_workflow.R
1. Run simulation → results.csv
2. Analyze → summary stats
3. Visualize → figures/*.pdf
4. Generate report → manuscript/results.tex
5. Build PDF → build/manuscript.pdf
```

---

## Proposed API

### 1. Configuration

```r
library(medsim)

# Auto-detect environment
config <- medsim_config(mode = "auto")

# Or manually specify
config_test <- medsim_config(mode = "test")      # Quick validation
config_local <- medsim_config(mode = "local")    # Development
config_cluster <- medsim_config(mode = "cluster")  # Production

# Print configuration summary
print(config)
#> ========================================================
#>   LOCAL MODE
#> ========================================================
#>
#> Simulation Parameters:
#>   Replications:        100
#>   Scenarios:           all
#>
#> Computing Resources:
#>   Cores:               10
#>   Random Seed:         12345
#>
#> Output:
#>   Directory:           simulation_results
#> ========================================================

# Custom configuration
config_custom <- medsim_config(
  n_replications = 500,
  n_cores = 8,
  seed = 42,
  output_dir = "my_results"
)
```

### 2. Scenario Definitions

```r
# Use standard mediation scenarios
scenarios <- medsim_scenarios_mediation()
#> Creates 6 standard scenarios:
#>   1. Independent paths
#>   2. Moderate correlation
#>   3. High correlation
#>   4. Suppression
#>   5. Non-zero effects
#>   6. Unequal variances

# Custom scenario
my_scenario <- medsim_scenario(
  name = "Custom Scenario",
  data_generator = function(n) {
    data.frame(
      X = rnorm(n),
      M = rnorm(n),
      Y = rnorm(n)
    )
  },
  params = list(n = 200, a = 0.3, b = 0.5)
)

scenarios <- c(scenarios, my_scenario)
```

### 3. Simulation Execution

```r
# Define what to simulate (user provides this function)
my_method <- function(data, params) {
  # User's method implementation
  # E.g., compute mediation effect, fit model, etc.
  fit_m <- lm(M ~ X, data = data)
  fit_y <- lm(Y ~ X + M, data = data)

  a <- coef(fit_m)["X"]
  b <- coef(fit_y)["M"]

  list(
    indirect = a * b,
    a_path = a,
    b_path = b
  )
}

# Run simulation
results <- medsim_run(
  method = my_method,
  scenarios = scenarios,
  config = config,
  methods_to_compare = list(
    proposed = my_method,
    delta = delta_method,
    bootstrap = bootstrap_method
  )
)
#> Detected: LOCAL environment
#> [STEP 1] Computing ground truth...
#>   Scenario 1/6: Independent paths
#>   ✓ Cached ground truth loaded
#> [STEP 2] Running simulation replications...
#>   Scenario 1/6: Independent paths
#>   |========================================| 100%
#> ...
#> Results saved to: simulation_results/all_results.csv
```

### 4. Analysis and Visualization

```r
# Automatic analysis
summary <- medsim_analyze(results)
#> Computing accuracy metrics...
#> Computing timing statistics...
#> Computing coverage rates...

print(summary)
#> Accuracy (Maximum Absolute Error):
#>   Proposed:   1.2e-04
#>   Delta:      0.169 (1,367× worse)
#>   Bootstrap:  0.009 (75× worse)

# Generate publication-ready figures
medsim_visualize(
  results,
  output_dir = "figures",
  format = c("pdf", "png")
)
#> Generated:
#>   figures/error_boxplot.pdf
#>   figures/error_by_quantile.pdf
#>   figures/cdf_comparison.pdf
#>   figures/timing_barplot.pdf
#>   figures/combined_panel.pdf (4-panel for manuscript)

# Generate LaTeX tables
medsim_tables(
  results,
  output_dir = "tables",
  format = "latex"
)
#> Generated:
#>   tables/accuracy_table.tex
#>   tables/timing_table.tex
```

### 5. Complete Workflow (Single Function)

```r
# One-shot: simulation → analysis → figures → tables
medsim_workflow(
  method = my_method,
  scenarios = scenarios,
  config = config,
  output_dir = "output",
  run_simulation = TRUE  # Set FALSE to use cached results
)
#> [1/4] Running simulation...
#> [2/4] Analyzing results...
#> [3/4] Generating figures...
#> [4/4] Creating tables...
#>
#> ✓ Complete! Results in: output/
#>   - output/all_results.csv
#>   - output/summary_stats.csv
#>   - output/figures/*.pdf
#>   - output/tables/*.tex
```

---

## Feature Set

### Core Features

#### 1. Environment Detection

```r
# Automatic detection
is_cluster <- medsim_detect_cluster()
#> Checks for:
#>   - SLURM_JOB_ID (HPC cluster)
#>   - PBS_JOBID (PBS scheduler)
#>   - LSB_JOBID (LSF scheduler)
#>   - Interactive session

# Adjust parameters based on environment
config <- if (is_cluster) {
  medsim_config("cluster")
} else {
  medsim_config("local")
}
```

#### 2. Parallel Processing

```r
# Built-in parallel execution
results <- medsim_run_parallel(
  tasks = param_grid,
  fun = simulate_one,
  n_cores = 10,
  progress = TRUE,  # Show progress bar
  cluster_type = "PSOCK"  # or "FORK" on Unix
)

# Handles:
#   - Cluster setup/teardown
#   - Package loading on workers
#   - Object export to workers
#   - Progress bars (suppressed on cluster logs)
#   - Error handling and recovery
```

#### 3. Ground Truth Caching

```r
# Compute expensive ground truth once
truth <- medsim_compute_truth(
  scenario = scenario,
  method = monte_carlo_truth,
  n_samples = 1e8,
  cache_file = "cache/truth_scenario1.rds"
)
#> Cached truth loaded from: cache/truth_scenario1.rds

# Force recomputation
truth <- medsim_compute_truth(..., force = TRUE)
```

#### 4. Reproducibility

```r
# Set global seed
medsim_set_seed(12345)

# Track package versions
medsim_save_session_info("results/session_info.txt")

# renv integration
medsim_init_renv("my_simulation")
#> Creates:
#>   - renv/ directory
#>   - renv.lock file
#>   - .Rprofile with renv activation
```

#### 5. Progress Reporting

```r
# Automatic progress bars (local only)
results <- medsim_run(...)
#> [STEP 1] Computing ground truth...
#>   Scenario 1/6: Independent paths
#>   |========================================| 100%
#> [STEP 2] Running simulations...
#>   |========================================| 100%
#>   Est. remaining: 2.5 minutes

# On cluster (batch jobs): minimal output
#> [STEP 1] Computing ground truth... (6 scenarios)
#> [STEP 2] Running simulations... (6000 total iterations)
```

#### 6. Result Analysis

```r
# Automatic metrics
summary <- medsim_analyze(results)

summary$accuracy
#>   method     mae     median_ae  max_ae
#>   proposed   1.2e-04 5.3e-05    8.7e-04
#>   delta      0.169   0.156      0.342
#>   bootstrap  0.009   0.007      0.043

summary$timing
#>   method     mean_time  median_time  total_time
#>   proposed   3.95s      3.82s        6h 35m
#>   delta      0.001s     0.001s       1m 40s
#>   bootstrap  0.0003s    0.0003s      30s

summary$coverage
#>   method     coverage_90  coverage_95  coverage_99
#>   proposed   0.902        0.951        0.991
#>   delta      0.645        0.723        0.834
#>   bootstrap  0.887        0.944        0.986
```

#### 7. Visualization

```r
# Standard plots for simulation studies
medsim_plot_error_boxplot(results)
medsim_plot_error_by_quantile(results)
medsim_plot_cdf_comparison(results, scenario = "Independent")
medsim_plot_timing(results)
medsim_plot_coverage(results)
medsim_plot_power_curves(results)

# Combined panel for manuscript
medsim_plot_combined_panel(
  results,
  panels = c("error", "timing", "coverage", "cdf"),
  layout = "2x2"
)

# Customization
medsim_plot_error_boxplot(
  results,
  log_scale = TRUE,
  color_palette = "viridis",
  theme = "minimal"
)
```

---

## Integration with Mediation Packages

### medfit Integration

```r
# medfit/inst/simulations/accuracy_study.R

library(medsim)
library(medfit)

# Define scenarios
scenarios <- medsim_scenarios_mediation()

# Define extraction methods
extract_proposed <- function(data, params) {
  fit_m <- lm(M ~ X, data = data)
  fit_y <- lm(Y ~ X + M, data = data)

  med_data <- medfit::extract_mediation(
    fit_m, model_y = fit_y,
    treatment = "X", mediator = "M"
  )

  list(
    indirect = med_data@a_path * med_data@b_path,
    a_path = med_data@a_path,
    b_path = med_data@b_path
  )
}

# Run simulation
results <- medsim_run(
  method = extract_proposed,
  scenarios = scenarios,
  config = medsim_config("local")
)

# Analyze and visualize
medsim_workflow(results, output_dir = "inst/simulations/results")
```

### probmed Integration

```r
# probmed/inst/simulations/pmed_coverage.R

library(medsim)
library(probmed)
library(medfit)

# Simulation function
compute_pmed_with_ci <- function(data, params) {
  fit_m <- lm(M ~ X, data = data)
  fit_y <- lm(Y ~ X + M, data = data)

  med_data <- medfit::extract_mediation(fit_m, model_y = fit_y,
                                        treatment = "X", mediator = "M")

  # P_med point estimate
  p_med <- probmed::compute_pmed(med_data)

  # Bootstrap CI
  boot_result <- medfit::bootstrap_mediation(
    med_data,
    statistic = probmed::compute_pmed,
    n_boot = 1000
  )

  list(
    p_med = p_med,
    ci_lower = boot_result@ci_lower,
    ci_upper = boot_result@ci_upper
  )
}

# Run coverage simulation
config <- medsim_config("local")
config$n_replications <- 1000  # Need many reps for coverage

results <- medsim_run(
  method = compute_pmed_with_ci,
  scenarios = scenarios,
  config = config
)

# Check coverage rates
summary <- medsim_analyze_coverage(results, nominal_level = 0.95)
#> Method: P_med Bootstrap CI
#>   Nominal Coverage: 95%
#>   Actual Coverage:  94.6% (acceptable)
#>   CI Width (median): 0.234
```

### RMediation Integration

```r
# RMediation/inst/simulations/dop_vs_mc.R

library(medsim)
library(RMediation)
library(medfit)

# Compare DOP vs Monte Carlo CIs
methods <- list(
  dop = function(data, params) {
    med_data <- medfit::extract_mediation(...)
    ci <- RMediation::medci(
      med_data@a_path,
      med_data@b_path,
      type = "dop"
    )
    list(ci_lower = ci$lower, ci_upper = ci$upper)
  },

  mc = function(data, params) {
    med_data <- medfit::extract_mediation(...)
    ci <- RMediation::medci(
      med_data@a_path,
      med_data@b_path,
      type = "mc"
    )
    list(ci_lower = ci$lower, ci_upper = ci$upper)
  }
)

results <- medsim_compare_methods(
  methods = methods,
  scenarios = scenarios,
  config = medsim_config("local")
)

# Generate comparison table
medsim_compare_table(results, metrics = c("coverage", "width", "power"))
```

---

## Package Structure

```
medsim/
├── DESCRIPTION
├── NAMESPACE
├── README.md
├── NEWS.md
├── LICENSE
├── _pkgdown.yml
│
├── R/
│   ├── medsim-package.R           # Package documentation
│   ├── config.R                   # Configuration and environment detection
│   ├── scenarios.R                # Standard scenario definitions
│   ├── runner.R                   # Simulation execution engine
│   ├── parallel.R                 # Parallel processing utilities
│   ├── cache.R                    # Ground truth caching
│   ├── progress.R                 # Progress reporting
│   ├── analyze.R                  # Result analysis
│   ├── visualize.R                # Plotting functions
│   ├── tables.R                   # LaTeX table generation
│   ├── workflow.R                 # Complete workflow functions
│   ├── utils.R                    # Utility functions
│   └── zzz.R                      # .onLoad()
│
├── inst/
│   ├── templates/
│   │   ├── simulation_script.R    # Template for new simulations
│   │   ├── cluster_submit.sh      # SLURM submission script template
│   │   └── config_template.R      # Configuration template
│   ├── examples/
│   │   ├── basic_simulation.R
│   │   ├── mediation_coverage.R
│   │   └── power_analysis.R
│   └── extdata/
│       └── standard_scenarios.rds # Pre-defined scenarios
│
├── tests/
│   └── testthat/
│       ├── test-config.R
│       ├── test-parallel.R
│       ├── test-cache.R
│       └── test-workflow.R
│
├── vignettes/
│   ├── getting-started.qmd        # Quick start guide
│   ├── custom-scenarios.qmd       # Defining custom scenarios
│   ├── parallel-computing.qmd     # Running on HPC clusters
│   ├── mediation-sims.qmd         # Mediation-specific simulations
│   └── advanced-topics.qmd        # Advanced features
│
└── man/                           # Generated documentation
```

---

## Implementation Roadmap

### Phase 1: Core Infrastructure (Weeks 1-2)

**Goal**: Basic simulation runner with parallel processing

- [ ] Package skeleton (`usethis::create_package("medsim")`)
- [ ] Environment detection (`config.R`)
- [ ] Parallel execution (`parallel.R`, `runner.R`)
- [ ] Progress reporting (`progress.R`)
- [ ] Basic tests

**Deliverable**: Can run simple simulation in parallel

### Phase 2: Mediation-Specific Features (Weeks 3-4)

**Goal**: Standard scenarios and mediation utilities

- [ ] Standard mediation scenarios (`scenarios.R`)
- [ ] Ground truth caching (`cache.R`)
- [ ] Integration with medfit classes
- [ ] Example simulations in `inst/examples/`

**Deliverable**: Can run mediation simulation with standard scenarios

### Phase 3: Analysis and Visualization (Weeks 5-6)

**Goal**: Automated result analysis and publication-ready figures

- [ ] Result analysis functions (`analyze.R`)
- [ ] Plotting functions (`visualize.R`)
- [ ] LaTeX table generation (`tables.R`)
- [ ] Combined workflow function (`workflow.R`)

**Deliverable**: Complete simulation → figures → tables pipeline

### Phase 4: Documentation and Testing (Weeks 7-8)

**Goal**: Production-ready package

- [ ] Comprehensive documentation
- [ ] Vignettes (5 total)
- [ ] pkgdown website
- [ ] CI/CD workflows
- [ ] Integration tests with medfit, probmed

**Deliverable**: CRAN-ready package

### Phase 5: Advanced Features (Weeks 9-10)

**Goal**: Power-user features

- [ ] Adaptive sample size determination
- [ ] Sequential monitoring (stop early if converged)
- [ ] Distributed computing (future/clustermq)
- [ ] Interactive dashboards (shiny)

**Deliverable**: Advanced simulation capabilities

---

## Dependencies

### Required

```
Depends: R (>= 4.1.0)
Imports:
    parallel,
    foreach,
    doParallel,
    pbapply,
    ggplot2,
    dplyr,
    tidyr
Suggests:
    medfit (>= 0.1.0),
    probmed,
    RMediation,
    medrobust,
    testthat (>= 3.0.0),
    knitr,
    rmarkdown
```

### Optional (for advanced features)

```
Suggests:
    future,           # Distributed computing
    clustermq,        # HPC job submission
    renv,             # Reproducibility
    here,             # Path management
    tictoc,           # Timing
    cli,              # Pretty console output
    shiny             # Interactive dashboards
```

---

## Benefits for Ecosystem

### 1. Reduced Code Duplication

**Before** (each package reimplements):
- medfit: 200 lines of simulation code
- probmed: 250 lines
- RMediation: 180 lines
- medrobust: 220 lines
- **Total: 850 lines** of duplicated functionality

**After** (shared infrastructure):
- medsim: 1,200 lines (comprehensive, tested)
- Each package: 20-30 lines (just method definition)
- **Total: 1,300 lines** (40% reduction, better quality)

### 2. Consistent APIs

All simulation studies use same interface:

```r
# medfit simulation
medfit_results <- medsim_run(method = medfit_extract, ...)

# probmed simulation
probmed_results <- medsim_run(method = probmed_compute, ...)

# Results have identical structure → easy comparison
```

### 3. Cross-Package Comparisons

```r
# Compare methods from different packages
methods <- list(
  pmed = probmed::compute_pmed,
  dop = RMediation::medci_dop,
  bounds = medrobust::compute_bounds
)

results <- medsim_compare_methods(methods, scenarios)
medsim_comparison_table(results)
#>           Accuracy  Coverage  Power  Runtime
#> P_med     0.95      0.948     0.82   3.2s
#> DOP CI    0.94      0.952     0.81   0.1s
#> Bounds    0.93      0.945     0.78   1.5s
```

### 4. Publication-Ready Output

Single function generates everything needed for manuscript:

```r
medsim_manuscript_output(
  results,
  output_dir = "manuscript",
  format = "latex"
)
#> Generated:
#>   manuscript/figure_main_panel.pdf (4-panel figure)
#>   manuscript/table_accuracy.tex
#>   manuscript/table_timing.tex
#>   manuscript/results_section.tex (data-driven narrative)
```

### 5. Reproducibility

```r
# Save complete simulation environment
medsim_save_environment(
  results,
  file = "simulation_archive.rds"
)
#> Saved:
#>   - Results
#>   - Configuration
#>   - Session info
#>   - Package versions
#>   - Random seed
#>   - System info

# Reproduce later
medsim_reproduce("simulation_archive.rds")
```

---

## Examples

### Example 1: Basic Mediation Accuracy Study

```r
library(medsim)
library(medfit)

# Define method to test
my_method <- function(data, params) {
  fit_m <- lm(M ~ X, data = data)
  fit_y <- lm(Y ~ X + M, data = data)
  med <- medfit::extract_mediation(fit_m, model_y = fit_y,
                                    treatment = "X", mediator = "M")
  list(indirect = med@a_path * med@b_path)
}

# Run complete workflow
medsim_workflow(
  method = my_method,
  scenarios = medsim_scenarios_mediation(),
  config = medsim_config("local"),
  output_dir = "results"
)
```

### Example 2: Coverage Study

```r
# Test CI coverage
compute_with_ci <- function(data, params) {
  med <- medfit::extract_mediation(...)
  boot <- medfit::bootstrap_mediation(med, statistic = \(x) x@a_path * x@b_path)

  list(
    estimate = boot@estimate,
    ci_lower = boot@ci_lower,
    ci_upper = boot@ci_upper,
    truth = params$true_indirect  # Passed from scenario
  )
}

results <- medsim_run(
  method = compute_with_ci,
  scenarios = scenarios,
  config = medsim_config(n_replications = 1000)  # Need many reps
)

# Analyze coverage
summary <- medsim_analyze_coverage(results, nominal_level = 0.95)
print(summary$coverage_by_scenario)
```

### Example 3: Power Analysis

```r
# Vary sample size to assess power
scenarios <- medsim_scenarios_mediation()

# Add sample size variation
scenarios_with_n <- medsim_add_parameter_grid(
  scenarios,
  n = c(50, 100, 200, 500, 1000)
)

results <- medsim_run(
  method = my_method,
  scenarios = scenarios_with_n,
  config = medsim_config("local")
)

# Plot power curves
medsim_plot_power_curve(
  results,
  x = "n",
  group = "scenario",
  alpha = 0.05
)
```

### Example 4: Cluster Submission

```bash
#!/bin/bash
#SBATCH --job-name=medsim
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --cpus-per-task=20
#SBATCH --time=24:00:00
#SBATCH --mem=64GB

module load R/4.3.0

Rscript run_simulation.R cluster
```

```r
# run_simulation.R
library(medsim)

# Auto-detects cluster environment
config <- medsim_config("auto")  # Will use "cluster" mode

results <- medsim_run(
  method = my_method,
  scenarios = scenarios,
  config = config
)

# Results automatically saved
```

---

## Open Questions / Decisions

### 1. Package Namespace

**Options**:
- `medsim` (simple, clear)
- `medsimulate` (more explicit)
- `medsims` (plural)
- `mediationsim` (full name, longer)

**Recommendation**: `medsim` (concise, consistent with medfit/probmed/medrobust naming)

### 2. Dependency on Mediation Packages

**Options**:

**A. Suggests all mediation packages** (recommended):
```
Suggests:
    medfit,
    probmed,
    RMediation,
    medrobust
```
- Pro: medsim can be used independently
- Pro: No circular dependencies during development
- Con: Examples/vignettes must check for package availability

**B. Imports medfit only**:
```
Imports:
    medfit
Suggests:
    probmed,
    RMediation,
    medrobust
```
- Pro: Can use medfit classes directly
- Pro: medfit is foundation package
- Con: Creates dependency chain

**Recommendation**: Option A (Suggests all) for maximum flexibility

### 3. Relationship to mediationverse

**medsim** could be:

**A. Part of mediationverse** (loads with other packages):
```r
library(mediationverse)
# Now have: medfit, probmed, RMediation, medrobust, medsim
```

**B. Separate tool** (load explicitly):
```r
library(mediationverse)  # Analysis packages
library(medsim)          # Simulation infrastructure
```

**Recommendation**: Option B - Keep medsim separate. Most users doing applied analysis don't need simulation tools. Developers and methodologists will explicitly load medsim.

### 4. HPC Scheduler Support

**Support for**:
- SLURM (most common)
- PBS/Torque
- LSF
- SGE

**Recommendation**: Start with SLURM (product-of-three uses this), add others based on user requests.

---

## Success Metrics

### Technical Metrics

- [ ] Code coverage ≥ 85%
- [ ] All tests pass on local and cluster
- [ ] Works on Windows, Mac, Linux
- [ ] Documented with 5+ vignettes
- [ ] < 5 dependencies beyond base R

### Adoption Metrics

- [ ] Used by all 4 core packages (medfit, probmed, RMediation, medrobust)
- [ ] At least 3 published simulation studies use medsim
- [ ] GitHub stars > 20
- [ ] Mentioned in mediation analysis courses/workshops

### Impact Metrics

- [ ] Reduces simulation code duplication by >50%
- [ ] Enables cross-package method comparisons
- [ ] Simplifies running simulations on HPC (no custom scripts)

---

## Timeline

| Phase | Duration | Deliverable | Quarter |
|-------|----------|-------------|---------|
| 0. Design | 2 weeks | This proposal | Q1 2025 |
| 1. Core | 2 weeks | Basic runner | Q2 2025 |
| 2. Mediation | 2 weeks | Standard scenarios | Q2 2025 |
| 3. Analysis | 2 weeks | Visualization | Q2 2025 |
| 4. Docs | 2 weeks | CRAN-ready | Q3 2025 |
| 5. Advanced | 2 weeks | Power features | Q3 2025 |

**Target Release**: August 2025 (Q3 2025)

**Prerequisites**:
- medfit MVP complete (Phase 3-4)
- probmed stable API

---

## Related Work

### Existing Simulation Packages

**simstudy**: Data generation for simulation studies
- Focus: Synthetic data generation
- Limitation: No parallel processing, no HPC support
- **medsim advantage**: Built-in parallelization, cluster support

**SimDesign**: Comprehensive simulation framework
- Focus: General-purpose simulation
- Limitation: Complex API, steep learning curve
- **medsim advantage**: Mediation-specific, simpler API

**rsimsum**: Simulation summary statistics
- Focus: Summarizing existing simulation results
- Limitation: No execution framework
- **medsim advantage**: End-to-end (execution + analysis)

**faux**: Generate correlated data
- Focus: Data generation only
- Limitation: No simulation study infrastructure
- **medsim advantage**: Complete workflow

**MonteCarlo**: Run simulation studies in parallel
- Focus: Parallel execution
- Limitation: No analysis, visualization, or HPC support
- **medsim advantage**: Complete pipeline + cluster support

### Unique Value of medsim

1. **Mediation-specific** scenarios and analyses
2. **HPC cluster support** out-of-the-box
3. **Publication-ready output** (figures + tables + LaTeX)
4. **Integrated with ecosystem** (medfit, probmed, RMediation, medrobust)
5. **Config-driven** (test/local/cluster modes)
6. **Ground truth caching** (avoid recomputation)
7. **Complete workflow** (single function: simulation → manuscript)

---

## Conclusion

**medsim** fills a critical gap in the mediation analysis ecosystem by providing:

1. **Standardized infrastructure** for simulation studies
2. **Seamless local-to-cluster scaling**
3. **Reduced code duplication** across packages
4. **Publication-ready output** generation
5. **Reproducibility** by design

By learning from the excellent patterns in the product-of-three simulation, medsim will enable researchers to:
- Run rigorous simulation studies with minimal code
- Compare methods across packages consistently
- Scale from laptop to HPC cluster effortlessly
- Generate manuscript-ready figures and tables automatically

**Next Steps**:
1. Review and approve this proposal
2. Create package skeleton (`usethis::create_package("medsim")`)
3. Begin Phase 1 implementation (Core Infrastructure)
4. Update `mediationverse` meta-package to include medsim (as Suggests)

---

**Document Status**: Proposal (awaiting approval)
**Prepared by**: Claude Code (based on product-of-three simulation patterns)
**Date**: December 3, 2025
