# ADR 0001: Documentation-first approach

## Status

Accepted.

## Context

Smooth-Cookie aims to automate "Necessary only" consent across diverse, non-standardized consent banners. The problem involves many technical hurdles (TCF vs. non-TCF, dark patterns, multi-step flows, timing, storage). Building implementation before clarifying design risks:

- Inconsistent or incomplete handling of edge cases.
- Rule format churn as we discover requirements late.
- Difficulty onboarding contributors who cannot infer intent from code alone.

We want a single source of truth that implementation can follow and that can be refined and validated before writing code.

## Decision

We use a **documentation-first** process:

1. All product and technical decisions are captured in the `docs/` folder (problem, goals, landscape, technical hurdles, solution design, rule format, validation criteria).
2. No implementation (e.g. browser extension code) is started until the design is validated against the criteria in [06-validation-and-success-criteria](../06-validation-and-success-criteria.md).
3. Implementation must reference and align with these docs; changes that contradict the design require updating the docs (and ADRs when appropriate) first.

## Consequences

- Design can be reviewed and refined without touching code.
- Contributors and maintainers share a clear reference.
- Implementation has a defined target; scope creep is reduced by the documented non-goals and success criteria.
- Slightly slower start on code; payoff is coherence and maintainability.
