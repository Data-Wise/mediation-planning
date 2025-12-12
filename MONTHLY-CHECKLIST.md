# Mediationverse Monthly Ecosystem Sync Checklist

Use this checklist at the beginning of each month to maintain ecosystem health.

---

## Quick Links

| Package | GitHub | CI Status | Website | Status Dashboard |
|---------|--------|-----------|---------|------------------|
| medfit | [repo](https://github.com/data-wise/medfit) | [actions](https://github.com/data-wise/medfit/actions) | [docs](https://data-wise.github.io/medfit/) | [dashboard](https://github.com/data-wise/mediationverse/blob/main/STATUS.md) |
| probmed | [repo](https://github.com/data-wise/probmed) | [actions](https://github.com/data-wise/probmed/actions) | [docs](https://data-wise.github.io/probmed/) | [dashboard](https://github.com/data-wise/mediationverse/blob/main/STATUS.md) |
| RMediation | [repo](https://github.com/data-wise/rmediation) | [actions](https://github.com/data-wise/rmediation/actions) | [docs](https://data-wise.github.io/rmediation/) | [dashboard](https://github.com/data-wise/mediationverse/blob/main/STATUS.md) |
| medrobust | [repo](https://github.com/data-wise/medrobust) | [actions](https://github.com/data-wise/medrobust/actions) | [docs](https://data-wise.github.io/medrobust/) | [dashboard](https://github.com/data-wise/mediationverse/blob/main/STATUS.md) |
| medsim | [repo](https://github.com/data-wise/medsim) | [actions](https://github.com/data-wise/medsim/actions) | [docs](https://data-wise.github.io/medsim/) | [dashboard](https://github.com/data-wise/mediationverse/blob/main/STATUS.md) |
| mediationverse | [repo](https://github.com/data-wise/mediationverse) | [actions](https://github.com/data-wise/mediationverse/actions) | [docs](https://data-wise.github.io/mediationverse/) | [dashboard](https://github.com/data-wise/mediationverse/blob/main/STATUS.md) |

---

## Monthly Checklist

### 1. Package Health Check

For each package, verify:

**medfit**
- [ ] R CMD check passing on all platforms (ubuntu, macos, windows)
- [ ] Test coverage >= 80%
- [ ] pkgdown site building correctly
- [ ] No open critical issues

**probmed**
- [ ] R CMD check passing
- [ ] Tests pass with medfit @main
- [ ] No open critical issues

**RMediation**
- [ ] R CMD check passing
- [ ] Tests pass with medfit @main (when integrated)
- [ ] No open critical issues

**medrobust**
- [ ] R CMD check passing
- [ ] Tests pass with medfit @main (when integrated)
- [ ] No open critical issues

**medsim**
- [ ] R CMD check passing
- [ ] All simulation tests passing
- [ ] No open critical issues

### 2. Cross-Package Compatibility

- [ ] Run medfit reverse dependency check:
  ```r
  # In medfit directory
  revdepcheck::revdep_check(num_workers = 4)
  ```
- [ ] Review any failures
- [ ] Create issues for compatibility problems

### 3. Version Matrix Update

Update ECOSYSTEM-COORDINATION.md with current versions:

| Package | Current Version | Last Updated |
|---------|-----------------|--------------|
| medfit | ___.___.___ | YYYY-MM-DD |
| probmed | ___.___.___ | YYYY-MM-DD |
| RMediation | ___.___.___ | YYYY-MM-DD |
| medrobust | ___.___.___ | YYYY-MM-DD |
| medsim | ___.___.___ | YYYY-MM-DD |

### 4. Documentation Review

- [ ] README files up to date with ecosystem context
- [ ] NEWS files have consistent format
- [ ] CLAUDE.md files reference current ecosystem state
- [ ] Vignettes render correctly

### 5. Roadmap Progress

Review against quarterly goals in ECOSYSTEM-COORDINATION.md:

**Q__ 2025 Goals:**
- [ ] Goal 1: ___
- [ ] Goal 2: ___
- [ ] Goal 3: ___

### 6. Issue Triage

For each package, review open issues:

- [ ] Assign priority labels
- [ ] Close stale issues (no activity > 3 months)
- [ ] Identify cross-package issues
- [ ] Create coordination issues if needed

### 7. Breaking Change Review

Check for upcoming breaking changes:

- [ ] Any deprecated functions approaching removal?
- [ ] Any S7 class changes planned?
- [ ] Dependent packages notified of timeline?

---

## Quarterly Deep Review

Every 3 months, additionally:

### API Stability Audit

- [ ] Review S7 class stability (MediationData, BootstrapResult, etc.)
- [ ] Check function signature consistency across packages
- [ ] Document any planned changes

### Performance Check

- [ ] Run benchmark suite (if available)
- [ ] Compare against previous quarter
- [ ] Identify optimization opportunities

### Security Review

- [ ] Check for outdated dependencies
- [ ] Review any security advisories
- [ ] Update if necessary

### Release Planning

- [ ] Review release timeline
- [ ] Plan coordinated releases if needed
- [ ] Update roadmap

---

## Log Template

Copy and fill this for each monthly sync:

```markdown
# Ecosystem Sync - YYYY-MM-DD

## Summary
- All packages passing: [ ] Yes [ ] No
- Issues found: {count}
- Actions needed: {list}

## Package Status

| Package | Version | CI | Issues | Notes |
|---------|---------|-----|--------|-------|
| medfit | | | | |
| probmed | | | | |
| RMediation | | | | |
| medrobust | | | | |
| medsim | | | | |

## Actions Taken
1.
2.
3.

## Next Month Focus
-
```

---

## Automation Opportunities

Consider automating:

1. **Weekly CI status email** - GitHub Actions can send notifications
2. **Dependency freshness check** - `pak::pkg_deps_explain()` for dependency tree
3. **Version matrix update** - Script to query all package versions
4. **Cross-package test runner** - Workflow to test all packages together

---

*Last Updated: December 2025*
