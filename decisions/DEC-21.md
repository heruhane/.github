---
id: DEC-21
title: "The documentation standard: a README, documentation with the code, and open formats"
date: 2026-10-30
topics: documentation
holding: "Repositories are named in lowercase kebab-case, each has a README saying what it is, who it is for and how to start, documentation changes with the code, significant decisions are recorded, build output, secrets, .env files and large binaries stay out of Git, and open data formats are preferred."
amended_by:
superseded_by:
amends:
supersedes:
applies:
codified_in: STANDARDS.md, Documentation standard
---

## 21. The documentation standard: a README, documentation with the code, and open formats

*Decided 30 October 2026, in standards version 2026-10-30.1.*

**Decided.** Repositories are named in lowercase kebab-case, and each has a README saying what it is, who it is for and how to start. Documentation changes in the same change as the code. Significant decisions are recorded. Build output, secrets, `.env` files and large binaries stay out of Git. Open data formats are preferred, and data shared between Python and TypeScript has one schema.

Documentation written later tends not to be written. A decision is significant if it is expensive to reverse, affects users, adds a major dependency or changes how data is handled, and those are the ones people ask about afterwards. Open formats let people take their data elsewhere.

Two alternatives were considered. **Requiring a documentation site for every project** is too heavy for a small tool, whose README is enough. **Requiring one template for decisions** was rejected, because [Adopting the standards](../STANDARDS.md#adopting-the-standards) only asks for what a decision must contain, in whatever form a repository already uses.

**Obligations.**

- **Follow the [documentation standard](../STANDARDS.md#documentation-standard)** in every repository.
