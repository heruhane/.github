---
id: DEC-9
title: "Node.js is pinned to an exact version in .node-version"
date: 2026-10-30
topics: tooling
holding: "Node.js is pinned to an exact version in .node-version, as Python is pinned in .python-version. A copy of the version elsewhere, including in devEngines.runtime, reads the pin or is checked against it."
amended_by:
superseded_by:
amends:
supersedes:
applies:
codified_in: STANDARDS.md, Core tools
---

## 9. Node.js is pinned to an exact version in .node-version

*Decided 30 October 2026, in standards version 2026-10-30.1.*

**Decided.** Node.js is pinned to an exact version in `.node-version`. This replaces the earlier rule, which pinned it through `devEngines.runtime` so that `pnpm-lock.yaml` recorded the exact runtime.

Both runtimes are now pinned the same way, each in a file of its own. Other tools already read this file, including [`actions/setup-node`](https://github.com/actions/setup-node/blob/main/docs/advanced-usage.md) in CI and [Cloudflare's build image](https://developers.cloudflare.com/pages/configuration/build-image/). The version is exact because the earlier rule gave an exact runtime, and a bare major version could give different results on different days.

This has two costs. pnpm downloads the runtime that `devEngines.runtime` declares, so a repository that drops it needs something else to install Node.js. `devEngines.runtime` stays allowed as a copy of the version, checked against the pin. The standard also names nothing that moves the pin automatically, so a repository moves it by hand.

**Obligations.**

- **Keep `.node-version` exact**, as in `26.13.0` rather than `26`.
- **Check every copy of the version against it**, as the tooling standard requires.
