---
id: DEC-6
title: "Sites are hosted on Cloudflare Workers, deployed with Wrangler"
date: 2026-10-30
topics: tooling
holding: "A site is served as static assets from Cloudflare Workers and deployed with Wrangler, installed through pnpm, with its settings in wrangler.jsonc. Its output stays portable to another static host."
amended_by:
superseded_by:
amends:
supersedes:
applies:
codified_in: STANDARDS.md, Core tools
---

## 6. Sites are hosted on Cloudflare Workers, deployed with Wrangler

*Decided 30 October 2026, in standards version 2026-10-30.1.*

**Decided.** A site is served as static assets from **Cloudflare Workers** and deployed with **Wrangler**, installed through pnpm. Its configuration lives in the repository, in `wrangler.jsonc`.

Cloudflare's documentation calls Workers [its primary platform for building applications and says to start new projects there](https://developers.cloudflare.com/pages/). Keeping the configuration in a file means that a change to it is reviewed like any other change.

The criteria below apply to tools, and Cloudflare is a service, so they are applied to Wrangler. A service can change its terms or prices, which is why a site's output stays plain static files that another host could serve.

Wrangler, measured against [Choosing a tool](../STANDARDS.md#choosing-a-tool) on 30 October 2026:

1. **Openly licensed.** MIT or Apache-2.0.
2. **Proven.** Its first stable release, 2.0, came on 8 May 2022, followed by 3.0 on 17 May 2023 and 4.0 on 13 March 2025.
3. **Maintained.** It had 85 stable releases in the 180 days to 30 October 2026, the latest on 29 October, and its repository has at least 100 contributors. Cloudflare develops it, and a site hosted there depends on Cloudflare in any case.
4. **Widely used.** 108,382,160 downloads in the last month (npm).
5. **Fits the toolchain.** It installs through `pnpm-lock.yaml` and needs only Node.js. Its esbuild and workerd dependencies have to be allowed to run install scripts.
6. **Does several jobs.** It deploys a site, uploads previews and serves the build locally.

Two alternatives were considered. **Cloudflare Pages** still works, but it is not where Cloudflare directs new projects. A site already on Pages records a temporary exception until it moves. **GitHub Pages** would serve the same files, but a site would then depend on a different provider from the one that holds heruhane's DNS.

**Obligations.**

- **Configure a Worker in `wrangler.jsonc`**, so that the file stays the record of how the site is served.
- **Keep the output portable.** A site does not depend on anything only Workers can serve, unless a decision says why.
