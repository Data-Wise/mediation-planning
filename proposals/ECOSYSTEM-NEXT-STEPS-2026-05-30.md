# Ecosystem "What's Next" — Brainstorm

**Date:** 2026-05-30
**Author:** Davood Tofighi (with Claude Code)
**Scope:** RMediation repo review + mediationverse ecosystem review + prioritized next steps
**Trigger:** `/craft:do "review the repository and branches and review the mediationverse repo and brainstorm what's next"`

---

## TL;DR

RMediation is the **only** package on CRAN. Its planned v1.5.0 headline feature — computing
confidence intervals directly on `medfit` `MediationData` objects — **cannot ship to CRAN until
medfit is on CRAN first** (CRAN forbids `Remotes:`/GitHub-only deps). That single constraint makes
**medfit's CRAN submission the critical path for the entire ecosystem.**

Within the RMediation repo itself, the one real code blocker is an **incomplete covariance
extraction stub** (`R/ci_medfit.R:187`) that gates the v1.5.0 integration.

---

## 1. RMediation Repository Review

| Item | State |
|------|-------|
| **Version** | `1.4.0` (development; S7 core refactor complete; Imports cut 10 → 7) |
| **CRAN** | ✅ Published — the only ecosystem package on CRAN |
| **Branches** | `develop` (default) = `main` + 1 docs commit; `gh-pages` behind 31 (auto-deploy) |
| **medfit integration** | 🟡 Partially built — `ci()` method for `MediationData` exists, but **covariance extraction incomplete** |
| **Branch naming** | Uses `develop`; rest of ecosystem standardized on `dev` |

### Issues found

1. **Stale `.STATUS` (untracked)** — declares `Version: 1.0.0`, "maintenance mode",
   "Downloads 10k+/month". Contradicts `DESCRIPTION` (`1.4.0`). Generic template; misleading.
2. **`R/ci_medfit.R` covariance gaps:**
   - `:187` — `# TODO: Implement full covariance extraction`
   - `:206` — serial mediation emits `"Full covariance extraction for serial mediation not yet implemented"`
   - `:121` — `"This is a fallback that may not capture covariance correctly"`
3. **`gh-pages` behind 31 commits** — likely benign (auto-deployed), worth a glance.

---

## 2. Ecosystem Review (mediationverse)

> Source: `active/mediationverse/STATUS.md` (last updated 2026-05-09 — ~3 weeks stale) and
> `mediation-planning/PROJECT-HUB.md` / `TODOS.md`.

```
medfit          ████████████████████ 100% (427 tests, R CMD check clean) — NOT on CRAN
probmed         stable, ready for integration testing               — NOT on CRAN
RMediation      ✅ on CRAN (v1.4.0 dev)
medrobust       experimental / WIP
medsim          core complete; R-CMD-check failing (deps unpublished)
mediationverse  meta-package skeleton
Phase 3 (Ecosystem Integration): ░░░░░░ 0%
```

**Open PRs (as of 2026-05-09; may have merged since):**
- `mediationverse#2` — Imports → Suggests (✅ all green, ready to merge)
- `medrobust#1` — R-CMD-check workflow + Windows fix (awaiting CI re-run)
- `medsim#1` — add `Remotes:` field (R-CMD-check not triggering — mystery to investigate)

**medfit CRAN-prep blockers (small):** vignette decision (articles → vignettes/ or reword README),
delete stale `..Rcheck/` dir, drop redundant `Maintainer:` line, add optional `inst/CITATION`,
win-builder + R-hub pre-submission checks.

---

## 3. Brainstorm: What's Next

### ⚡ Quick Wins (< 30 min)
1. **Fix `.STATUS`** in RMediation (→ `1.4.0`, real focus) or delete it. Untracked + misleading.
2. **Refresh ecosystem `STATUS.md`** — 3 weeks stale; reconcile which open PRs merged.
3. **Decide `develop` → `dev` rename** for RMediation — last ecosystem naming inconsistency.

### 🔨 Medium Effort (1–4 hrs)
4. **Finish covariance extraction** (`ci_medfit.R:187`) — the real code blocker for RMediation
   v1.5.0; needs full vcov sub-matrix mapping for Monte Carlo + serial mediation. Buildable now
   against GitHub-only medfit.
5. **Delta-method SE for indirect effects** in `tidy()` — currently `NA`;
   formula `SE(ab) = √(a²σ²_b + b²σ²_a)` already specified in ecosystem TODOS.
6. **End-to-end integration tests** — RMediation `ci()` ↔ medfit `MediationData`.

### 🎯 Long-term / Strategic
7. **🔴 CRITICAL PATH — medfit → CRAN.** Unblocks the entire ecosystem integration story.
8. **RMediation v1.5.0** — after medfit lands on CRAN: `Suggests` → `Imports`, finish integration,
   release.
9. **probmed → CRAN** (also depends on medfit).

---

## 4. Recommended Sequence

```
medfit CRAN prep (#7)  ──►  medfit on CRAN
        │
        ├──►  RMediation: covariance stub (#4)  ──►  v1.5.0 integration (#8)  ──►  RMediation v1.5.0 on CRAN
        └──►  probmed integration  ──────────────►  probmed on CRAN (#9)
```

**Highest-value self-contained RMediation work right now:** #4 (covariance stub) — it's the one
real code blocker for v1.5.0 and can be developed/tested today against the already-complete medfit.

**Highest-leverage ecosystem move overall:** #7 (medfit → CRAN), which lives in the `medfit` repo.

---

## 5. Decision Point

- [ ] Tackle #4 (covariance stub) in an RMediation feature worktree?
- [ ] Pivot to #7 (medfit CRAN prep) in the medfit repo?
- [ ] Do the quick wins (#1–#3) first to clear stale state?
