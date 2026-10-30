---
id: DEC-16
title: "The accessibility standard: WCAG 2.2 Level AA, checked by tools and by hand"
date: 2026-10-30
topics: accessibility
holding: "Web interfaces meet WCAG 2.2 Level AA, checked by @axe-core/playwright in the browser tests and by hand for new interface features. Colour alone never carries meaning, time limits are avoided or can be turned off, and each public site or service publishes an accessibility statement."
amended_by:
superseded_by:
amends:
supersedes:
applies:
codified_in: STANDARDS.md, Accessibility standard
---

## 16. The accessibility standard: WCAG 2.2 Level AA, checked by tools and by hand

*Decided 30 October 2026, in standards version 2026-10-30.1.*

**Decided.** Web interfaces meet WCAG 2.2 Level AA. `@axe-core/playwright` checks the key pages in the browser tests, and new interface features are checked by hand with the keyboard, a screen reader, zoom and reduced motion. Colour alone never carries meaning, time limits are avoided or can be turned off, and each public site or service publishes an accessibility statement.

If a learner cannot use a site, it does not work for them. Level AA is the level most widely used as a target. Automated tools find only some problems, and cannot tell whether a page makes sense with a screen reader, so the two kinds of check do different jobs.

Two alternatives were considered. **Level AAA** was rejected, because some of its criteria cannot be met for all content. **Automated checks alone** were rejected, for the reason above.

**Obligations.**

- **Follow the [accessibility standard](../STANDARDS.md#accessibility-standard)**, and treat an axe violation as a failed test.
