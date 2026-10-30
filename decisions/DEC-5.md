---
id: DEC-5
title: "Astro builds sites, with static output"
date: 2026-10-30
topics: tooling
holding: "Websites are built with Astro, with static output. A site that needs a server adapter records its own decision first."
amended_by:
superseded_by:
amends:
supersedes:
applies:
codified_in: STANDARDS.md, Core tools
---

## 5. Astro builds sites, with static output

*Decided 30 October 2026, in standards version 2026-10-30.1.*

**Decided.** Websites are built with **Astro**, with static output. A site that needs a server adapter records its own decision first.

Most of what heruhane publishes is content to read, with some interaction. By default Astro's pages carry no browser JavaScript, which suits learners on old devices and slow connections, and its output is static files that are cheap and safe to host. Browser tests already need Node.js, so a site needs only one toolchain.

Measured against [Choosing a tool](../STANDARDS.md#choosing-a-tool) on 30 October 2026:

1. **Openly licensed.** The `astro` package is MIT.
2. **Proven.** Its 1.0 came on 9 August 2022, so it has more than four years of stable releases. Major releases have been frequent: 5.0 on 3 December 2024, 6.0 on 10 March 2026 and 7.0 on 22 June 2026, the last two with published upgrade guides. This is the criterion to recheck first, because two major releases in about three months come close to forcing rework on its users.
3. **Maintained.** It had 62 stable releases in the 180 days to 30 October 2026, the latest on 28 October, and has at least 100 contributors on GitHub.
4. **Widely used.** 29,526,691 downloads in the last month (npm).
5. **Fits the toolchain.** It installs through `pnpm-lock.yaml` and needs only Node.js.
6. **Does several jobs.** It builds pages, validates content through content collections and its own copy of Zod, and type checks `.astro` files with `astro check`.

Two alternatives were considered. **Vite with React** suits an app in which almost every screen is dynamic ([DEC-7](DEC-7.md)), but it would send JavaScript for pages that are mostly text. **A Python static site generator** would add a second toolchain alongside the browser tests.

**Obligations.**

- **Use static output**, and record a decision before adding a server adapter.
- **Recheck the proven criterion** at each quarterly review while major releases stay this frequent.
