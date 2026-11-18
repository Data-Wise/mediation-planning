# mediationverse Development Roadmap

> **Last Updated**: 2026-11-17 (direction and gates; version and CRAN numbers live in `ECOSYSTEM-MANIFEST.yaml`)
> **Status**: Integration complete; CRAN cascade gated (see Milestone gates)
>
> **Refreshed 2026-11-17.** Direction, gates and the next-release candidates below are current. Phase 1 keeps its original text as history, and Phases 2 to 4 are replaced by milestone gates; version numbers and CRAN state are intentionally not repeated here, so read them from [`ECOSYSTEM-MANIFEST.yaml`](../ECOSYSTEM-MANIFEST.yaml) and each package's `.STATUS`.

This is the **master roadmap** for the mediationverse ecosystem. For user-facing overview, see [mediationverse package roadmap](https://data-wise.github.io/mediationverse/articles/roadmap.html).

---

## Where things stand (2026-11-17)

- **Foundation and Integration are done.** medfit, probmed, RMediation, medrobust, medsim and missingmed are
  developed against the shared medfit infrastructure; each ships GitHub releases (see the manifest for versions).
- **CRAN is the open front.** Only medfit and RMediation are on CRAN, and both CRAN versions are far behind their
  GitHub releases. The rest wait on the gates below.
- **mediationverse** shipped 0.2.0 (registry, citations, sitrep checks), 0.3.0 (provenance, news, attach_all,
  quiet option) and 0.4.0 (update detection) on 2026-11-16 and 2026-11-17, through GitHub and r-universe, not CRAN.

---

## Phase 1: Foundation (Q4 2025) - ✅ **COMPLETE**

### ✅ medfit Phase 1-7 (100% Complete)

**Completed Phases:**
- [x] **Phase 1**: Package setup (CI/CD, documentation, Dependabot)
- [x] **Phase 2**: S7 class architecture (MediationData, SerialMediationData, BootstrapResult)
- [x] **Phase 2.5**: Comprehensive Quarto vignettes (4 articles)
- [x] **Phase 3**: Model extraction (lm/glm, lavaan)
- [x] **Phase 4**: fit_mediation() with GLM engine
- [x] **Phase 5**: bootstrap_mediation() (parametric, nonparametric, plugin)
- [x] **Phase 6**: Generic functions
  - [x] Base R generics: `coef()`, `vcov()`, `confint()`, `nobs()`
  - [x] Effect extractors: `nie()`, `nde()`, `te()`, `pm()`, `paths()`
  - [x] Tidyverse methods: `tidy()`, `glance()`
- [x] **Phase 6.5**: ADHD-friendly API
  - [x] `med()` - One-function mediation analysis
  - [x] `quick()` - One-liner summary output
- [x] **Phase 7**: Polish & CRAN prep (99%)
  - [x] R CMD check clean (1 NOTE - new submission)
  - [x] Spell check (inst/WORDLIST)
  - [x] DESCRIPTION updates (Language, Date)
  - [x] Documentation simplified (ecosystem references)
  - [ ] Final merge dev → main (in progress)
  - [ ] CRAN submission

**Stats:**
- 427 tests passing (0 errors, 0 warnings)
- 100% test success rate
- pkgdown site deployed: https://data-wise.github.io/medfit/
- Branch protection enabled on main

**Repository:** https://github.com/Data-Wise/medfit
**Website:** https://data-wise.github.io/medfit/

---

### ✅ medsim (Complete)
- [x] Core simulation infrastructure
- [x] Environment detection (local/HPC)
- [x] Parallel processing with progress bars
- [x] Standard mediation scenarios
- [x] Publication-ready output

---

### ✅ Documentation (Complete)
- [x] medfit vignettes (4 comprehensive guides)
- [x] probmed vignettes (integration guides)
- [x] Package websites for all packages
- [x] Unified styling (Bootstrap 5 + Litera)
- [x] mediation-planning coordination hub

---

## Phase 2 to 4: replaced by milestone gates (2026-11-17)

The original Phase 2 (Integration), Phase 3 (Refinement) and Phase 4 (Release) text, with its calendar quarters,
was removed: Integration finished, and the release dates slipped past what the project controls. What remains is a
chain of dependencies, not a schedule. Dates are deliberately omitted.

### Milestone gates (CRAN cascade, in order)

1. **probmed submits.** Needs its Remotes dropped, a version bump and a submission; currently on hold after the
   Phase 7 I6 gate failed 2026-10-19 (see probmed `.STATUS`).
2. **medrobust hold lifts.** CRAN-ready, paused until its associated manuscript is submitted; a journal decision,
   not a technical blocker.
3. **medsim submits.** Blocked only on medrobust.
4. **mediationverse submits.** Last: drop the `Remotes:` entries, CRAN-check, submit. Needs probmed, medrobust and
   medsim on CRAN first.

Side gate: **missingmed** needs a newer RMediation on CRAN (win-builder R-devel fails on 1.6.1's `.onLoad`), and
RMediation's own submission is on hold until it and medfit are both mature.

Order in the original plan was medfit, probmed, medrobust, mediationverse; medfit and RMediation reached CRAN
earlier and are no longer gates.

### mediationverse next release (candidates, not a commitment)

0.5.0 has no committed theme. Candidates:

- More docs-only recipes like `vignettes/first-analysis.qmd` (shipped in dev, PR #42), built on `medfit::med()`.
- Revisit a unified `mediate()` or pipeline object only when the exit criteria in
  `mediationverse/planning/DESIGN-mediate-pipeline-2026-11-17.md` are met: no breaking change to `med()` or
  `fit_mediation()` signatures across three consecutive medfit releases, and a shared result interface agreed by a
  sibling package.

---

## Post-Release: Future Enhancements

### medfit Extensions

#### High Priority
- **Delta Method SEs**: Add standard errors for derived effects (nie, pm) in `tidy()`
- **lmer Engine**: Mixed-effects models for multilevel mediation
- **brms Engine**: Bayesian mediation analysis

#### Medium Priority
- **Four-way Decomposition**: VanderWeele interaction support
- **Estimation Engines**: CMAverse adapters (g-formula, IPW)
- **BCa Bootstrap**: Bias-corrected accelerated CIs

#### Future
- **Targeted Learning**: tmle3 adapter
- **Machine Learning**: DoubleML integration
- **Sensitivity Methods**: Built-in sensitivity analysis

---

### New Packages (Tentative)

**Under Consideration:**
- **medcausal**: Causal inference tools
- **medpower**: Power analysis specialized for mediation
- **medviz**: Advanced visualization (DAGs, path diagrams)
- **medML**: Machine learning for mediation

---

### Integration Priorities Timeline

> Indicative order only. The quarters below date from 2026-05 and are not commitments; see the milestone gates above.

| Quarter | Enhancement |
|---------|-------------|
| Q3 2026 | Delta method SEs for derived effects |
| Q4 2026 | lmer engine for multilevel mediation |
| Q1 2027 | CMAverse integration (g-formula, IPW) |
| Q2 2027 | Four-way decomposition |
| Q3 2027 | Targeted learning (tmle3) |
| Q4 2027 | Bayesian methods (brms) |

---

## Success Metrics

### Technical Metrics
- [x] All packages pass R CMD check locally
- [ ] All packages pass on CRAN platforms (win-builder, rhub)
- [x] >90% code coverage (medfit: 100% pass rate)
- [x] <500ms for typical mediation analysis
- [x] Zero critical bugs in production

### Adoption Metrics (Post-CRAN)
- Target: 1000+ downloads in first quarter
- Target: 10+ citations in first year
- Active user community (GitHub stars, issues)
- Positive feedback from beta testers

### Quality Metrics
- [x] Comprehensive documentation (medfit complete)
- [ ] Responsive issue resolution (<48h)
- [ ] Regular releases (quarterly)
- [ ] Active maintenance

---

## Risk Assessment

### Technical Risks
| Risk | Probability | Impact | Mitigation | Status |
|------|-------------|--------|------------|--------|
| API breaking changes | Low | High | Extensive testing, deprecation warnings | ✅ Stable API |
| Performance issues | Low | Medium | Benchmarking, profiling | ✅ Fast |
| CRAN rejection | Medium | High | Early submission, quick feedback response | Pending |
| S7 stability | Low | High | Track S7 development, contribute fixes | ✅ S7 0.1.1 stable |

### Project Risks
| Risk | Probability | Impact | Mitigation | Status |
|------|-------------|--------|------------|--------|
| Delayed integration | Low | Medium | Modular design, parallel development | ✅ Ahead of schedule |
| Scope creep | Low | Medium | Strict MVP definition, phase gates | ✅ Controlled |
| Resource constraints | Low | High | Prioritize core features | ✅ On track |

---

## Communication Plan

### Regular Updates
- **Weekly**: Update mediation-planning/PROJECT-HUB.md
- **Monthly**: Progress updates on GitHub Discussions
- **Quarterly**: Blog posts on major milestones
- **Ad-hoc**: Announce breaking changes early

### Channels
- GitHub Issues/Discussions
- R-bloggers
- Social media (Twitter/Mastodon)
- Mailing list (future)

---

## Key Decisions & Standards

### API Design
See [GENERIC-FUNCTIONS-STRATEGY.md](../specs/GENERIC-FUNCTIONS-STRATEGY.md) for:
- Effect extractor design (nie, nde, te, pm, paths)
- ADHD-friendly API (med, quick)
- Tidyverse integration (tidy, glance)
- Base R generics (coef, vcov, confint, nobs)

### Development Standards
See [DEVELOPMENT-STANDARDS.md](../specs/DEVELOPMENT-STANDARDS.md) for:
- Coding standards
- Testing requirements
- Documentation requirements
- S7 class design patterns

### Branching Strategy
See [BRANCHING-STRATEGY.md](../specs/BRANCHING-STRATEGY.md) for:
- Git workflow (dev → main)
- Branch protection rules
- PR requirements
- Release process

---

## Contact & Feedback

**Project Lead**: Davood Tofighi (dtofighi@gmail.com)

**Provide Feedback**:
- [GitHub Discussions](https://github.com/data-wise/mediationverse/discussions)
- [Feature Requests](https://github.com/data-wise/mediationverse/issues/new)
- Email: dtofighi@gmail.com

**Planning Repository**: https://github.com/data-wise/mediation-planning

---

## Document History

| Date | Update | Author |
|------|--------|--------|
| 2025-12-04 | Initial roadmap created | DT |
| 2025-12-17 | Updated to reflect medfit completion (3 months ahead!) | DT |
| 2026-05-09 | Mid-Phase-2 update: ecosystem hardening (branch protection, CI alignment, dependency model fixes); 3 open PRs; medfit CRAN readiness assessed | DT (with Claude) |
| 2026-11-17 | Refresh: Phase 2 to 4 replaced by milestone gates; mediationverse 0.2.0 to 0.4.0 recorded; 0.5.0 as candidates; versions deferred to the manifest | DT |

---

**Next Review**: when a milestone gate moves (a gate opening or closing), not on a calendar date

**See also:**
- [PROJECT-HUB.md](../PROJECT-HUB.md) - Coordination dashboard
- [TODOS.md](../TODOS.md) - Active task tracking
- [IDEAS.md](../IDEAS.md) - Future enhancements
- [mediationverse roadmap](https://data-wise.github.io/mediationverse/articles/roadmap.html) - User-facing overview
