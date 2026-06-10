# Ecosystem API & Naming Design Rationale

> The *why* behind the ecosystem's API conventions (S7, hybrid generics, function
> names). These are background research and finalized decisions — the **normative**
> contracts live in [`specs/GENERIC-FUNCTIONS-STRATEGY.md`](../../specs/GENERIC-FUNCTIONS-STRATEGY.md)
> and [`specs/API-CONTRACTS.md`](../../specs/API-CONTRACTS.md).

**Provenance:** these four documents were relocated here from
`medfit/planning/` on **2026-06-10** during planning consolidation. Although first
written in medfit's context, their decisions (object system, generic naming,
ecosystem-standard generics) govern API conventions across **all** mediation
packages, so they belong in the ecosystem hub. medfit retains pointer stubs.

| Document | What it covers |
|---|---|
| `API-DESIGN-DECISIONS.md` | Finalized decisions: S7, hybrid S3 generics, `mediate()`/`boot()`/`paths()`, standard R generics, broom, selective loading, CMAverse adapter. The decisions record. |
| `GENERIC-FUNCTIONS-RESEARCH.md` | Comparison of R OOP systems (S3/S4/S7/R6) — the research behind choosing S7. |
| `GENERIC-NAMING-STRATEGY.md` | Standard vs. custom generics; `confint` vs `ci`; ecosystem naming conventions. |
| `FUNCTION-NAMING-DEEP-DIVE.md` | Deep dive on `med()`/`mediate()`/`paths()` naming alternatives. |

> Note: these docs contain some medfit-specific implementation detail (roadmap
> phases, test-file names) preserved as historical context. The reusable content
> is the design reasoning, which applies ecosystem-wide.
