# Brainstorm: Mediation-Planning as Ecosystem Coordination Hub

**Date:** 2025-12-17
**Goal:** Use mediation-planning as the central hub for planning, project controls, and coordination across the mediationverse ecosystem.

---

## Current State Analysis

### What Exists Now

**In mediation-planning:**
- `.STATUS` file (manual updates)
- `PROJECT-HUB.md` (progress dashboard)
- `docs/ECOSYSTEM-PACKAGES.md` (package inventory)
- `specs/` folder (technical specs)

**Elsewhere:**
- `~/projects/r-packages/PROJECT-BOARD.md` (daily ADHD tasks)
- Each package has its own `.STATUS` file
- GitHub Projects board exists but rarely used
- No automated sync between locations

### Pain Points
1. Status scattered across 7+ locations
2. Manual updates = stale information
3. No single source of truth
4. Context switching between repos loses momentum
5. Hard to see "big picture" ecosystem health

---

## BRAINSTORM: Coordination Options

### Category A: Status Synchronization

#### A1. ⭐ Central Status Aggregator Script
```bash
# medstatus - Run from anywhere
medstatus           # Show all package status
medstatus sync      # Pull all .STATUS files into one view
medstatus update medfit "Progress: 95"  # Update from CLI
```

**Implementation:**
- Shell script reads all `.STATUS` files
- Generates unified view in mediation-planning
- Can push updates to individual packages

**Pros:** Simple, works offline, CLI-friendly
**Cons:** Still manual trigger, not real-time

---

#### A2. ⭐⭐ Git Hook Propagation
When `.STATUS` changes in any package → auto-push to central hub

```
medfit/.STATUS changes
   ↓ (post-commit hook)
mediation-planning/status/medfit.status updated
   ↓ (generates)
mediation-planning/ECOSYSTEM-STATUS.md (aggregated view)
```

**Implementation:**
- Post-commit hooks in each package
- Central repo has receiver script
- Auto-generates dashboard

**Pros:** Automatic, no manual intervention
**Cons:** Requires hook setup in all repos, git dependency

---

#### A3. GitHub Actions Sync
Workflow triggered by any package update → syncs to central

```yaml
# In each package: .github/workflows/sync-status.yml
on:
  push:
    paths: ['.STATUS', 'DESCRIPTION']
jobs:
  sync:
    - Checkout mediation-planning
    - Update package status
    - Commit & push
```

**Pros:** Cloud-based, works across machines
**Cons:** Requires GitHub, slower feedback loop

---

#### A4. Symlink Farm
All `.STATUS` files symlinked into central location

```
mediation-planning/status/
├── medfit.status -> ~/projects/r-packages/active/medfit/.STATUS
├── probmed.status -> ~/projects/r-packages/active/probmed/.STATUS
└── ...
```

**Pros:** Always up-to-date, zero sync needed
**Cons:** Only works locally, symlinks can break

---

### Category B: Dashboard Generation

#### B1. ⭐ Auto-Generated Dashboard (Daily Cron)
```bash
# Runs at 8am daily
medgenerate-dashboard
```

Generates `ECOSYSTEM-DASHBOARD.md`:
```markdown
# Mediationverse Status - 2025-12-17

| Package | Progress | Status | Next Action |
|---------|----------|--------|-------------|
| medfit | 95% | active | Extended testing |
| probmed | 55% | active | Distribution support |
...

## Dependency Health
✅ All packages installable
⚠️ medrobust: 2 failing tests

## Recent Changes (7 days)
- medfit: 12 commits
- probmed: 3 commits
```

**Pros:** Always fresh, comprehensive
**Cons:** Requires cron setup

---

#### B2. ⭐⭐ Live Dashboard via Claude Code Slash Command
```
/ecosystem    # Generates live dashboard in terminal
/ecosystem medfit  # Focus on one package
```

**Implementation:**
- Custom slash command in mediation-planning
- Reads all .STATUS files
- Formats beautifully for terminal

**Pros:** ADHD-friendly, instant, no file clutter
**Cons:** Only works in Claude Code sessions

---

#### B3. Web Dashboard (GitHub Pages)
Static site auto-deployed from mediation-planning

**Pros:** Always accessible, shareable
**Cons:** Overkill for single-user, maintenance overhead

---

### Category C: Change Coordination

#### C1. ⭐⭐⭐ RFC-Style Proposals
For any cross-package change:

```
mediation-planning/rfcs/
├── 001-s7-migration.md         [ACCEPTED]
├── 002-generic-functions.md    [ACCEPTED]
├── 003-adhd-api.md            [DRAFT]
└── TEMPLATE.md
```

Each RFC contains:
- Problem statement
- Proposed solution
- Affected packages
- Migration plan
- Status (Draft → Review → Accepted → Implemented)

**Pros:** Traceable decisions, clear history
**Cons:** More process overhead

---

#### C2. ⭐ Ecosystem-Wide Issues in Central Repo
```
mediation-planning/issues/
- Use GitHub Issues for cross-cutting concerns
- Label: `affects:medfit`, `affects:probmed`, etc.
- Milestone: "Q1 2025"
```

**Pros:** Familiar GitHub workflow
**Cons:** Requires discipline to use

---

#### C3. Change Notification System
When making breaking changes:

```bash
medannounce "Breaking: medfit coef() signature changed"
# → Creates issue in all dependent packages
# → Updates CHANGELOG in mediation-planning
# → Sends notification (if configured)
```

**Pros:** Ensures nothing falls through cracks
**Cons:** Complex to implement

---

### Category D: Progress Tracking

#### D1. ⭐ Unified Progress in PROJECT-HUB.md
Single file with all package progress:

```markdown
## Package Progress

### medfit [95%] ████████████████████░
- [x] S7 classes
- [x] fit_mediation()
- [x] bootstrap_mediation()
- [ ] lmer engine
- [ ] brms engine

### probmed [55%] ███████████░░░░░░░░░
...
```

**Pros:** Visual, scannable, ADHD-friendly
**Cons:** Manual updates

---

#### D2. Derived Progress from Git/Tests
Auto-calculate progress from:
- % tests passing
- % functions documented
- % code coverage
- Commits this month

**Pros:** Objective, no manual entry
**Cons:** May not reflect actual progress

---

#### D3. ⭐ Kanban in Markdown
```markdown
## Kanban: Mediationverse

### Backlog
- [ ] brms engine for medfit
- [ ] CRAN submission for probmed

### In Progress
- [ ] medfit extended testing @medfit

### Review
- [ ] Generic functions strategy

### Done (This Week)
- [x] fit_mediation() implementation
- [x] bootstrap_mediation()
```

**Pros:** Visual flow, satisfying to move items
**Cons:** Still manual

---

### Category E: ADHD-Friendly Approaches

#### E1. ⭐⭐⭐ "What Should I Do Now?" Command
```bash
mednow
# Output:
# 🎯 SUGGESTED: medfit extended testing
#
# Why: You're 95% done with medfit. Finishing this
#      unblocks probmed and medrobust.
#
# Time estimate: 2-3 hours
# Command: cd ~/projects/r-packages/active/medfit && work medfit
```

**Pros:** Removes decision paralysis, context-aware
**Cons:** Needs smart logic

---

#### E2. ⭐ Daily Auto-Generated Focus Card
Morning cron job generates:
```
~/TODAYS-FOCUS.md

# Tuesday Dec 17: Mediationverse Focus

## ONE THING
Complete medfit extended testing

## IF TIME
- Review probmed vignettes
- Update ECOSYSTEM-DASHBOARD

## AVOID TODAY
- Starting new features
- Refactoring working code
```

**Pros:** Reduces morning decision fatigue
**Cons:** May not match energy level

---

#### E3. Context Breadcrumbs Across Packages
```bash
# When switching packages
medswitch probmed
# Saves context: "Was working on bootstrap tests in medfit"
# Later:
medcontext
# Shows: "Last time you were in probmed: adding distribution support"
```

**Pros:** Reduces context-switching cost
**Cons:** Requires discipline to use

---

### Category F: Wild Ideas

#### F1. Monorepo Migration
Move all packages into single repo with packages/ folder

**Pros:** Single git history, atomic commits across packages
**Cons:** Huge migration, CRAN complications

---

#### F2. Package Health Bot
GitHub App that monitors all repos, posts weekly summary

**Pros:** Hands-off monitoring
**Cons:** Overkill, another thing to maintain

---

#### F3. Voice-Activated Status Updates
"Hey Claude, mark medfit as 95% complete"

**Pros:** Zero friction
**Cons:** Not currently possible

---

#### F4. Obsidian Vault Integration
Use Obsidian as the coordination layer with graph view

**Pros:** Visual connections, powerful linking
**Cons:** Tool dependency, sync complexity

---

## Combinations That Work Well

### Combo 1: "Minimal Viable Coordination" ⭐⭐⭐
- A1 (Status aggregator script) + B2 (Slash command) + D1 (Unified progress)
- **Effort:** 2-3 hours
- **Impact:** Huge clarity improvement

### Combo 2: "Professional Hub"
- A2 (Git hooks) + C1 (RFCs) + D3 (Kanban) + E1 (What now)
- **Effort:** 1-2 days
- **Impact:** Full ecosystem control

### Combo 3: "ADHD Optimized"
- E1 (What now) + E2 (Daily focus) + E3 (Breadcrumbs) + B2 (Slash command)
- **Effort:** Half day
- **Impact:** Removes friction completely

---

## Top 3 Recommendations

### 1. ⭐⭐⭐ Minimal: Status Aggregator + Slash Command

**First steps:**
1. Create `~/.local/bin/medstatus` script
2. Reads all `.STATUS` files from known locations
3. Outputs unified table
4. Add `/ecosystem` slash command to mediation-planning

**Effort:** 1-2 hours
**Files to create:**
- `~/.local/bin/medstatus`
- `mediation-planning/.claude/commands/ecosystem.md`

---

### 2. ⭐⭐ "What Should I Do Now?" System

**First steps:**
1. Create `mednow` command that:
   - Reads all `.STATUS` files
   - Identifies blocking package (lowest progress with no blockers)
   - Suggests next action
   - Provides `cd` command to start

**Effort:** 2-3 hours
**Files to create:**
- `~/.local/bin/mednow`
- Logic file for priority calculation

---

### 3. ⭐ RFC System for Cross-Package Changes

**First steps:**
1. Create `proposals/rfcs/` folder (done - we have `proposals/`)
2. Create RFC template
3. Move GENERIC-FUNCTIONS-STRATEGY to RFC format
4. Document RFC workflow

**Effort:** 1 hour
**Files to create:**
- `proposals/RFC-TEMPLATE.md`
- `proposals/RFC-INDEX.md`

---

## Implementation Priority

| Priority | Item | Effort | Impact |
|----------|------|--------|--------|
| P0 | `medstatus` aggregator script | 1 hr | High |
| P0 | `/ecosystem` slash command | 30 min | High |
| P1 | `mednow` suggestion command | 2 hr | High |
| P1 | Unified progress in PROJECT-HUB | 30 min | Medium |
| P2 | RFC system | 1 hr | Medium |
| P2 | Git hook propagation | 2 hr | Medium |
| P3 | Daily focus generator | 2 hr | Medium |
| P3 | Context breadcrumbs | 3 hr | Medium |

---

## Decision Needed

Which approach resonates most?

1. **Minimal** - Just aggregation + slash command
2. **ADHD-Optimized** - Focus on reducing decisions
3. **Professional** - Full RFC/governance system
4. **Hybrid** - Mix of above

