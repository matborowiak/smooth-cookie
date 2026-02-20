# Smooth-Cookie documentation

This folder is the **single source of truth** for design and architecture. All product and technical decisions live here; implementation will reference these docs.

## Reading order

For a full picture, read in this order:

1. **[01-problem-and-goals.md](01-problem-and-goals.md)** — Why we're building this; goals and non-goals.
2. **[02-landscape-and-standards.md](02-landscape-and-standards.md)** — IAB TCF, CMPs, and prior art.
3. **[03-technical-hurdles.md](03-technical-hurdles.md)** — What makes automating "Necessary only" hard.
4. **[04-solution-design.md](04-solution-design.md)** — Proposed architecture: TCF-first + rule-based fallback.
5. **[05-rule-format-and-lifecycle.md](05-rule-format-and-lifecycle.md)** — Rule schema, maintenance, and safety.
6. **[06-validation-and-success-criteria.md](06-validation-and-success-criteria.md)** — When the design is "done" and what implementation must achieve.

## Index

| Doc | Purpose |
|-----|---------|
| [01-problem-and-goals](01-problem-and-goals.md) | Problem statement, goals, non-goals, stakeholders |
| [02-landscape-and-standards](02-landscape-and-standards.md) | IAB TCF, CMP diversity, prior art |
| [03-technical-hurdles](03-technical-hurdles.md) | Automation challenges (DOM, TCF, dark patterns, etc.) |
| [04-solution-design](04-solution-design.md) | Dual strategy, delivery, detection order |
| [05-rule-format-and-lifecycle](05-rule-format-and-lifecycle.md) | Rule JSON schema, maintenance, conflicts, safety |
| [06-validation-and-success-criteria](06-validation-and-success-criteria.md) | Design-complete criteria, implementation acceptance criteria |
| [rules-examples.json](rules-examples.json) | Example rules (single-button, two-step, cookie-only) for schema reference |
| [adr/](adr/) | Architecture Decision Records for key decisions |

## Conventions

- **Stable, linkable sections**: Each doc has clear headings so ADRs or code can link to specific sections.
- **Versioning**: Docs may include a "Last updated" line; significant changes should be noted.
- **ADRs**: Consequential decisions are recorded in [adr/](adr/) (context, decision, status).
