# ADR 0002: Dual strategy (TCF + rule-based fallback)

## Status

Accepted.

## Context

Consent banners are implemented in two broad ways:

1. **Standardized**: Sites using IAB TCF expose `__tcfapi` and (optionally) allow programmatic consent via commands like `postCustomConsent`. The same API works across many CMPs that implement TCF.
2. **Non-standard**: Many sites use custom UIs, different CMPs without a shared API, or CMPs that only expose read-only TCF. Automation then requires interacting with the DOM (clicks) and/or setting cookies/localStorage.

We need a strategy that works for both and avoids duplicating logic or maintaining two unrelated code paths in an ad hoc way.

## Decision

We adopt a **dual strategy** with a clear order:

1. **TCF-first**: On each page, check for `__tcfapi`. If present and the CMP supports setting consent, use the API to set "necessary only" (minimal purposes, no optional vendors). No DOM interaction for these sites.
2. **Rule-based fallback**: If TCF is absent or read-only, match the current domain to a declarative rule set and execute the rule (clicks and/or cookie/localStorage writes). Rules are keyed by domain and support multi-step flows and storage.

Only one path runs per page; we do not combine TCF and rule clicks for the same domain unless explicitly designed (e.g. a rule that says "tcfOnly" and we only use TCF).

## Consequences

- **Coverage**: We can handle both TCF and non-TCF sites with one extension and one rule format.
- **Maintainability**: TCF path is centralized and API-driven; rule path is data-driven and community-maintainable.
- **Brittleness**: TCF path is less brittle (API contract); rule path requires ongoing rule updates as CMPs change their DOM.
- **Clear precedence**: Implementation always tries TCF first, then rules, so behavior is predictable and documented.
