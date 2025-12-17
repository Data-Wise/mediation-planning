# Ideas for Mediationverse Ecosystem

**Last Updated:** 2025-12-17

A collection of ideas, enhancements, and future directions for the mediationverse ecosystem.

---

## API Design Ideas

### ADHD-Friendly API (Decision Pending)
From `specs/GENERIC-FUNCTIONS-STRATEGY.md`:

| Option | Entry Point | Description |
|--------|-------------|-------------|
| **A** | `med()` only | Single function → tab-complete discovery |
| **B** | `fit_mediation()` | Verb-first pipes (`fit_*`, `get_*`) |
| **C** | Hybrid | `med()` for simple + chains for complex |

**Key functions proposed:**
- `med()` - ONE function to remember
- `quick()` - "Just tell me the answer" mode
- `effects()` - All effects at once
- `nie()`, `nde()`, `te()`, `pm()` - Individual effect extractors

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
