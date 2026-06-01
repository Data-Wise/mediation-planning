# SPEC: medfit Covariance Extraction Blockers (upstream of RMediation v1.5.0)

**Status:** Draft
**Author:** Davood Tofighi (with Claude Code)
**Date:** 2026-05-31
**Affected repo:** `data-wise/medfit` — `R/extract-lavaan.R`, (new) serial extractor
**Blocks:** `RMEDIATION-MEDFIT-COVARIANCE-SPEC.md` (RMediation v1.5.0 integration)
**Verified against:** medfit 0.1.0 source (`extract-lavaan.R` sha `44a02307…`, `classes.R`)

---

## 0. Why this spec exists

Implementing the RMediation covariance spec revealed that **its acceptance criteria cannot be met
against medfit 0.1.0** — the gaps are upstream, in medfit. This spec defines the two medfit fixes
that must land first. RMediation's `.extract_path_vcov()` is correct in design but will return wrong
numbers (or `stop()`) until medfit produces correct, complete covariance information.

**Investigation method:** read medfit source directly; integrity-probed the tool channel
(`md5("hello") = 5d41402abc4b2a76b9719d911017c592`, ✓) to confirm reads were trustworthy.

---

## 1. Blocker A — lavaan extractor drops off-diagonal `cov(a, b)`

### Evidence (`medfit/R/extract-lavaan.R`)

The extractor adds named aliases `a`/`b`/`c_prime` to `@estimates`/`@vcov`, then fills the alias
**variances** — but only the **diagonal**:

```r
# extract-lavaan.R:239-280 (abridged)
vcov_expanded <- matrix(0, nrow = n_total, ncol = n_total)   # zero-initialised
vcov_expanded[1:n_orig, 1:n_orig] <- vcov_mat                # original block copied

copy_alias_variance <- function(alias_name, param_names_to_try) {
  ...
  alias_idx <- which(vcov_names == alias_name)
  for (param_name in param_names_to_try) {
    if (param_name %in% names(all_coef)) {
      orig_idx <- which(names(all_coef) == param_name)
      vcov_expanded[alias_idx, alias_idx] <<- vcov_mat[orig_idx, orig_idx]   # <-- DIAGONAL ONLY
      return()
    }
  }
}
copy_alias_variance("a", paste0(mediator, "~", treatment))
copy_alias_variance("b", paste0(outcome,  "~", mediator))
copy_alias_variance("c_prime", paste0(outcome, "~", treatment))
```

### The bug

The alias rows/cols are zero-initialised and only their **diagonal** entries are populated. The
**off-diagonal** `cov(a, b)` (and `cov(a, c')`, `cov(b, c')`) between the `a`/`b`/`c_prime` aliases
is **never copied** from the original `vcov_mat`. Result: any consumer reading the 2×2 sub-matrix
`vcov[c("a","b"), c("a","b")]` gets `cov(a,b) = 0`.

### Why it matters

In a **single-equation SEM** (the common lavaan mediation case), structural paths sharing an
equation are estimated jointly and **are correlated**, so their covariance is non-zero. Dropping it
biases the indirect-effect CI (DOP and MC), defeating the reason RMediation would consume a lavaan
fit rather than two `lm`s.

> **Correction (verified against `lavaan::vcov` during implementation, 2026-05-31):** in a *recursive*
> model with exogenous X, `cov(a, b)` is in fact **0** — `a` lives in the mediator equation and `b`
> in the outcome equation, estimated independently even within a single `sem()` call. The genuinely
> non-zero off-diagonal is **`cov(b, c')`** (both share the outcome equation). The fix copies the
> full sub-block regardless, so it is correct for whichever off-diagonals are non-zero; the
> acceptance test asserts the entire `a`/`b`/`c_prime` block equals `lavaan::vcov` (capturing
> `cov(b, c')`). The original "`cov(a,b) ≠ 0`" framing was the motivating hypothesis; this is the
> corrected statistical picture.

> Note: the `lm` extractor (`extract-lm.R`) is **correct** — it builds a genuinely block-diagonal
> vcov from two independent regressions, where `cov(a,b)=0` is true by construction. The bug is
> specific to the lavaan alias path.

### Required behavior

When adding the `a`/`b`/`c_prime` aliases, copy the **full** covariance structure among the original
parameters they alias — both variances **and** pairwise covariances — into the corresponding alias
rows/columns, and the alias↔original cross-covariances as needed by consumers.

### Design options (decide during implementation)

- **Option 1 (preferred): don't alias — relabel.** Instead of *appending* alias rows, attach the
  names `a`/`b`/`c_prime` directly to the existing parameter positions (rename, don't duplicate).
  Then `vcov[c("a","b"), …]` is just a relabelled view of the real matrix — all covariances correct,
  no copying logic, no zero-init trap. Cleanest; removes `copy_alias_variance` entirely.
- **Option 2: copy the full sub-block.** Keep aliasing, but for the alias set `{a, b, c_prime}` copy
  the full `length×length` sub-matrix of `vcov_mat` (variances + off-diagonals) into the alias block,
  and copy alias↔original covariances. More code; easy to get the index mapping wrong.

### Acceptance criteria (Blocker A)

- [ ] For a single-equation lavaan mediation fit with correlated paths,
      `extract_mediation_lavaan(fit)@vcov[c("a","b"), c("a","b")]` reproduces the true
      `lavaan::vcov(fit)` covariance for those paths, including the **off-diagonal**.
- [ ] `cov(a, c_prime)` and `cov(b, c_prime)` are likewise correct.
- [ ] `lm` extractor behavior unchanged (still block-diagonal, `cov(a,b)=0`).
- [ ] New test in medfit `tests/testthat/` asserting off-diagonal correctness vs `lavaan::vcov`.
- [ ] `R CMD check` clean; medfit test suite green.

---

## 2. Blocker B — no serial mediation extractor exists

### Evidence

- `SerialMediationData` is **defined** (`classes.R:294`) with slots `a_path`, `d_path` (length k-1),
  `b_path`, `estimates`, `vcov`, `mediators`, …
- But `grep "SerialMediationData(" R/` returns **only** `R/classes.R` (the definition/examples).
  **No extractor constructs it** from a fitted model — not from `lm`, not from `lavaan`. There is no
  `extract_*_serial` function and no serial branch in the existing extractors.

### Impact on RMediation spec

The RMediation spec's serial acceptance criteria (full within-model path covariance for
`c(a, d…, b)`) reference a medfit capability that **does not exist**. RMediation's
`ci_serial_mediation_data()` can only be correctly finished once medfit can *produce* a
`SerialMediationData` with a properly named, full `@vcov`.

### Required behavior

A serial mediation extractor (lavaan first; lm/path-by-path optional) that returns a
`SerialMediationData` whose:

- `@estimates`/`@vcov` are **named** with resolvable labels for `a`, each `d_path[i]`, and `b`
  (and `c_prime`), consistent with the naming contract the simple extractor uses.
- `@vcov` carries the **full** covariance among all chain paths (these often come from the same
  outcome/mediator equations, so off-diagonals are non-zero and must be preserved).
- `@d_path` ordering and the `d` label strings follow a documented convention (M1→M2, M2→M3, …)
  so RMediation can build `c("a", d_labels, "b")` in the correct product order.

### Design decisions (RESOLVED 2026-05-31)

1. **Label convention for `d_path[i]` — DECIDED: positional `d1, d2, …`.**
   `d1 = M1→M2`, `d2 = M2→M3`, … i.e. `d[i]` is the i-th `@d_path` slot. Labels are decoupled
   from variable names and map 1:1 to `@d_path` ordering. RMediation assembles the product order as
   `c("a", paste0("d", seq_len(k - 1)), "b")`. (Resolves RMediation Open Question #2.)
2. **lavaan vs lm scope — DECIDED: both lavaan and lm in v1.**
   lavaan single-equation SEM carries the full (non-zero) off-diagonal covariance among chain paths;
   the lm/sequential-regression path is block-structured (paths from separate regressions independent
   by construction). Both must be supported and tested, with their differing covariance structure
   documented.
3. **API shape — DECIDED: auto-detect in `extract_mediation()`.**
   `extract_mediation()` counts mediators and dispatches: `k == 1` → `MediationData`, `k ≥ 2` →
   `SerialMediationData`. Detection/branching lives in the single entry point; no separate public
   `extract_mediation_serial()`.

### Acceptance criteria (Blocker B)

- [x] A documented extractor produces `SerialMediationData` from a fitted (lavaan) serial model.
      *(commit `082f1b9` on `feature/serial-extractor`: `.extract_serial_mediation_lavaan()`,
      dispatched from `extract_mediation_lavaan()` when `mediator` is a length-≥2 ordered vector.)*
- [x] `@vcov` is named and includes correct off-diagonal covariances among `a`, `d…`, `b`
      (validated vs `lavaan::vcov`). *(named `a, d1…, b, c_prime`; tests assert the
      `vcov[c("a","d1","b"), …]` sub-block for 2- and 3-mediator chains.)*
- [x] `d_path` ordering + label convention documented and tested.
      *(positional `d_names <- paste0("d", seq_len(k-1))`, i.e. decision #1 — `d1`=M1→M2, etc.)*
- [ ] **GAP (open 2026-05-31): lm/sequential-regression serial extractor.** Decision #2 scoped v1 as
      **lavaan + lm**, but `082f1b9` ships only the lavaan path — no lm serial extractor exists
      (block-structured `@vcov`, `cov` between separate-equation paths = 0 by construction; must be
      documented as differing from the lavaan full-covariance case). Until this lands, a serial chain
      fit with `lm` has no extractor.
      **→ SPEC'D (2026-05-31, decision-complete, ready to implement):**
      `medfit/planning/specs/SPEC-lm-serial-extractor-2026-05-31.md`. Resolves the path *toward
      implementing* (not re-scoping): input API = `mediator_models` list (one entry point, honors
      decision #3); also fixes the latent simple-lm `cov(b, c')` alias bug; `lm + glm`; ordering
      cross-check; per-mediator `NA` sigma; covariates document-only. Implementation pending on a
      `feature/lm-serial-extractor` worktree.
- [ ] medfit `R CMD check` clean; tests green.

---

## 3. Sequencing

```
medfit Blocker A (lavaan off-diagonal cov)  ── smaller, well-defined ──┐
medfit Blocker B (serial extractor)         ── larger, needs design ───┤
                                                                       ▼
RMEDIATION-MEDFIT-COVARIANCE-SPEC.md  (now satisfiable)
                                                                       ▼
RMediation v1.5.0  ── still needs ──►  medfit on CRAN (CRAN forbids Remotes:)
```

**Recommended order:** Blocker A first (surgical, unblocks the simple+lavaan acceptance criteria),
then Blocker B (requires the label/API design decisions above). Both are independent of medfit's
CRAN submission and can proceed now against the local checkout.

---

## 4. Out of Scope

- RMediation-side changes (covered by `RMEDIATION-MEDFIT-COVARIANCE-SPEC.md`).
- medfit CRAN submission (separate ecosystem critical-path item; see
  `../proposals/ECOSYSTEM-NEXT-STEPS-2026-05-30.md`).
- New CI methods or effect types.

---

## 5. Status of the autonomous "implement until spec met" goal

**Blocker A: DONE** — merged to medfit `dev` via PR #19 (lavaan extractor preserves off-diagonal
`cov(b, c')`; `print.mediation_effect` dispatch fixed). Not yet on `main` (dev→main release pending).

**Blocker B: LAVAAN DONE, lm PENDING.** The **lavaan** serial extractor is implemented, tested, and
**merged to medfit `dev`** via PR #20 (commit `082f1b9` + review fixes), satisfying 3 of 4 acceptance
items (see §2) — positional `d1,d2,…` labels and arity-based auto-dispatch both match the resolved
decisions. The **medfit `dev` CI is fully green** (the pre-existing lint debt was also cleared, PR #21).
**One gap remains:** the lm/sequential-regression serial path. It is now **decision-complete and
spec'd** — `medfit/planning/specs/SPEC-lm-serial-extractor-2026-05-31.md` (input API `mediator_models`
list; also fixes the simple-lm `cov(b,c')` bug; lm + glm) — and the team chose to **implement** it
(not re-scope). Implementation pending on a `feature/lm-serial-extractor` worktree.

Once Blocker B is complete, `RMEDIATION-MEDFIT-COVARIANCE-SPEC.md` becomes satisfiable and
RMediation v1.5.0 is gated only on medfit reaching CRAN.
