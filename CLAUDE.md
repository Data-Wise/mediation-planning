# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Purpose

This is the **ecosystem coordination hub** for the mediationverse R package family. It contains planning documents, dependency maps, architecture decisions, and cross-project coordination materials.

**This is NOT an R package** - it's a research planning repository that coordinates development across multiple related R packages.

## Directory Structure

```
mediation-planning/
├── PROJECT-HUB.md          # Entry point - master dashboard
├── CHANGELOG.md            # Change tracking
├── .STATUS                 # Quick status file
├── docs/                   # Planning & coordination
├── specs/                  # Technical specifications (stable)
├── proposals/              # Ideas awaiting decisions
├── tooling/                # Claude/MCP related
├── archive/                # Superseded docs
└── templates/              # Document templates
```

## Key Files

| File | Purpose |
|------|---------|
| `PROJECT-HUB.md` | Start here - overall status, next actions, links |
| `specs/GENERIC-FUNCTIONS-STRATEGY.md` | API design with 6 pending decisions |
| `specs/API-CONTRACTS.md` | S7 classes and function signatures |
| `specs/DEVELOPMENT-STANDARDS.md` | Coding standards for ecosystem |

## Related Packages

The mediationverse packages live in `~/projects/r-packages/`:
- `active/medfit` - Core fitting infrastructure
- `active/probmed` - Probability-based mediation
- `stable/rmediation` - Production CRAN package

## Workflow

This repository uses markdown documents only. No build system, tests, or code to compile.

When making changes:
1. Update the relevant document
2. Add entry to CHANGELOG.md
3. Update PROJECT-HUB.md if scope changes
4. Update .STATUS if next actions change
