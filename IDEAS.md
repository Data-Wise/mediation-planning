# Ideas for Mediationverse Ecosystem

**Last Updated:** 2025-12-17

A collection of ideas, enhancements, and future directions for the mediationverse ecosystem.

---

## Implementation Ready (No Blockers)

### Phase 1: Base R Generics (medfit) - 30 min
```r
# Add to medfit/R/methods-base.R
method(coef, MediationData) <- function(object, type = "paths", ...)
method(vcov, MediationData) <- function(object, ...)
method(confint, MediationData) <- function(object, parm, level = 0.95, ...)
method(nobs, MediationData) <- function(object, ...)
```

### Phase 2: Effect Extractors (medfit) - 1 hr
```r
# Add to medfit/R/generics.R + methods-effects.R
nie <- new_generic("nie", "x")
nde <- new_generic("nde", "x")
te <- new_generic("te", "x")
pm <- new_generic("pm", "x")
paths <- new_generic("paths", "x")
```

### Phase 3: ADHD Entry Point (medfit) - 30 min
```r
# Simple wrapper in medfit/R/med.R
med <- function(data, treatment, mediator, outcome, ...) {

  fit_mediation(
    formula_y = as.formula(paste(outcome, "~", treatment, "+", mediator)),
    formula_m = as.formula(paste(mediator, "~", treatment)),
    data = data, treatment = treatment, mediator = mediator, ...
  )
}

quick <- function(x, ...) {
  # One-line summary: NIE = 0.20 [0.15, 0.25], p < 0.001
}
```

---

## API Design (Decided - Option C Hybrid)

| Layer | Functions | Use Case |
|-------|-----------|----------|
| **Simple** | `med()`, `quick()` | 80% of analyses |
| **Standard** | `fit_mediation()`, `bootstrap_mediation()` | Full control |
| **Extract** | `nie()`, `nde()`, `te()`, `pm()`, `paths()` | Specific values |
| **Base R** | `coef()`, `vcov()`, `confint()`, `summary()` | R conventions |
| **Tidy** | `tidy()`, `glance()` | Tibble output |

---

## Coordination Hub Ideas

### Status Aggregation
- [ ] `medstatus` - CLI tool to aggregate all `.STATUS` files
- [ ] `/ecosystem` - Claude Code slash command for live dashboard
- [ ] Git hooks to auto-sync status to central hub

### ADHD-Friendly Workflow
- [ ] `mednow` - "What should I do now?" suggestion system
- [ ] Daily auto-generated focus card
- [ ] Context breadcrumbs across packages

### Change Coordination
- [ ] RFC system for cross-package changes
- [ ] Ecosystem-wide GitHub Issues
- [ ] Change notification system (`medannounce`)

---

## Package Enhancement Ideas

### medfit (Foundation)
- [ ] `med()` entry point function
- [ ] `quick()` instant results function
- [ ] lmer engine for mixed-effects models
- [ ] brms engine for Bayesian inference
- [ ] BCa bootstrap confidence intervals
- [ ] OpenMx engine for SEM

### probmed
- [ ] More distribution support
- [ ] Expanded vignettes
- [ ] Integration with medrobust for sensitivity

### RMediation
- [ ] Integration with medfit MediationData objects
- [ ] Expanded tidy methods

### medrobust
- [ ] Short aliases: `sens()`, `bounds()`, `power()`
- [ ] Integration with medfit workflow
- [ ] Interactive sensitivity plots (plotly)

### medsim
- [ ] Standard simulation scenarios for all packages
- [ ] Automated benchmarking suite
- [ ] Publication-ready table generation

### mediationverse
- [ ] `mediationverse_sitrep()` - Ecosystem situation report
- [ ] Unified vignette across all packages
- [ ] Starter templates for common analyses

---

## Documentation Ideas

- [ ] "Choose Your Own Adventure" vignette (decision tree)
- [ ] Video tutorials for each package
- [ ] Cheat sheet / reference card
- [ ] Case study gallery with real data examples

---

## Tooling Ideas

### Shell Commands
```bash
medstatus        # Aggregate status across packages
mednow           # Suggest next task
medcheck         # Ecosystem health check
medswitch <pkg>  # Switch context with breadcrumbs
medannounce      # Broadcast breaking changes
```

### Claude Code Integration
```
/ecosystem       # Live dashboard
/status <pkg>    # Single package status
/next            # What to work on next
/decisions       # Pending design decisions
```

---

## Research Ideas

### Methods to Implement
- [ ] Sequential mediation with k mediators
- [ ] Parallel mediation
- [ ] Moderated mediation
- [ ] Bayesian mediation
- [ ] Multilevel mediation

### Papers to Write
- [ ] Sequential mediation framework (target: JASA)
- [ ] Sensitivity analysis methods (target: Biostatistics)
- [ ] Product-of-coefficients extensions (target: Psychological Methods)

---

## Wild Ideas (Long-term)

- [ ] Shiny app for interactive mediation analysis
- [ ] VSCode extension for mediationverse
- [ ] Online mediation analysis calculator
- [ ] Integration with causalverse ecosystem
- [ ] Python port (pymediation)
- [ ] Teaching materials / course integration

---

## Prioritization

### High Priority (Next Month)
1. Approve API design decisions (7 items)
2. Implement `med()` entry point in medfit
3. Create `medstatus` aggregator script

### Medium Priority (Q1 2025)
1. RFC system for governance
2. `mednow` suggestion system
3. Complete medfit engines (lmer, brms)

### Low Priority (Future)
1. Shiny app
2. Python port
3. Video tutorials

---

*Add new ideas by editing this file or creating issues in the GitHub repo.*
