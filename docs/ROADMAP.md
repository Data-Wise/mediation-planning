# mediationverse Development Roadmap

> **Last Updated**: 2026-05-09
> **Status**: Phase 2 actively underway; ecosystem hardening complete

This is the **master roadmap** for the mediationverse ecosystem. For user-facing overview, see [mediationverse package roadmap](https://data-wise.github.io/mediationverse/articles/roadmap.html).

---

## Quick Status

```
ACTUAL STATUS (2026-05-09):
✅ Phase 1: Foundation         100% Complete
🚧 Phase 2: Integration         In progress — open PRs across 3 repos
⏸️  Phase 3: Refinement         Q3 2026 (slipped from Q1)
⏸️  Phase 4: Release            Q4 2026 (slipped from Q2)
```

**Schedule status**: Foundation finished ahead of plan; Integration phase started Dec 2025 but slipped through Q1-Q2 2026 due to ecosystem-wide tooling work (CI hardening, branch hygiene, dependency model fixes).

### Open PRs (2026-05-09)

- [Data-Wise/medsim#1](https://github.com/Data-Wise/medsim/pull/1) — `Remotes:` field for GitHub-only sibling deps (fixes failing R-CMD-check)
- [Data-Wise/medrobust#1](https://github.com/Data-Wise/medrobust/pull/1) — Add R-CMD-check workflow + Windows quarto fix
- [Data-Wise/mediationverse#2](https://github.com/Data-Wise/mediationverse/pull/2) — Imports → Suggests for specialty packages (✅ all CI green)

### 2026-05-09 Ecosystem Hardening (Just Completed)

- ✅ Branch protection added to all 5 mediation repos (PR required, no force, no delete)
- ✅ Required `ubuntu-latest (release)` status check on medfit, mediationverse, probmed
- ✅ medrobust: branch renamed from `claude/check-measurement-error-...` to conventional `main`
- ✅ medrobust: `dev` branch created (was missing)
- ✅ Global git workflow rules (`~/.claude/CLAUDE.md`) aligned with `dev-tools/craft` as source of truth
- ✅ medfit CRAN readiness assessment completed — 4-6 hr work + vignette decision pending

---

## Timeline Overview

```
Original Plan:
Q4 2025 (Current)    Q1 2026              Q2 2026              Q3 2026
    |                    |                    |                    |
    v                    v                    v                    v
[Foundation]         [Integration]        [Refinement]         [Release]

ACTUAL STATUS:
Q4 2025                                   Q1 2026              Q2 2026
    |                                         |                    |
    v                                         v                    v
[Foundation ✅]                          [Refinement]         [Release]
[Integration 🚧 READY NOW!]
```

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

## Phase 2: Integration (Started Dec 2025) - 🚧 **IN PROGRESS**

**Status (2026-05-09)**: medfit v0.1.0 complete and tarball-installable. Ecosystem-wide hardening done (branch protection, CI alignment, dependency model). Next: integration testing across packages and merging the open PRs (medsim Remotes:, medrobust R-CMD-check, mediationverse Imports→Suggests).

### Priority 1: probmed Integration

**Tasks:**
- [ ] Add medfit to DESCRIPTION (Imports)
- [ ] Replace extraction code with `medfit::extract_mediation()`
- [ ] Replace bootstrap with `medfit::bootstrap_mediation()`
- [ ] Re-export medfit generics (`nie()`, `nde()`, `te()`, `pm()`, `paths()`)
- [ ] Add `pmed()` extractor for P_med effect size
- [ ] Update formula interface to use medfit
- [ ] Verify backward compatibility
- [ ] Update all tests and vignettes
- [ ] Integration tests (probmed + medfit workflow)

**Estimate**: 4-6 hours
**Blockers**: None - medfit is ready

---

### Priority 2: RMediation Integration

**Tasks:**
- [ ] Add medfit to DESCRIPTION (Imports)
- [ ] Use `medfit::extract_mediation()` for lavaan
- [ ] Leverage medfit bootstrap utilities
- [ ] Update `ci()` function for MediationData class
- [ ] Maintain CRAN stability (careful testing)
- [ ] Update documentation
- [ ] Version bump to 1.5.0

**Estimate**: 3-4 hours
**Blockers**: None - RMediation already CRAN stable

---

### Priority 3: medrobust Integration (Optional)

**Tasks:**
- [ ] Add medfit to DESCRIPTION (Suggests)
- [ ] Use for naive estimates if beneficial
- [ ] Document integration patterns
- [ ] Add short aliases (`sens()`, `bounds()`, `power()`)

**Estimate**: 2-3 hours
**Blockers**: None

---

### Deliverables (Phase 2)
- medfit 0.1.0 release ✅ (ready pending final merge)
- probmed 0.2.0 with medfit integration
- RMediation 1.5.0 with medfit integration
- Updated integration tests across packages

**Timeline**: Q4 2025 (Dec 2025 - Jan 2026)

---

## Phase 3: Refinement (Q1 2026)

### Objectives
Polish packages for CRAN submission and public release.

### Milestones

#### Package Polishing
- [ ] Address all R CMD check notes/warnings
- [ ] Comprehensive testing on multiple platforms (win-builder, rhub)
- [ ] Performance optimization
- [ ] Memory profiling and optimization
- [ ] Security audit

#### Documentation Enhancement
- [ ] Complete API documentation
- [ ] Add more examples
- [ ] Create video tutorials (optional)
- [ ] Write blog posts
- [ ] Prepare manuscript drafts

#### mediationverse Meta-Package Enhancement
- [ ] Update attachment logic for new medfit API
- [ ] Enhanced conflict detection
- [ ] Improved startup message with version info
- [ ] Comprehensive vignettes (getting-started, workflow)
- [ ] pkgdown website updates

#### User Testing
- [ ] Beta testing with research collaborators
- [ ] Gather feedback on API design
- [ ] Address usability issues
- [ ] Create FAQ documentation

### Deliverables
- All packages pass R CMD check on CRAN platforms
- Complete documentation
- mediationverse meta-package polished
- User feedback incorporated

**Timeline**: Q1 2026 (Jan-Mar 2026)

---

## Phase 4: Release (Q2 2026)

### Objectives
Launch packages to CRAN and announce publicly.

### Milestones

#### CRAN Submissions
- [ ] Submit medfit to CRAN
- [ ] Submit probmed to CRAN (update)
- [ ] Submit medrobust to CRAN (new)
- [ ] Submit mediationverse to CRAN (update)
- [ ] Address reviewer feedback
- [ ] Coordinate releases

**Order**: medfit → probmed → medrobust → mediationverse

#### Public Launch
- [ ] Announcement blog post
- [ ] Social media campaign (Twitter/Mastodon)
- [ ] R-bloggers post
- [ ] Conference presentations (JSM, ENAR)
- [ ] Workshop materials

#### Long-term Maintenance
- [ ] Establish release schedule (quarterly)
- [ ] Set up community guidelines
- [ ] Create issue templates
- [ ] Establish code review process
- [ ] Plan future enhancements

### Deliverables
- All packages on CRAN
- Public announcement
- Workshop materials
- Maintenance plan

**Timeline**: Q2 2026 (Apr-Jun 2026)

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

---

**Next Review**: End of Q3 2026 (Sep 30, 2026)

**See also:**
- [PROJECT-HUB.md](../PROJECT-HUB.md) - Coordination dashboard
- [TODOS.md](../TODOS.md) - Active task tracking
- [IDEAS.md](../IDEAS.md) - Future enhancements
- [mediationverse roadmap](https://data-wise.github.io/mediationverse/articles/roadmap.html) - User-facing overview
