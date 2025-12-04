# Standard Badges for Mediationverse Packages

## Standard Badge Order

All mediationverse packages should use badges in this order:

1. **Lifecycle** - Development stage
2. **Repo Status** - Repository activity status
3. **R-CMD-check** - Build status
4. **Website Status** (pkgdown) - Documentation build
5. **R-hub** - Multi-platform testing
6. **Codecov** - Code coverage
7. **CRAN status** - Only if package is on CRAN

## Badge Template

### For Packages NOT on CRAN

```markdown
<!-- badges: start -->
[![Lifecycle: {status}](https://img.shields.io/badge/lifecycle-{status}-{color}.svg)](https://lifecycle.r-lib.org/articles/stages.html#{status})
[![Repo Status](https://www.repostatus.org/badges/latest/{repostatus}.svg)](https://www.repostatus.org/#{repostatus})
[![R-CMD-check](https://github.com/data-wise/{package}/actions/workflows/R-CMD-check.yaml/badge.svg)](https://github.com/data-wise/{package}/actions/workflows/R-CMD-check.yaml)
[![Website Status](https://github.com/data-wise/{package}/actions/workflows/pkgdown.yaml/badge.svg)](https://github.com/data-wise/{package}/actions/workflows/pkgdown.yaml)
[![R-hub](https://github.com/data-wise/{package}/actions/workflows/rhub.yaml/badge.svg)](https://github.com/data-wise/{package}/actions/workflows/rhub.yaml)
[![Codecov](https://codecov.io/gh/data-wise/{package}/graph/badge.svg)](https://codecov.io/gh/data-wise/{package})
<!-- badges: end -->
```

### For Packages ON CRAN

```markdown
<!-- badges: start -->
[![CRAN status](https://www.r-pkg.org/badges/version/{package})](https://CRAN.R-project.org/package={package})
[![Lifecycle: stable](https://img.shields.io/badge/lifecycle-stable-brightgreen.svg)](https://lifecycle.r-lib.org/articles/stages.html#stable)
[![R-CMD-check](https://github.com/data-wise/{package}/actions/workflows/R-CMD-check.yaml/badge.svg)](https://github.com/data-wise/{package}/actions/workflows/R-CMD-check.yaml)
[![Website Status](https://github.com/data-wise/{package}/actions/workflows/pkgdown.yaml/badge.svg)](https://github.com/data-wise/{package}/actions/workflows/pkgdown.yaml)
[![R-hub](https://github.com/data-wise/{package}/actions/workflows/rhub.yaml/badge.svg)](https://github.com/data-wise/{package}/actions/workflows/rhub.yaml)
[![Codecov](https://codecov.io/gh/data-wise/{package}/graph/badge.svg)](https://codecov.io/gh/data-wise/{package})
<!-- badges: end -->
```

## Badge Options

### Lifecycle Status

| Badge | When to Use |
|-------|-------------|
| `experimental` (orange) | Active development, API may change |
| `stable` (green) | Production-ready, stable API |
| `superseded` (blue) | Replaced by newer package |
| `deprecated` (orange) | No longer recommended |

**Usage:**
```markdown
[![Lifecycle: experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://lifecycle.r-lib.org/articles/stages.html#experimental)
```

### Repo Status

| Badge | When to Use |
|-------|-------------|
| `wip` | Work in Progress, not ready for use |
| `active` | Active development and maintenance |
| `inactive` | No active development but maintained |
| `unsupported` | No longer maintained |

**Usage:**
```markdown
[![Repo Status](https://www.repostatus.org/badges/latest/wip.svg)](https://www.repostatus.org/#wip)
```

## Current Mediationverse Package Badges

### medfit
```markdown
[![Lifecycle: experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://lifecycle.r-lib.org/articles/stages.html#experimental)
[![Repo Status](https://www.repostatus.org/badges/latest/wip.svg)](https://www.repostatus.org/#wip)
[![R-CMD-check](https://github.com/data-wise/medfit/actions/workflows/R-CMD-check.yaml/badge.svg)](https://github.com/data-wise/medfit/actions/workflows/R-CMD-check.yaml)
[![Website Status](https://github.com/data-wise/medfit/actions/workflows/pkgdown.yaml/badge.svg)](https://github.com/data-wise/medfit/actions/workflows/pkgdown.yaml)
[![R-hub](https://github.com/data-wise/medfit/actions/workflows/rhub.yaml/badge.svg)](https://github.com/data-wise/medfit/actions/workflows/rhub.yaml)
[![Codecov](https://codecov.io/gh/data-wise/medfit/graph/badge.svg)](https://codecov.io/gh/data-wise/medfit)
```

### probmed
```markdown
[![Lifecycle: stable](https://img.shields.io/badge/lifecycle-stable-brightgreen.svg)](https://lifecycle.r-lib.org/articles/stages.html#stable)
[![Repo Status](https://www.repostatus.org/badges/latest/active.svg)](https://www.repostatus.org/#active)
[![R-CMD-check](https://github.com/data-wise/probmed/actions/workflows/R-CMD-check.yaml/badge.svg)](https://github.com/data-wise/probmed/actions/workflows/R-CMD-check.yaml)
[![Website Status](https://github.com/data-wise/probmed/actions/workflows/pkgdown.yaml/badge.svg)](https://github.com/data-wise/probmed/actions/workflows/pkgdown.yaml)
[![R-hub](https://github.com/data-wise/probmed/actions/workflows/rhub.yaml/badge.svg)](https://github.com/data-wise/probmed/actions/workflows/rhub.yaml)
[![Codecov](https://codecov.io/gh/data-wise/probmed/graph/badge.svg)](https://codecov.io/gh/data-wise/probmed)
```

### RMediation (ON CRAN)
```markdown
[![CRAN status](https://www.r-pkg.org/badges/version/RMediation)](https://CRAN.R-project.org/package=RMediation)
[![Lifecycle: stable](https://img.shields.io/badge/lifecycle-stable-brightgreen.svg)](https://lifecycle.r-lib.org/articles/stages.html#stable)
[![R-CMD-check](https://github.com/data-wise/rmediation/actions/workflows/R-CMD-check.yaml/badge.svg)](https://github.com/data-wise/rmediation/actions/workflows/R-CMD-check.yaml)
[![Website Status](https://github.com/data-wise/rmediation/actions/workflows/pkgdown.yaml/badge.svg)](https://github.com/data-wise/rmediation/actions/workflows/pkgdown.yaml)
[![R-hub](https://github.com/data-wise/rmediation/actions/workflows/rhub.yaml/badge.svg)](https://github.com/data-wise/rmediation/actions/workflows/rhub.yaml)
[![Codecov](https://codecov.io/gh/data-wise/rmediation/graph/badge.svg)](https://codecov.io/gh/data-wise/rmediation)
```

### medrobust
```markdown
[![Lifecycle: experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://lifecycle.r-lib.org/articles/stages.html#experimental)
[![Repo Status](https://www.repostatus.org/badges/latest/wip.svg)](https://www.repostatus.org/#wip)
[![R-CMD-check](https://github.com/data-wise/medrobust/actions/workflows/R-CMD-check.yaml/badge.svg)](https://github.com/data-wise/medrobust/actions/workflows/R-CMD-check.yaml)
[![Website Status](https://github.com/data-wise/medrobust/actions/workflows/pkgdown.yaml/badge.svg)](https://github.com/data-wise/medrobust/actions/workflows/pkgdown.yaml)
[![R-hub](https://github.com/data-wise/medrobust/actions/workflows/rhub.yaml/badge.svg)](https://github.com/data-wise/medrobust/actions/workflows/rhub.yaml)
[![Codecov](https://codecov.io/gh/data-wise/medrobust/graph/badge.svg)](https://codecov.io/gh/data-wise/medrobust)
```

### medsim
```markdown
[![Lifecycle: experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://lifecycle.r-lib.org/articles/stages.html#experimental)
[![Repo Status](https://www.repostatus.org/badges/latest/wip.svg)](https://www.repostatus.org/#wip)
[![R-CMD-check](https://github.com/data-wise/medsim/actions/workflows/R-CMD-check.yaml/badge.svg)](https://github.com/data-wise/medsim/actions/workflows/R-CMD-check.yaml)
[![Website Status](https://github.com/data-wise/medsim/actions/workflows/pkgdown.yaml/badge.svg)](https://github.com/data-wise/medsim/actions/workflows/pkgdown.yaml)
[![R-hub](https://github.com/data-wise/medsim/actions/workflows/rhub.yaml/badge.svg)](https://github.com/data-wise/medsim/actions/workflows/rhub.yaml)
[![Codecov](https://codecov.io/gh/data-wise/medsim/graph/badge.svg)](https://codecov.io/gh/data-wise/medsim)
```

### mediationverse
```markdown
[![Lifecycle: experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://lifecycle.r-lib.org/articles/stages.html#experimental)
[![Repo Status](https://www.repostatus.org/badges/latest/wip.svg)](https://www.repostatus.org/#wip)
[![R-CMD-check](https://github.com/data-wise/mediationverse/actions/workflows/R-CMD-check.yaml/badge.svg)](https://github.com/data-wise/mediationverse/actions/workflows/R-CMD-check.yaml)
[![Website Status](https://github.com/data-wise/mediationverse/actions/workflows/pkgdown.yaml/badge.svg)](https://github.com/data-wise/mediationverse/actions/workflows/pkgdown.yaml)
[![R-hub](https://github.com/data-wise/mediationverse/actions/workflows/rhub.yaml/badge.svg)](https://github.com/data-wise/mediationverse/actions/workflows/rhub.yaml)
```

## Required GitHub Actions Workflows

All packages should have these workflows:

1. **R-CMD-check.yaml** - Standard R package check
2. **pkgdown.yaml** - Build and deploy documentation website
3. **rhub.yaml** - Multi-platform testing via R-hub
4. **test-coverage.yaml** (optional) - Upload coverage to Codecov

## Setting Up Codecov

1. Go to https://codecov.io
2. Sign in with GitHub
3. Add your repository
4. Copy the upload token (if needed)
5. Add `CODECOV_TOKEN` to repository secrets (if private repo)

The test-coverage.yaml workflow will automatically upload results.

## Best Practices

1. **Keep badges up to date**: Update Lifecycle and Repo Status as development progresses
2. **Fix red badges promptly**: Red badges indicate issues that should be addressed
3. **Use consistent formatting**: Follow the standard order for all packages
4. **Link to actual status pages**: Ensure badges link to meaningful status information
5. **Remove badges for non-existent workflows**: Don't add badges if you haven't set up the workflow

## Monitoring

See [STATUS.md](../packages/mediationverse/STATUS.md) for centralized monitoring of all package badges.
