# Changelog

All notable changes to mediation-planning are documented here.

## 2025-12-19 (Documentation Optimization)

### Changed
- **medfit/CLAUDE.md** - Optimized for better usability
  - File size: 1,672 → 538 lines (68% reduction)
  - Added Quick Reference section at top for instant command access
  - Reorganized flow: Quick Reference → Standards → Core Topics → Details
  - Condensed defensive programming from 200+ lines to essentials
  - Streamlined S7 documentation (removed verbose S3/S4 details)
  - Simplified LaTeX/pkgdown sections using table formats
  - All critical information preserved (100% retention)
  - Improved navigation and readability
  - PR #11 merged to main: https://github.com/Data-Wise/medfit/pull/11
  - All CI checks passed (14/15, lint failure pre-existing)
  - Documentation site deployed: https://data-wise.github.io/medfit/

### Impact
- Faster onboarding for new contributors
- Easier reference lookup for development decisions
- Better organized guidance without information loss
- Professional polish for ecosystem foundation package

---

## 2025-12-17 (Integration Update)

### Added
- **docs/ROADMAP.md** - Master roadmap for mediationverse ecosystem
  - Detailed timeline (Phases 1-4)
  - medfit status: 100% complete (427 tests passing)
  - Timeline adjusted: 3 months ahead of schedule!
  - Phase 2 Integration ready NOW (was Q1 2026)
  - Success metrics and risk assessment
  - Integration priorities and future enhancements

### Changed
- **PROJECT-HUB.md** - Established as ecosystem coordination hub
  - Progress: 85% → 90%
  - Phase 2 (medfit Implementation): 100% complete ✅
  - Phase 3 (Ecosystem Integration): Ready to start 🚧
  - Updated active tasks: probmed integration (4-6 hours)
  - Updated decision options (probmed, RMediation, tooling)
  - Updated celebration checklist with all Phase 6-7 achievements
  - Added master roadmap link
- **Integration with mediationverse** - Clear separation of concerns
  - mediation-planning: Detailed planning, coordination, cross-package tasks
  - mediationverse: User-facing R package docs, getting started guides
  - mediationverse roadmap.Rmd now references mediation-planning

### Documentation
- Established mediation-planning as single source of truth for ecosystem roadmap
- mediationverse package updated to reference coordination hub
- Clear role definition: mediation-planning = planning, mediationverse = meta-package

---

## 2025-12-17 (Evening Update)

### Changed
- Updated PROJECT-HUB.md to reflect medfit 95% completion
  - New progress: 85% overall (was 60%)
  - Updated active tasks to focus on generic functions implementation
  - Updated celebration checklist with medfit core achievements
  - Revised decision options (generic functions, tooling, probmed integration)
- Updated medfit/.STATUS with implementation phases and code snippets
- Updated medfit/CLAUDE.md with generic functions implementation roadmap
- Updated TODOS.md with implementation priorities and ready-to-implement status
- Updated IDEAS.md with ready-to-implement code snippets

---

## 2025-12-17

### Added
- S7/S3 Interoperability Guide section in GENERIC-FUNCTIONS-STRATEGY.md
  - Package setup with `S7::methods_register()` in `.onLoad()`
  - Pattern 1: S7 methods for base R generics (print, coef, vcov, confint)
  - Pattern 2: New S7 generics for mediation-specific operations (nie, nde, te, pm, paths)
  - Pattern 3: S7 generics for S3 classes using `new_S3_class()`
  - Pattern 4: Accessing underlying data with `S7_data()`
  - Pattern 5: Inheritance with explicit `super()` calls
  - Summary table of interoperability tools
  - Recommended file organization for S7 generics
- IDEAS.md - Collection of ideas and future directions for ecosystem
- TODOS.md - Active tasks and pending items across all packages
- COORDINATION-HUB-BRAINSTORM.md - Options for using mediation-planning as central coordination hub
  - 6 categories: Status sync, Dashboards, Change coordination, Progress tracking, ADHD-friendly, Wild ideas
  - Top recommendations: medstatus script, /ecosystem slash command, mednow suggestion system
- ECOSYSTEM-PACKAGES.md - Comprehensive summary of all 6 mediationverse packages
- ADHD-Friendly API Design section in GENERIC-FUNCTIONS-STRATEGY.md
  - Option A: "One Function to Start" (`med()` entry point)
  - Option B: "Verb-First Chaining" (pipe-friendly `fit_*`, `get_*`)
  - Option C: Hybrid (recommended) - simple entry + explicit chains
- Decision 0: ADHD-Friendly API Pattern choice
- `quick()` function concept for instant answers
- CHANGELOG.md for tracking changes
- CLAUDE.md for Claude Code guidance

### Changed
- Reorganized project structure into organized subdirectories:
  - `docs/` - Planning & coordination documents
  - `specs/` - Technical specifications (API, standards, strategies)
  - `proposals/` - Proposals awaiting decisions
  - `tooling/` - Claude/MCP related docs
  - `archive/` - Superseded documents
- Archived CLAUDE-EXTENSION-PLAN.md (superseded by v2)
- Updated design decisions from 6 to 7 items
