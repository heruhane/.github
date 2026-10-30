---
id: DEC-7
title: "React is the UI framework, with Vite for apps"
date: 2026-10-30
topics: tooling
holding: "An interactive part of an Astro site that needs more than a few lines of browser JavaScript is a React island, and an app in which almost every screen is dynamic is built with React and Vite. A repository does not mix UI frameworks."
amended_by:
superseded_by:
amends:
supersedes:
applies:
codified_in: STANDARDS.md, Conditional tools
---

## 7. React is the UI framework, with Vite for apps

*Decided 30 October 2026, in standards version 2026-10-30.1.*

**Decided.** An interactive part of an Astro site that needs more than a few lines of browser JavaScript is a **React** island, and an app in which almost every screen is dynamic is built with React and **Vite**. A repository does not mix UI frameworks. Both are conditional tools, adopted only when a repository has this need.

Plain `<script>` tags remain the first choice for small behaviour, so a page sends no framework it does not need. Naming one framework means that knowledge and fixes carry between repositories.

Measured against [Choosing a tool](../STANDARDS.md#choosing-a-tool) on 30 October 2026:

1. **Openly licensed.** Both are MIT.
2. **Proven.** React has had stable releases since 15.0 on 7 April 2016, with 19.0 on 5 December 2024. Vite's first stable release, 2.0, came on 16 February 2021, with 8.0 on 12 March 2026.
3. **Maintained.** React had 14 stable releases in the 180 days to 30 October 2026, the latest on 14 October, and Vite had 30, the latest on 21 October. Each has at least 100 contributors on GitHub.
4. **Widely used.** In the last month, React had 739,339,117 downloads and Vite had 764,924,124 (npm).
5. **Fits the toolchain.** Both install through `pnpm-lock.yaml` and need only Node.js. Astro already depends on Vite, so a site gains no new build tool.
6. **Does several jobs.** Vite builds an app and serves it in development. React does one job, and no other tool in the standard does it.

The alternative was another UI framework, such as Vue or Svelte. Either could do the job, and nothing in the criteria rules them out. What matters is using one.

**Obligations.**

- **Choose the simplest option that works**, in the order the [site standard](../STANDARDS.md#site-standard) gives.
- **Use one UI framework** in a repository.
