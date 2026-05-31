# SPEC: medfit Covariance Extraction for RMediation CI Methods

**Status:** Draft
**Author:** Davood Tofighi (with Claude Code)
**Date:** 2026-05-30
**Target release:** RMediation v1.5.0 (medfit integration)
**Affected repo:** `data-wise/rmediation` — `R/ci_medfit.R`
**Related:** `../proposals/ECOSYSTEM-NEXT-STEPS-2026-05-30.md` (item #4); `GENERIC-FUNCTIONS-STRATEGY.md`
**⚠️ Blocked by:** `MEDFIT-COVARIANCE-EXTRACTION-BLOCKERS-SPEC.md` — medfit must fix the lavaan
off-diagonal covariance drop (Blocker A) and add a serial extractor (Blocker B) before this spec's
acceptance criteria are satisfiable. Open Questions #1/#2 below are **answered** there.

---

## 1. Problem Statement

RMediation's `ci()` methods for `medfit` mediation objects (`R/ci_medfit.R`) locate the mediation
path parameters in the covariance matrix **by guessing**, and degrade to fallbacks that can silently
return incorrect confidence intervals.

This affects **both** entry points:

### Simple mediation — `ci_mediation_data()` (lines 69–153)

Three strategies to locate `a` and `b` in `@estimates` / `@vcov`:

1. match names `"a"`/`"b"` in `names(@estimates)`;
2. match `"a"`/`"b"` in `rownames(@vcov)`;
3. **positional value-matching**: `abs(estimates[1] - a) < 1e-10`, `estimates[2]` for `b`.

If all three fail:

```r
# R/ci_medfit.R:121-133
warning("Could not identify a and b parameters in vcov matrix. ",
        "Using approximation assuming independent parameters.")
se_a <- sqrt(diag_var[1]); se_b <- sqrt(diag_var[2])
cov_ab <- 0  # <-- assumes independence
```

### Serial mediation — `ci_serial_mediation_data()` (lines 157–235)

```r
# R/ci_medfit.R:186, 200-209
# TODO: Implement full covariance extraction
if (n_params == k) { sigma_paths <- vcov_mat }          # only when NO covariates
else {
  warning("Full covariance extraction for serial mediation not yet implemented. ",
          "Using diagonal variance approximation.")
  sigma_paths <- diag(diag(vcov_mat)[seq_len(k)])        # <-- drops path covariances
}
```

### What's actually correct vs. buggy (verified against `medfit/R/extract-lm.R`)

medfit's `lm` extractor **does** produce a named, block-diagonal vcov:

```r
# medfit/R/extract-lm.R:197-220
names(estimates) <- c(names_m, names_y)   # "m_<coef>", "y_<coef>"
estimates["a"] <- a_path; estimates["b"] <- b_path; estimates["c_prime"] <- c_prime
rownames(vcov_combined) <- colnames(vcov_combined) <- names(estimates)
# block-diagonal: vcov_m top-left, vcov_y bottom-right; cross-block = 0
```

So for **regression-based** mediation:

- The labels `"a"`, `"b"`, `"c_prime"` **exist** → Strategies 1–2 succeed, and
- `cov(a, b) = 0` is **correct by construction** (a from model_m, b from model_y, fit
  independently). The independence fallback is therefore *not* wrong for the lm path.

The genuine bugs are narrower but real:

1. **Silent wrong answer when labels are absent.** Strategy 3 (positional value-matching) and the
   `cov_ab <- 0` fallback fire with only a `warning()` for any extractor that does **not** append
   `"a"`/`"b"` names — e.g. `extract-lavaan` (correlated paths from a single SEM fit, where
   `cov(a,b) ≠ 0`) or future extractors. There the CI is silently incorrect.
2. **Serial within-model covariances dropped.** In serial mediation, the `d` paths and `b` path can
   come from the **same** outcome/mediator regressions, so their covariances are **non-zero**. The
   diagonal approximation discards them, biasing interval width — and it triggers whenever
   covariates make `n_params != k`.
3. **No hard failure.** Mis-resolution degrades to a `warning()`, not a `stop()`.

---

## 2. Current State (verified against source)

### medfit classes (`medfit/R/classes.R`, verified)

```r
MediationData <- S7::new_class("MediationData", package = "medfit", properties = list(
  a_path = class_numeric, b_path = class_numeric, c_prime = class_numeric,   # scalar paths
  estimates = class_numeric,                 # ALL coefficients; NAMED by extractor
  vcov      = new_S3_class("matrix"),        # square; rownames/colnames == names(estimates)
  sigma_m = class_numeric | NULL, sigma_y = class_numeric | NULL,
  treatment = class_character, mediator = class_character, outcome = class_character,
  mediator_predictors = class_character, outcome_predictors = class_character,
  data = class_data.frame | NULL, n_obs = class_integer, converged = class_logical,
  source_package = class_character
))
SerialMediationData <- new_class(...)         # adds d_path = class_numeric (length k-1),
                                              # mediators = character vector (ordered)
```

**Key finding:** there is **no `par_labels` slot.** The authoritative position map is the
**names on `@estimates` / dimnames on `@vcov`**, which the extractor populates as
`"m_<coef>"`, `"y_<coef>"`, plus appended `"a"`, `"b"`, `"c_prime"` (see §1). Reliable extraction
must key off **those names** — and medfit must guarantee they exist for every extractor (see Open
Question #1).

### RMediation consumers (`R/ci_medfit.R`, 256 lines)

| Function | Lines | Status |
|----------|-------|--------|
| `ci_mediation_data()` | 69–153 | 3-strategy guess → independence fallback (`cov_ab <- 0`) |
| `ci_serial_mediation_data()` | 157–235 | diagonal approximation; `TODO` at :187 |
| `.register_medfit_methods()` | 240–255 | registers both as S7 methods on `ci` via `zzz.R` |

---

## 3. Goal

A single, reliable **path → covariance** extraction that:

1. Resolves each mediation path (`a`, `b`, and `d_path[i]` for serial) to its exact index in
   `@estimates`/`@vcov` using the **names/dimnames** as the authoritative map (the `"a"`/`"b"`/
   `"c_prime"` labels the extractor appends).
2. Returns the correct **full** sub-covariance matrix (off-diagonals preserved) — so it is correct
   for both block-diagonal regression vcov (`cov(a,b)=0`) **and** correlated SEM vcov
   (`cov(a,b)≠0`, e.g. lavaan).
3. Errors **loudly** when a path label cannot be resolved — never silently positional-matches or
   assumes independence.
4. Removes the guessing path (Strategy 3 + `cov_ab <- 0`) and the serial diagonal approximation.

---

## 4. Proposed Design

> Spec only — no implementation here.

### 4.1 Path-index resolver (new internal `.resolve_path_indices()`)

- Input: a `MediationData`/`SerialMediationData` object + ordered vector of path labels
  (e.g. `c("a", "b")` or `c("a", d_labels, "b")`).
- Map each label → column index via `rownames(@vcov)` (fall back to `names(@estimates)`).
- If any label is unresolved (names absent or missing), **stop()** with a message naming the
  unresolved labels and pointing at the medfit extractor contract. **No positional/value-matching
  fallback.**

### 4.2 Sub-covariance extractor (replaces inline logic)

`.extract_path_vcov(mu, path_labels)` → `vcov[idx, idx, drop = FALSE]` (ordered to match
`c(a, d…, b)`), off-diagonals intact.

### 4.3 Wire into consumers

- `ci_mediation_data()`: build the 2×2 a–b Σ from `.extract_path_vcov(mu, c("a","b"))`; drop
  Strategies 1–3 and the `cov_ab <- 0` fallback.
- `ci_serial_mediation_data()`: replace the `n_params == k` branch + diagonal warning with
  `.extract_path_vcov(mu, c("a", d_labels, "b"))`; feed the full Σ to `MASS::mvrnorm`.

### 4.4 Backward compatibility

Public `ci()` signature and return list (`CI`, `Estimate`, `SE`, `type`, `level`, …) unchanged.

---

## 5. Acceptance Criteria

- [ ] With covariates present, extracted Σ equals the correctly-named sub-matrix (hand-checked vs
      `lm`/`lavaan` vcov), not a positional slice.
- [ ] **lm path**: extracted `cov(a,b) = 0` (block-diagonal) — result unchanged from today, but now
      via names, not the value-matching heuristic.
- [ ] **lavaan/correlated path**: extracted `cov(a,b) ≠ 0` is carried into the CI (the case the
      current `cov_ab <- 0` fallback gets wrong).
- [ ] Serial: full within-model path covariance used; "not yet implemented" warning removed.
- [ ] Unresolvable path label → informative `stop()` (no silent positional/independence fallback).
- [ ] `ci(type="dop") ≈ ci(type="MC")` (large `n.mc`) ≈ direct `medci()` for the same simple model.
- [ ] `R CMD check` clean; `devtools::test()` green.

---

## 6. Test Plan

New `tests/testthat/test-ci-medfit-covariance.R`:

1. **Covariates present** — `lm`-extracted model; assert Σ matches the named sub-matrix.
2. **Cross-method agreement** — `dop ≈ MC ≈ medci()` on one `MediationData`.
3. **Serial** — 2-mediator chain; full-Σ MC CI differs from old diagonal result, matches reference
   `MASS::mvrnorm` simulation with known Σ.
4. **Error path** — `MediationData` with unnamed `@vcov` raises an informative `stop()`.
5. **Non-zero cov(a,b)** — construct a `MediationData` with an off-diagonal Σ (lavaan-style); CI
   must change vs the independence result.

---

## 7. Out of Scope

- New CI method types (only fixing covariance extraction for existing dop/MC/asymp).
- Editing medfit's class definitions — **unless** Open Question #1 requires medfit to guarantee
  named `@estimates`/`@vcov` across all extractors.
- Moving medfit `Suggests → Imports` (gated on medfit reaching CRAN).
- `develop → dev` rename and `.STATUS` fix (separate quick wins).

---

## 8. Dependencies & Sequencing

- **Developable now** against GitHub-only medfit (`Remotes: data-wise/medfit` already in
  DESCRIPTION).
- The resulting RMediation v1.5.0 **CRAN release** still requires **medfit on CRAN** (CRAN forbids
  `Remotes:`). See ecosystem critical path in `../proposals/ECOSYSTEM-NEXT-STEPS-2026-05-30.md`.

---

## 9. Open Questions (coordinate with medfit)

1. **Do `extract-lavaan` and `extract-glm` append the `"a"`/`"b"`/`"c_prime"` names** (and serial
   `d` labels) to `@estimates`/`@vcov` the way `extract-lm` does (lines 197–215)? `extract-lm` is
   confirmed; the others are unverified. This is the linchpin — if any extractor omits the names,
   medfit must guarantee them (small medfit-side change) or this spec's resolver will `stop()`.
2. **What are the exact label strings for serial `d_path[i]`?** `extract-lm` appends `"a"`/`"b"`/
   `"c_prime"`; the serial extractor's `d` naming convention must be confirmed to build
   `c("a", d_labels, "b")` in the correct product order (M1→M2, M2→M3, …).
3. Should `asymp` (delta method) route through the same extractor for consistency? (Recommended:
   yes.)
