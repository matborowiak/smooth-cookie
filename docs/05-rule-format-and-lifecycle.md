# 05 — Rule format and lifecycle

**Last updated:** 2025-02 (initial)

## Table of contents

- [Schema overview](#schema-overview)
- [Required and optional fields](#required-and-optional-fields)
- [Examples](#examples)
- [Maintenance and versioning](#maintenance-and-versioning)
- [Conflicts: multiple rules per domain](#conflicts-multiple-rules-per-domain)
- [Safety](#safety)

---

## Schema overview

Rules are stored as a **list of rule objects** in a single JSON (or JSON5) file. Each rule describes how to achieve "necessary only" (or equivalent) for one or more domains. Rules are **declarative only**: selectors and cookie/storage key-value data. No arbitrary JavaScript.

---

## Required and optional fields

| Field | Required | Type | Description |
|-------|----------|------|-------------|
| `id` | Yes | string | Unique identifier (e.g. UUID). |
| `domains` | Yes | string[] | Domain list the rule applies to (e.g. `["example.com", "www.example.com"]`). No wildcards in v1; exact hostnames. |
| `tcfOnly` | No | boolean | If `true`, only use TCF for this domain; do not run click/cookie steps. |
| `presence` | No | string | CSS selector that indicates the consent banner is present. If missing, the rule may still run (e.g. cookie-only). |
| `optOut` | No | string or object | Selector for the "Reject all" / "Necessary only" button, or a step definition (see below). |
| `optIn` | No | string | Selector for "Accept all" (used only when we need to distinguish; most rules use optOut). |
| `steps` | No | array | Ordered list of steps for multi-click flows (e.g. open preferences, then reject, then save). Each step: `{ "click": "selector" }` or `{ "wait": 500 }`. |
| `cookies` | No | array | Cookies to set when opting out. Each item: `name`, `value`, optional `host`, `path`, `expiryRelative`, etc. |
| `localStorage` | No | object | Key-value pairs to set in localStorage when opting out (optional; same-origin only). |

**Step object** (inside `steps`):

- `{ "click": "selector" }` — click the element matching the selector.
- `{ "wait": ms }` — wait for the given number of milliseconds before the next step.

Cookie object (inside `cookies`): at minimum `name` and `value`; optionally `host`, `path`, `sameSite`, `secure`, `expiryRelative` (seconds), etc., as in Mozilla-style rules.

---

## Examples

### Single-button reject

```json
{
  "id": "example-single-reject",
  "domains": ["example.com"],
  "presence": ".cookie-banner",
  "optOut": ".cookie-banner button[data-action=reject]"
}
```

### Two-step: Preferences → Reject

```json
{
  "id": "example-two-step",
  "domains": ["site-with-preferences.com"],
  "presence": "#consent-modal",
  "steps": [
    { "click": "#consent-modal a[href='#preferences']" },
    { "wait": 300 },
    { "click": "#preferences-panel button.reject-all" },
    { "click": "#preferences-panel button.save" }
  ]
}
```

### Cookie-injection only (no click)

```json
{
  "id": "example-cookie-only",
  "domains": ["legacy-cmp.com"],
  "cookies": [
    {
      "name": "cookie_consent",
      "value": "necessary_only",
      "path": "/"
    }
  ]
}
```

---

## Maintenance and versioning

- Rules are **versioned in the repository** (e.g. `rules.json` or under `rules/`). Alternatively, a separate "rules list" repo can be used and referenced by the extension.
- **Community submissions**: Contributors submit new or updated rules via pull request. Each rule should reference the domain and, if known, the CMP name.
- **Validation**: A validation script (e.g. JSON Schema or custom checks) runs on the rule file to ensure required fields and allowed shapes. No arbitrary code in rules.

---

## Conflicts: multiple rules per domain

If several rules match the same domain:

- **Strategy**: "Most specific domain wins." If we have rules for `example.com` and `sub.example.com`, and the current host is `sub.example.com`, use the rule that lists `sub.example.com` (or the longest matching domain).
- If specificity is equal (e.g. two rules both list `example.com`), use **first match** in the list. The list should be curated to avoid duplicates; validators can warn when the same domain appears in multiple rules.

---

## Safety

- **No arbitrary code**: Rules may only contain selectors (strings), step objects (`click`, `wait`), and cookie/localStorage data. No `eval`, no script URLs, no inline handlers.
- **Sanitization**: Implementation must validate and sanitize selectors (e.g. reject `script`, `javascript:` in attributes if ever used) and cookie/localStorage values to prevent injection.
- **Review**: New or changed rules should be reviewed in PRs; maintainers can require a test URL or steps to verify before merge.
