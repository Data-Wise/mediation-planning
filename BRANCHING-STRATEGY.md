# Branching Strategy for Mediation Ecosystem

**Last Updated**: December 3, 2025
**Applies To**: medfit, probmed, RMediation, medrobust, mediationverse

---

## Recommended Branch Structure

### Main Branches (Permanent)

Each package maintains these long-lived branches:

#### 1. `main`
- **Purpose**: Production-ready code
- **Protection**: Protected, requires PR + CI passing
- **Releases**: All CRAN/GitHub releases tagged from this branch
- **Stability**: Never broken, always installable
- **Updates**: Only via PR from `dev` after thorough testing

#### 2. `dev`
- **Purpose**: Integration branch for ongoing development
- **Protection**: Protected, requires CI passing (can skip PR for small fixes)
- **Stability**: Generally stable, but may have experimental features
- **Updates**: Feature branches merge here first
- **Testing Ground**: Where cross-package integration is tested
- **Release Cycle**: Merged to `main` when ready for release

---

## Feature Development Branches (Temporary)

### Naming Conventions

```
feature/descriptive-name      # New features
fix/issue-number-description  # Bug fixes
refactor/component-name       # Code refactoring
docs/topic                    # Documentation updates
test/coverage-area            # Test improvements
```

### Examples

```bash
# Good branch names
feature/serial-mediation
feature/four-way-decomposition
fix/123-lavaan-extraction-bug
refactor/bootstrap-methods
docs/api-contracts
test/s7-validators

# Bad branch names
new-stuff
updates
fix
my-work
```

### Workflow

```bash
# Create feature branch from dev
git checkout dev
git pull origin dev
git checkout -b feature/my-feature

# Work on feature
# ... make changes ...

# Push to remote
git push -u origin feature/my-feature

# Create PR to dev (not main!)
gh pr create --base dev --title "Add my feature"

# After PR approved and CI passes
gh pr merge --squash  # or --merge or --rebase

# Delete feature branch
git branch -d feature/my-feature
git push origin --delete feature/my-feature
```

---

## Release Branches (Temporary)

For coordinating releases across the ecosystem.

### Naming Convention

```
release/v0.1.0
release/v1.0.0
```

### Purpose

- Final pre-release testing
- Version bumps and NEWS.md updates
- CRAN submission preparation
- Last-minute bug fixes

### Workflow

```bash
# Create release branch from dev
git checkout dev
git pull origin dev
git checkout -b release/v0.2.0

# Update version numbers
# Update NEWS.md
# Run R CMD check --as-cran
# Fix any issues

# Merge to main when ready
git checkout main
git merge --no-ff release/v0.2.0
git tag -a v0.2.0 -m "Release version 0.2.0"
git push origin main --tags

# Also merge back to dev (to get version bumps)
git checkout dev
git merge --no-ff release/v0.2.0
git push origin dev

# Delete release branch
git branch -d release/v0.2.0
git push origin --delete release/v0.2.0
```

---

## Hotfix Branches (Temporary)

For critical bugs in production (`main`).

### Naming Convention

```
hotfix/critical-bug-description
hotfix/v0.1.1-cran-issue
```

### When to Use

- Critical bug in released version on `main`
- CRAN requests immediate fix
- Security vulnerability
- Data loss or corruption bug

### Workflow

```bash
# Create from main (not dev!)
git checkout main
git pull origin main
git checkout -b hotfix/critical-bug

# Fix the bug
# Update version (patch bump: 0.1.0 -> 0.1.1)
# Update NEWS.md

# Test thoroughly
R CMD check --as-cran

# Merge to main
git checkout main
git merge --no-ff hotfix/critical-bug
git tag -a v0.1.1 -m "Hotfix: critical bug"
git push origin main --tags

# IMPORTANT: Also merge to dev
git checkout dev
git merge --no-ff hotfix/critical-bug
git push origin dev

# Delete hotfix branch
git branch -d hotfix/critical-bug
```

---

## Cross-Package Coordination Branches

For features spanning multiple packages.

### Naming Convention

```
ecosystem/feature-name
ecosystem/api-update
```

### When to Use

- API changes affecting multiple packages
- Coordinated feature rollout
- Breaking changes requiring simultaneous updates

### Workflow

```bash
# In medfit (foundation package)
git checkout -b ecosystem/new-api
# Implement new API
git push -u origin ecosystem/new-api

# In probmed (dependent package)
git checkout -b ecosystem/new-api
# Update to use new API
git push -u origin ecosystem/new-api

# Test integration before merging
# Use git worktrees for simultaneous testing

# Merge coordinated across packages (medfit first!)
# 1. Merge medfit ecosystem/new-api -> dev
# 2. Wait for CI
# 3. Merge probmed ecosystem/new-api -> dev
```

---

## Git Worktree Strategy for Multi-Package Development

### Why Use Worktrees?

- Work on multiple packages simultaneously
- Test cross-package changes without switching branches
- Keep each package's working directory clean

### Setup

```bash
# Main repositories (one-time setup)
cd ~/packages/medfit
cd ~/packages/probmed
cd ~/packages/RMediation
cd ~/packages/medrobust
cd ~/packages/mediationverse

# Create worktrees for coordinated feature
cd ~/packages/medfit
git worktree add ~/.claude-worktrees/medfit/new-feature-work ecosystem/new-api

cd ~/packages/probmed
git worktree add ~/.claude-worktrees/probmed/new-feature-work ecosystem/new-api

# Now work in worktrees
cd ~/.claude-worktrees/medfit/new-feature-work
# ... make changes to medfit ...

cd ~/.claude-worktrees/probmed/new-feature-work
# ... make changes to probmed that use new medfit API ...

# Test integration
cd ~/.claude-worktrees/probmed/new-feature-work
devtools::load_all("../../medfit/new-feature-work")  # Load dev medfit
devtools::test()  # Test probmed with dev medfit
```

### Worktree Naming Convention

```
~/.claude-worktrees/
├── medfit/
│   ├── main-work/              # Working on main branch
│   ├── feature-serial/         # Feature branch
│   └── ecosystem-api-v2/       # Cross-package feature
├── probmed/
│   ├── main-work/
│   └── ecosystem-api-v2/
└── mediationverse/
    └── initial-setup/
```

### Cleanup

```bash
# When done with feature
cd ~/packages/medfit
git worktree remove ~/.claude-worktrees/medfit/new-feature-work

# Or force remove if worktree has uncommitted changes
git worktree remove --force ~/.claude-worktrees/medfit/new-feature-work
```

---

## Branch Protection Rules (GitHub)

### `main` Branch

```yaml
Protection Rules:
  - Require pull request before merging
  - Require approvals: 1
  - Dismiss stale reviews on new commits
  - Require status checks to pass:
    - R-CMD-check (ubuntu-latest, R-release)
    - R-CMD-check (windows-latest, R-release)
    - R-CMD-check (macos-latest, R-release)
    - test-coverage (must be ≥80%)
    - lintr (must pass)
  - Require branches to be up to date before merging
  - Include administrators: No (maintainers can override in emergencies)
  - Restrict who can push: Maintainers only
  - Allow force pushes: No
  - Allow deletions: No
```

### `dev` Branch

```yaml
Protection Rules:
  - Require status checks to pass:
    - R-CMD-check (ubuntu-latest, R-release)
    - test-coverage (must be ≥70%)
  - Allow force pushes: No (use revert commits instead)
  - Allow deletions: No
  - Require pull request: Recommended but not required (for quick fixes)
```

---

## Ecosystem-Wide Release Coordination

### Version Bump Timeline

When medfit (foundation) releases a new version:

| Time | Package | Action |
|------|---------|--------|
| Week 1 | medfit | Create release/v0.2.0 branch, finalize features |
| Week 2 | medfit | Submit to CRAN, create GitHub release |
| Week 2-3 | probmed | Update dependency, test with new medfit |
| Week 3-4 | probmed | Release probmed v0.3.0 |
| Week 4 | RMediation, medrobust | (Optional) Update if using new features |
| Week 4-5 | mediationverse | Update dependency versions, release new mediationverse |

### Breaking Changes Coordination

When medfit plans breaking changes:

```bash
# 1. Announce in GitHub issue
# Title: "[BREAKING] API change in v2.0.0"
# Tag: @probmed-maintainer @rmediation-maintainer

# 2. Create ecosystem branch across packages
cd ~/packages/medfit
git checkout -b ecosystem/api-v2

cd ~/packages/probmed
git checkout -b ecosystem/api-v2

# 3. Develop and test in parallel (using worktrees)

# 4. Coordinate merge (medfit first, then probmed)

# 5. Coordinate release (medfit v2.0.0, then probmed v1.0.0)
```

---

## Branch Hygiene Best Practices

### 1. Keep Branches Short-Lived

- Feature branches: 1-2 weeks max
- Release branches: 1 week max
- Hotfix branches: 1-2 days max

### 2. Sync with `dev` Regularly

```bash
# Update feature branch with latest dev
git checkout feature/my-feature
git fetch origin
git merge origin/dev  # or rebase if no conflicts expected
```

### 3. Delete Merged Branches

```bash
# After PR merged, delete remote branch
git push origin --delete feature/my-feature

# Delete local branch
git branch -d feature/my-feature

# Prune stale remote-tracking branches
git remote prune origin
```

### 4. Use Descriptive Commit Messages

```
# Good commit messages
feat: Add SerialMediationData S7 class for product-of-three

fix(lavaan): Handle dimension mismatch in vcov extraction (#123)

docs: Update API-CONTRACTS.md with bootstrap signature

test: Add edge cases for empty mediator names

refactor: Simplify parametric bootstrap logic

# Bad commit messages
updates
fix stuff
wip
changes
```

### 5. Squash Before Merging (Optional)

For feature branches with many small commits:

```bash
# Squash last 5 commits before creating PR
git checkout feature/my-feature
git rebase -i HEAD~5
# Mark commits as "squash" in editor

# Or use GitHub's "Squash and merge" button
```

---

## Recommended Branch Strategy Summary

### Per-Package Branch Setup

```
medfit/
├── main (protected)
├── dev (protected)
└── feature/*, fix/*, docs/* (temporary)

probmed/
├── main (protected)
├── dev (protected)
└── feature/*, fix/*, docs/* (temporary)

RMediation/
├── main (protected)
├── dev (protected)
└── feature/*, fix/*, docs/* (temporary)

medrobust/
├── main (protected)
├── dev (protected)
└── feature/*, fix/*, docs/* (temporary)

mediationverse/
├── main (protected)
├── dev (protected)
└── feature/*, fix/*, docs/* (temporary)
```

### Workflow Summary

1. **Feature development**: `dev` → `feature/name` → PR → `dev`
2. **Bug fixes**: `dev` → `fix/name` → PR → `dev`
3. **Releases**: `dev` → `release/vX.Y.Z` → `main` (tagged) + merge back to `dev`
4. **Hotfixes**: `main` → `hotfix/name` → `main` (tagged) + merge to `dev`
5. **Cross-package**: `dev` → `ecosystem/name` (in multiple repos) → coordinated merge to `dev`

### Daily Development Commands

```bash
# Start new feature
git checkout dev && git pull
git checkout -b feature/my-feature

# Daily sync with dev
git fetch origin
git merge origin/dev

# Create PR when ready
git push -u origin feature/my-feature
gh pr create --base dev

# After merge, cleanup
git checkout dev && git pull
git branch -d feature/my-feature
```

---

## Claude Code Workflow Integration

When Claude Code works across packages:

1. **Check current branch** in each package worktree
2. **Create consistent branch names** across packages (e.g., `ecosystem/api-v2`)
3. **Use worktrees** for simultaneous multi-package work
4. **Test integration** before pushing
5. **Coordinate commits and PRs** (foundation package first)
6. **Update shared planning docs** in `/Users/dt/mediation-planning/`

---

**Document Status**: Recommended practice for ecosystem
**Adoption**: Implement gradually, starting with medfit and probmed
**Review**: Update as team workflow evolves
