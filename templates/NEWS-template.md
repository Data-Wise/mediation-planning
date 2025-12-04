# NEWS Template for Mediationverse Packages

Copy this structure for each package's NEWS.md file.

---

# {package} (development version)

## Breaking Changes

{List breaking changes with migration guidance. Include ecosystem impact.}

- `old_function()` has been removed. Use `new_function()` instead.
  - Migration: Replace `old_function(x)` with `new_function(x, default = TRUE)`
  - Ecosystem: probmed/RMediation updated in v{X.Y.Z}

## New Features

{List new features with brief descriptions.}

- Added `new_function()` for {purpose} (#issue_number)
- `existing_function()` gains new argument `option` for {purpose}

## Bug Fixes

{List bug fixes with issue references.}

- Fixed edge case in `function()` when input is NULL (#issue_number)
- `function()` now correctly handles negative values (#issue_number)

## Documentation

{Documentation improvements.}

- New vignette: "Getting Started with {package}"
- Improved examples in `?function`

## Internal

{Internal changes, usually not user-facing.}

- Refactored internal helper functions
- Improved test coverage to {X}%

## Ecosystem Notes

{Cross-package compatibility notes.}

- Tested with medfit v{X.Y.Z}
- Compatible with probmed >= {X.Y.Z}
- {Any coordination notes}

---

# {package} X.Y.Z (YYYY-MM-DD)

{Same structure as above for each release.}

## Breaking Changes
...

## New Features
...

## Bug Fixes
...

## Ecosystem Notes
...

---

# {package} X.Y.0 (YYYY-MM-DD)

## Major Release Notes

{For major version releases, include a summary section.}

This is the first stable release of {package} as part of the mediationverse ecosystem.

### Highlights

- Feature 1
- Feature 2
- Feature 3

### Migration from v{previous}

{For major versions, include detailed migration guidance.}

| Old | New | Notes |
|-----|-----|-------|
| `old_func()` | `new_func()` | Changed signature |
| `OldClass` | `NewClass` | S7 migration |

...

---

## Template Usage Notes

### Version Sections

```markdown
# {package} (development version)  <- Always at top, for unreleased changes

# {package} 1.2.3 (2025-06-15)     <- Released versions, newest first

# {package} 1.2.2 (2025-03-01)     <- Older releases below
```

### Linking Issues

Use GitHub issue references:
- `(#123)` for issues in same repo
- `(data-wise/medfit#45)` for cross-repo references

### Breaking Change Format

Always include:
1. What changed
2. Why it changed (briefly)
3. How to migrate
4. Ecosystem impact (if any)

Example:
```markdown
- `extract_mediation()` now requires explicit `treatment` argument.
  Previously inferred from formula, now must be specified for clarity.
  Migration: Add `treatment = "X"` to all calls.
  Ecosystem: probmed v2.0.0+ handles this automatically.
```

### Ecosystem Notes Section

Include for every release:
- medfit version tested against
- Any dependent package coordination
- Breaking changes that affect other packages

Example:
```markdown
## Ecosystem Notes

- Tested with medfit 1.0.0 (CRAN) and 1.0.0.9000 (dev)
- Coordinated release with probmed 2.1.0 for new feature X
- Breaking change in `func()` requires probmed >= 2.0.0
```

### Commit Message Integration

When using conventional commits, NEWS entries can be auto-generated:

| Commit Type | NEWS Section |
|-------------|--------------|
| `feat:` | New Features |
| `fix:` | Bug Fixes |
| `BREAKING CHANGE:` | Breaking Changes |
| `docs:` | Documentation |
| `refactor:` | Internal |
