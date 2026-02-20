# 01 — Problem and goals

**Last updated:** 2025-02 (initial)

## Table of contents

- [Problem](#problem)
- [Goal](#goal)
- [Non-goals](#non-goals)
- [Stakeholders](#stakeholders)

---

## Problem

The modern web experience is gated by consent management interfaces that prioritize data collection over user experience. Most users click "Accept all" not because they want to be tracked, but because the cognitive load of opting out is intentionally made too high.

- **Dark patterns**: Reject / "Necessary only" is often hidden behind extra steps (e.g. "Preferences" or "Customize"), smaller buttons, or unclear wording.
- **Friction**: Choosing minimal consent requires multiple clicks and reading fine print, while "Accept all" is one prominent click.
- **Result**: Users lose agency. The system is designed so that the path of least resistance is maximum consent.

We want to build an open-source solution that reclaims this agency by automating the minimal-consent choice wherever possible.

---

## Goal

**Automate choosing the minimal-consent option ("Necessary only" or equivalent) across as many websites as possible.**

- **Minimal consent** means: only what is strictly necessary for the site to function (e.g. essential cookies, no marketing or non-essential tracking).
- **Across diverse sites**: Support both standardized consent (e.g. IAB TCF) and non-standard consent banners via a combination of APIs and declarative rules.
- **Open source**: Implementation, rules, and design are public; the project is maintainable and improvable by the community, with the original author credited.

---

## Non-goals

- **Not a general "cookie blocker".** We are not blocking all cookies or stripping consent; we are automating the choice to accept only necessary cookies (or reject non-essential ones) where the user has already expressed that intent by using the tool.
- **Not legal advice.** We do not guarantee that using this tool satisfies any specific jurisdiction's consent requirements; users and operators should consider their own legal context.
- **Not replacing user consent where the law requires explicit action.** Our stance is that the user, by installing and enabling the extension, has expressed the intent to choose minimal consent; we automate that expression. Legal interpretations may vary; we document this stance for transparency.

---

## Stakeholders

| Stakeholder | Interest |
|-------------|----------|
| **End users** | One-click (or zero-click) minimal consent; less tracking without the cognitive burden. |
| **Maintainers** | Clear design, maintainable rules, and a sustainable contribution process. |
| **Rule contributors** | Easy-to-understand rule format and a way to add or fix rules for specific sites/CMPs. |
| **Site operators** | Transparency: the tool is detectable in principle (e.g. via missing or altered consent state). We do not aim to hide; we aim to express the user's preference automatically. |
