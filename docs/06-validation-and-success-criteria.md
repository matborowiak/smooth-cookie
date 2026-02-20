# 06 — Validation and success criteria

**Last updated:** 2025-02 (initial)

## Table of contents

- [Design complete when](#design-complete-when)
- [Future implementation: acceptance criteria](#future-implementation-acceptance-criteria)

---

## Design complete when

The design phase is considered complete when all of the following are satisfied:

1. **All technical hurdles** listed in [03-technical-hurdles](03-technical-hurdles.md) are addressed in the [solution design](04-solution-design.md) and [rule format](05-rule-format-and-lifecycle.md) (TCF + rules, multi-step, timing, storage, iframe limitation or strategy, legal stance).

2. **Rule schema is fixed** and documented in [05-rule-format-and-lifecycle](05-rule-format-and-lifecycle.md), with at least **three example rules** written for different CMPs or site types (e.g. one single-button, one two-step, one cookie-only). Reference examples: [rules-examples.json](rules-examples.json).

3. **TCF flow is specified**: When we call which TCF commands (`ping`, `getTCData`, `postCustomConsent` or equivalent), in what order, and how we verify success (e.g. callback, event, or banner dismissal). This is captured in [04-solution-design](04-solution-design.md) and can be refined in an ADR if needed.

4. **At least two ADRs** exist in [docs/adr/](adr/):
   - One for **documentation-first** (why we design before implementing).
   - One for **dual strategy** (TCF + rule-based fallback).

Once these are done, the documentation is the **source of truth** for implementation.

---

## Future implementation: acceptance criteria

When implementation starts, the first deliverable (e.g. browser extension) should satisfy at least:

- **Rule-based path**: Works on at least three distinct test sites (A, B, C) using the documented rule format; each site’s banner is correctly reduced to "necessary only" (or equivalent) according to the rule.
- **TCF path**: Works on at least one site (D) that uses TCF and supports setting consent via the API; consent is set to minimal without any DOM click.
- **No regressions**: On a small set of sites (e.g. E, F) without rules and without TCF (or with read-only TCF), the extension does not break the page or consent flow (e.g. it does nothing, or only attempts TCF read).
- **Rule validation**: A script or build step validates the rule file against the documented schema before release.

Additional criteria (performance, privacy, store compliance, etc.) can be added in ADRs or a separate implementation checklist when the time comes.
