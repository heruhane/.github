---
id: DEC-8
title: "Starlight joins the conditional tools as an exception to the proven criterion"
date: 2026-10-30
topics: tooling
holding: "Starlight is the conditional tool for publishing documentation or a long reference work as a site, although it has no stable release and so does not meet the proven criterion. The exception ends when Starlight publishes a stable 1.0, and until then each minor upgrade is read for breaking changes."
amended_by:
superseded_by:
amends:
supersedes:
applies:
codified_in: STANDARDS.md, Conditional tools
---

## 8. Starlight joins the conditional tools as an exception to the proven criterion

*Decided 30 October 2026, for the Starlight entry added in standards version 2026-10-30.1.*

**Decided.** **Starlight** is the conditional tool for publishing documentation or a long reference work as a site, as an exception to the proven criterion in [Choosing a tool](../STANDARDS.md#choosing-a-tool). The exception ends when Starlight publishes a stable 1.0.

Measured against the criteria on 30 October 2026:

1. **Openly licensed.** MIT.
2. **Proven: not met.** The latest release is 0.42.8, and there has never been a 1.0. It was first published on 8 May 2023, so it has more than three years of public releases, but its releases have had breaking changes, such as those listed for 0.42.0 on 2 September 2026, so it does not pass the three-year test either.
3. **Maintained.** It had 27 stable releases in the 180 days to 30 October 2026, the latest on that day, and has at least 100 contributors on GitHub. It is developed in the Astro organisation.
4. **Widely used.** 5,772,865 downloads in the last month (npm).
5. **Fits the toolchain.** It installs through `pnpm-lock.yaml`, needs only Node.js, and is built on Astro ([DEC-5](DEC-5.md)).
6. **Does several jobs.** It provides navigation, search, dark mode and internationalisation for a documentation site.

The exception is made because Starlight has been released steadily for more than three years, and large organisations rely on it for documentation. On 30 October 2026, the generator tags of these sites named it: [OpenAI's Agents SDK](https://openai.github.io/openai-agents-js/), [GitHub's gh-aw](https://github.github.com/gh-aw/), [Microsoft's GenAIScript](https://microsoft.github.io/genaiscript/) and [Netlify's documentation](https://docs.netlify.com/). The first three are the documentation of individual projects, not of those companies as a whole. The risk this accepts is limited: a breaking change costs a repository some upgrade work, which is seen in review before it lands.

Two alternatives were considered. **Waiting for 1.0** would leave no named tool for this job, and each repository that needed one would have to choose its own. **Building navigation and search by hand in Astro** would mean maintaining what Starlight already provides.

**Obligations.**

- **Read the changelog of each minor upgrade.** Before 1.0, a minor release can break things.
- **Close this exception** at the first quarterly review after a stable 1.0, or record why it continues.
