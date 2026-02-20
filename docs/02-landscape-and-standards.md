# 02 — Landscape and standards

**Last updated:** 2025-02 (initial)

## Table of contents

- [IAB Transparency & Consent Framework (TCF)](#iab-transparency--consent-framework-tcf)
- [CMP diversity](#cmp-diversity)
- [Prior art](#prior-art)
- [How Smooth-Cookie differs](#how-smooth-cookie-differs)

---

## IAB Transparency & Consent Framework (TCF)

The [IAB Transparency & Consent Framework](https://github.com/InteractiveAdvertisingBureau/GDPR-Transparency-and-Consent-Framework) is a technical standard used by many consent management platforms (CMPs) to comply with EU data protection rules (e.g. GDPR).

### CMP API v2 (`__tcfapi`)

CMPs that implement TCF expose a global function `__tcfapi` in the page. Key commands:

| Command | Purpose |
|--------|---------|
| `ping` | Check if the CMP is loaded and whether GDPR (or similar) applies. |
| `getTCData` | Retrieve current consent data, including the TC string (encoded purposes and vendors). |
| `addEventListener` / `removeEventListener` | Subscribe to consent updates when the user changes preferences. |
| `postCustomConsent` (optional) | Set consent programmatically (purposes, vendors, legitimate interest). Not all CMPs support this. |

**TC string**: Encodes the user's consent and legitimate interest choices for purposes and vendors. "Necessary only" corresponds to minimal purposes and no optional vendors.

**Relevance for Smooth-Cookie**: When `__tcfapi` is present and supports setting consent (e.g. via `postCustomConsent` or equivalent), we can set "necessary only" without interacting with the DOM. When it is absent or read-only, we fall back to rule-based UI automation.

---

## CMP diversity

Consent UIs are implemented by many different CMPs, each with its own:

- **DOM structure**: Different class names, IDs, and HTML layout.
- **Button labels**: "Reject all", "Necessary only", "Essential only", "Only required", etc.
- **Flows**: Single button vs. "Preferences" → "Save" vs. toggles per category.
- **Loading**: Scripts load asynchronously (e.g. OneTrust's stub + BannerSDK; Cookiebot's consent script). Banners may appear late or inside iframes.

Common CMPs include OneTrust, Cookiebot, Quantcast, Sourcepoint, Cookiebot, Consent Manager (consentmanager.net), and many custom implementations. Many use TCF under the hood but expose custom UIs, so both API and DOM strategies are needed.

---

## Prior art

| Project | Approach | Notes |
|--------|----------|--------|
| **Consent-O-Matic** | Rule-based; crowdsourced rules (e.g. Rules.json). Users can report sites for rule updates. | Open source; focuses on DOM/click rules. |
| **Cocoma** | Uses Mozilla's cookie-banner-rules-list as an external dependency. | Leverages community-maintained rule list. |
| **Firefox built-in** | Used Mozilla's cookie-banner-rules-list (JSON schema: presence, optIn/optOut selectors, optional cookie injection). | The [Mozilla list was archived in January 2025](https://github.com/mozilla/cookie-banner-rules-list); PRs no longer accepted. |

These show that (1) rule-based approaches are viable but require ongoing maintenance, and (2) a shared rule format can be reused across implementations.

---

## How Smooth-Cookie differs

- **TCF-first**: When the page uses IAB TCF and allows programmatic consent, we use the API instead of DOM automation, reducing brittleness and avoiding click-based rules for those sites.
- **Dual strategy**: Combine TCF with a declarative rule set for non-TCF and custom banners.
- **Clear rule format and lifecycle**: Documented schema, validation, and contribution process; rules are data only (no arbitrary code).
- **Open governance**: Design and rules are public; contribution and ADRs are documented.
