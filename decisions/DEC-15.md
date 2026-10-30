---
id: DEC-15
title: "The site standard: light static pages that call nobody else"
date: 2026-10-30
topics: sites
holding: "Sites build static output and send no JavaScript unless a feature needs it, staying under 200 KB of compressed JavaScript on first load on public pages. Repeated content is validated at build time, logic stays out of components, nothing loads from third parties, and security headers are set."
amended_by:
superseded_by:
amends:
supersedes:
applies:
codified_in: STANDARDS.md, Site standard
---

## 15. The site standard: light static pages that call nobody else

*Decided 30 October 2026, in standards version 2026-10-30.1.*

**Decided.** Sites build static output and send no JavaScript unless a feature needs it, staying under 200 KB of compressed JavaScript on first load on every page the public visits. Repeated content is validated at build time, logic stays out of components, nothing loads from third parties, and security headers are set.

Learners may be on old, shared or slow devices, so page weight is part of whether a site works for them. A page that calls no other company's servers while it loads tells nobody else about the visit. Invalid content that fails the build never reaches a learner. An editor's admin pages are not counted in the budget, because the public does not load them.

Two alternatives were considered. **Requiring a preview deployment for every pull request** was left out, because not every site needs one. **Third-party fonts or analytics**, which are simpler to add, were rejected as against the [privacy standard](../STANDARDS.md#privacy-standard).

**Obligations.**

- **Follow the [site standard](../STANDARDS.md#site-standard)**, and say in the pull request why any island uses `client:load`.
