# Changelog

All notable changes to mediation-planning are documented here.

## 2025-12-17

### Added
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
