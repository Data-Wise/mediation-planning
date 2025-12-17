# Mediationverse Ecosystem Coordination

This document provides coordination guidelines for the mediationverse multi-package ecosystem.

---

## Package Dependency Graph

```
mediationverse (meta-package)
|
+-- Imports all packages for user convenience
|
+------------------------------------------------------------------+
|                                                                  |
v                                                                  v
medfit (foundation)                                           medsim (simulation)
|   - S7 classes: MediationData, SerialMediationData              |
|   - Functions: extract_mediation(), fit_mediation(),            |
|                bootstrap_mediation()                            |
|                                                                  |
+----------------+----------------+----------------+               |
|                |                |                |               |
v                v                v                v               |
probmed      RMediation      medrobust      (future packages)      |
- P_med        - DOP/MBCO CIs   - Sensitivity     ...              |
- Uses medfit  - Uses medfit    - Suggests medfit                  |
               extraction       for naive est.                     |
                                                                   |
<------------------------------------------------------------------+
      medsim can test any mediation method
```

---

## Version Compatibility Matrix

| medfit | probmed | RMediation | medrobust | medsim | Status |
|--------|---------|------------|-----------|--------|--------|
| 0.1.x  | 0.2.x   | 1.5.x      | 0.2.x     | 0.1.x  | Development (current) |
| 0.2.x  | 0.3.x   | 1.6.x      | 0.3.x     | 0.2.x  | Planned: lmer support |
| 1.0.x  | 1.0.x   | 2.0.x      | 1.0.x     | 1.0.x  | Stable release target |

### Current Versions (December 2025)

| Package | Version | Branch | Status |
|---------|---------|--------|--------|
| medfit | 0.1.0.9000 | main | Phase 4 (fitting) in progress |
| probmed | 0.2.0 | main | Awaiting medfit integration |
| RMediation | 1.5.0 | main | Awaiting medfit integration |
| medrobust | 0.2.0 | main | In development |
| medsim | 0.0.0.9000 | main | Phase 2 (visualization) in progress |
| mediationverse | - | - | Planned for Q2-Q3 2025 |

---

## Change Propagation Workflow

### When medfit Changes

```
+-------------------+
| 1. Make changes   |
|    in medfit      |
+--------+----------+
         |
         v
+-------------------+
| 2. Update         |
|    NEWS.md        |
+--------+----------+
         |
         v
+-------------------+
| 3. Run revdepcheck|    revdepcheck::revdep_check()
|    locally        |
+--------+----------+
         |
         v
+-------------------+
| 4. All pass?      |--No--> Fix issues or notify dependents
+--------+----------+
         | Yes
         v
+-------------------+
| 5. For breaking   |
|    changes:       |
|    - Create issue |
|    - 2 month      |
|      notice       |
+--------+----------+
         |
         v
+-------------------+
| 6. Release medfit |
+--------+----------+
         |
         v
+-------------------+
| 7. Dependent pkgs |
|    update & test  |
+-------------------+
```

### Breaking Change Protocol

1. **Announce** - Create GitHub issue in medfit with breaking change notice
2. **Timeline** - Minimum 2 months before removal of deprecated features
3. **Deprecation** - Use `lifecycle::deprecate_warn()` for soft deprecation
4. **Migration Guide** - Document migration path in vignette or NEWS.md
5. **Coordinate** - Schedule dependent package updates before release

### Non-Breaking Change Protocol

1. **Document** - Update NEWS.md with feature description
2. **Test** - Run `revdepcheck::revdep_check()` to verify no breakage
3. **Release** - Can release independently
4. **Notify** - Optional: inform dependent packages of new features

---

## Release Coordination Timeline

### Quarterly Release Schedule

| Quarter | Focus | medfit | Dependent Packages |
|---------|-------|--------|-------------------|
| Q1 2025 | MVP Release | v0.1.0 | Begin integration |
| Q2 2025 | Enhancement | v0.2.0 (lmer) | Update to use new features |
| Q3 2025 | Stabilization | v0.3.0 | Polish and testing |
| Q4 2025 | CRAN Prep | v1.0.0 | v1.0.0 releases |

### Release Checklist (medfit)

Before any medfit release:

- [ ] All tests passing (`devtools::test()`)
- [ ] R CMD check passing (`devtools::check()`)
- [ ] NEWS.md updated with all changes
- [ ] Reverse dependency check passed:
  ```r
  revdepcheck::revdep_check(num_workers = 4)
  ```
- [ ] Version bumped (`usethis::use_version()`)
- [ ] For breaking changes:
  - [ ] 2-month notice given
  - [ ] Migration guide written
  - [ ] Dependent packages notified

### Release Checklist (Dependent Packages)

Before any probmed/RMediation/medrobust release:

- [ ] All tests passing with CRAN medfit version
- [ ] All tests passing with medfit @main:
  ```r
  remotes::install_github("data-wise/medfit@main")
  devtools::test()
  ```
- [ ] NEWS.md updated with ecosystem notes
- [ ] DESCRIPTION has correct medfit version requirement
- [ ] Version bumped

---

## Cross-Package CI/CD

### GitHub Actions Setup

**In medfit** (`.github/workflows/revdep-check.yaml`):

```yaml
name: Reverse Dependency Check
on:
  workflow_dispatch:
  schedule:
    - cron: '0 0 * * 0'  # Weekly on Sunday

jobs:
  revdep:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: r-lib/actions/setup-r@v2
      - uses: r-lib/actions/setup-r-dependencies@v2
      - name: Install revdepcheck
        run: install.packages("revdepcheck")
        shell: Rscript {0}
      - name: Check reverse dependencies
        run: revdepcheck::revdep_check(num_workers = 4)
        shell: Rscript {0}
      - name: Upload results
        uses: actions/upload-artifact@v4
        with:
          name: revdep-results
          path: revdep/
```

**In dependent packages** (add to R-CMD-check.yaml):

```yaml
      # After standard check, also test with medfit dev
      - name: Test with medfit @main
        run: |
          remotes::install_github("data-wise/medfit@main")
          devtools::test()
        shell: Rscript {0}
```

---

## Package Locations

| Package | GitHub | Website | Local Development |
|---------|--------|---------|-------------------|
| medfit | [data-wise/medfit](https://github.com/data-wise/medfit) | [Website](https://data-wise.github.io/medfit/) | GoogleDrive/packages/medfit/ |
| probmed | [data-wise/probmed](https://github.com/data-wise/probmed) | [Website](https://data-wise.github.io/probmed/) | GoogleDrive/packages/probmed/ |
| RMediation | [data-wise/rmediation](https://github.com/data-wise/rmediation) | [Website](https://data-wise.github.io/rmediation/) | GoogleDrive/packages/rmediation/ |
| medrobust | [data-wise/medrobust](https://github.com/data-wise/medrobust) | [Website](https://data-wise.github.io/medrobust/) | GoogleDrive/packages/medrobust/ |
| medsim | [data-wise/medsim](https://github.com/data-wise/medsim) | [Website](https://data-wise.github.io/medsim/) | GoogleDrive/packages/medsim/ |
| mediationverse | [data-wise/mediationverse](https://github.com/data-wise/mediationverse) | [Website](https://data-wise.github.io/mediationverse/) | GoogleDrive/packages/mediationverse/ |

---

## Website Standards

All package websites follow consistent mediationverse branding:

### Navbar Structure
All packages include these menus:
- **Home** | **Reference** | **Articles** | **Ecosystem** | **Status** | **News** | **GitHub**

### Ecosystem Dropdown
Links to all packages:
- mediationverse (Meta-package)
- medfit (Foundation)
- probmed (P_med Effect Size)
- RMediation (Confidence Intervals)
- medrobust (Sensitivity Analysis)
- medsim (Simulation Infrastructure)

### Status Dropdown
Links to:
- Package Status Dashboard (STATUS.md)
- Development Roadmap
- Contributing Guide

### Styling
- **Primary Color**: #0054AD (Academic blue)
- **Fonts**: Inter, Montserrat, Fira Code
- **Theme**: Bootstrap 5 with Litera bootswatch

---

## Communication Channels

| Purpose | Location |
|---------|----------|
| Package-specific bugs/features | GitHub Issues in each repo |
| Ecosystem-wide design discussions | GitHub Discussions in medfit |
| Breaking change announcements | GitHub Issues in medfit + NEWS.md |
| Planning documents | /Users/dt/mediation-planning/ |

---

## S7 Class Stability

### Stable (Do Not Change Without Major Version Bump)

| Class | Package | Properties (stable) |
|-------|---------|---------------------|
| MediationData | medfit | a_path, b_path, c_prime, estimates, vcov |
| BootstrapResult | medfit | estimate, ci_lower, ci_upper, boot_estimates, method |

### Experimental (May Change)

| Class | Package | Notes |
|-------|---------|-------|
| SerialMediationData | medfit | Stable in v0.2.0+ |
| InteractionMediationData | medfit | Planned post-MVP |

---

## Semantic Versioning Policy

All packages follow semantic versioning: `MAJOR.MINOR.PATCH`

| Change Type | Version Bump | Example | Notice Required |
|-------------|--------------|---------|-----------------|
| Breaking S7 class changes | MAJOR | 1.0.0 -> 2.0.0 | 2 months |
| Breaking function signature | MAJOR | 1.0.0 -> 2.0.0 | 2 months |
| New non-breaking features | MINOR | 1.0.0 -> 1.1.0 | None |
| Bug fixes | PATCH | 1.0.0 -> 1.0.1 | None |
| Development version | .9000 suffix | 1.0.0.9000 | N/A |

---

## Monthly Ecosystem Sync

See [MONTHLY-CHECKLIST.md](MONTHLY-CHECKLIST.md) for recurring tasks (in same folder).

---

*Last Updated: December 2025*
