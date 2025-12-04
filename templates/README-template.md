# README Template for Mediationverse Packages

Copy and customize this template for each package in the ecosystem.

---

# {package}: {tagline}

<!-- badges: start -->
[![R-CMD-check](https://github.com/data-wise/{package}/workflows/R-CMD-check/badge.svg)](https://github.com/data-wise/{package}/actions)
[![Codecov test coverage](https://codecov.io/gh/data-wise/{package}/branch/main/graph/badge.svg)](https://app.codecov.io/gh/data-wise/{package}?branch=main)
[![CRAN status](https://www.r-pkg.org/badges/version/{package})](https://CRAN.R-project.org/package={package})
[![Lifecycle: {lifecycle}](https://img.shields.io/badge/lifecycle-{lifecycle}-{color}.svg)](https://lifecycle.r-lib.org/articles/stages.html#{lifecycle})
<!-- badges: end -->

{Brief 1-2 sentence description of what the package does.}

## Mediationverse Ecosystem

**{package}** is part of the **mediationverse** ecosystem for mediation analysis in R:

| Package | Purpose | Status |
|---------|---------|--------|
| [**medfit**](https://github.com/data-wise/medfit) | Foundation: model fitting, extraction, bootstrap | Core |
| [**probmed**](https://github.com/data-wise/probmed) | Probabilistic effect size (P_med) | Application |
| [**RMediation**](https://github.com/data-wise/rmediation) | Confidence intervals (DOP, MBCO) | Application |
| [**medrobust**](https://github.com/data-wise/medrobust) | Sensitivity analysis | Application |
| [**medsim**](https://github.com/data-wise/medsim) | Simulation infrastructure | Support |

{For dependent packages, add:}
> This package builds on **medfit** for model infrastructure and bootstrap inference.

## Installation

```r
# Install from CRAN (when available)
install.packages("{package}")

# Install development version from GitHub
# install.packages("remotes")
remotes::install_github("data-wise/{package}")
```

{For dependent packages, add:}
```r
# Install with all dependencies
remotes::install_github(c(
  "data-wise/medfit",
  "data-wise/{package}"
))
```

## Quick Start

```r
library({package})

# {Package-specific example code}
# Show the main workflow in 5-10 lines
```

## Features

- {Feature 1}
- {Feature 2}
- {Feature 3}

## Documentation

- [Getting Started](https://{package}.r-universe.dev/articles/getting-started)
- [Full Documentation](https://data-wise.github.io/{package}/)
- [Function Reference](https://data-wise.github.io/{package}/reference/)

## Compatibility

- **R version**: >= 4.1.0
{For dependent packages:}
- **medfit**: >= {version}

## Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

For ecosystem-wide changes, see the [Ecosystem Coordination Guide](https://github.com/data-wise/medfit/blob/main/planning/ECOSYSTEM.md).

## Citation

If you use {package} in your research, please cite:

```
@software{{package},
  author = {{Author Name}},
  title = {{{package}: {Title}}},
  year = {{Year}},
  url = {{https://github.com/data-wise/{package}}}
}
```

## License

MIT (c) {Year} {Author}

---

## Template Customization Notes

### Replace These Placeholders

| Placeholder | Description |
|-------------|-------------|
| `{package}` | Package name (lowercase) |
| `{tagline}` | Short description (e.g., "Probabilistic Mediation Effects") |
| `{lifecycle}` | One of: experimental, stable, deprecated |
| `{color}` | Badge color: blue (experimental), green (stable), orange (deprecated) |
| `{version}` | Minimum required medfit version |
| `{Year}` | Current year |
| `{Author}` | Package author |

### Package-Specific Sections

**medfit** (foundation):
- Emphasize S7 classes, extraction methods, bootstrap
- No dependency section needed

**probmed/RMediation/medrobust** (applications):
- Emphasize what they add on top of medfit
- Include medfit dependency in installation

**medsim** (support):
- Emphasize simulation framework, HPC support
- Show example with method comparison

**mediationverse** (meta-package):
- Show how to load all packages at once
- Emphasize convenience, not direct use in other packages
