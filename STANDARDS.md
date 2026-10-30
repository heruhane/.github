# heruhane software development standards

*Draft of 30 October 2026, to replace `TOOLING.md`. It takes its version number when it is merged.*

The heruhane software development standards are a set of separate standards kept in one document, with one version. The [tooling standard](#tooling-standard) names one tool for each job in each language, and one way of running it. The others cover [testing](#testing-standard), [review](#review-standard), [coding agents](#coding-agent-standard), [Japanese language content](#japanese-language-content-standard), [sites](#site-standard), [accessibility](#accessibility-standard), [privacy](#privacy-standard), [security](#security-standard), [licensing](#licensing-standard), [releases](#release-standard), [documentation](#documentation-standard), [services](#service-standard) and [public repositories](#public-repository-standard). A repository adopts the ones it chooses (see [Adopting the standards](#adopting-the-standards)). heruhane is a small Japanese language education organisation, and coding agents do much of its programming, so the rules are written to be followed by people and agents alike.

How a change reaches `main` is covered by [`CONTRIBUTING.md`](CONTRIBUTING.md). The reasons behind individual tool choices are recorded in [`decisions/`](decisions/), one file per decision, so that this document can say only what a repository must do.

Rules are written as plain instructions and are required wherever they apply. "Should" marks a default: depart from it when you have a good reason, and say why in the pull request or commit message. Departing from anything else takes an [exception](#exceptions).

## Scope

heruhane repositories come in a few kinds: TypeScript or Astro sites and apps, Python tools and data projects, projects that combine the two, and repositories that are mostly writing, such as books and course material, with a build system around them. A repository follows the standards it has adopted, and each of those applies as follows:

- **The language rows** of the tooling standard's [Core tools](#core-tools) apply to each language a repository contains. A Python-only repository has no Node.js toolchain, and a site with no Python has no uv.
- **The tooling rules that do not depend on a language** apply to every repository that has the files they cover: Make, pre-commit with the standard file checks, Dependabot for GitHub Actions, actionlint for workflows, and the [Supply chain](#supply-chain) rules. A repository with no code yet adopts these when it gets its first workflow or script.
- **Writing is never formatted by a tool.** Where a repository's main content is prose, the formatters and the file-fixing hooks are kept off it (see [Rules](#rules)).
- **The [testing](#testing-standard) and [review](#review-standard) standards** apply to every repository with code, the [coding agent standard](#coding-agent-standard) to every repository an agent works in, and the [privacy](#privacy-standard), [security](#security-standard) and [documentation](#documentation-standard) standards to every repository.
- **The other standards have a condition**, and apply once it is met. Each opens by saying what it is.

## Adopting the standards

A repository adopts whichever of these standards it chooses, and does not have to adopt them all. It does so by recording a decision that names the standards it follows and the version of this document, and lists any exceptions (see [Exceptions](#exceptions)). The document has one version, which covers every standard in it, so a repository follows one version for everything it has adopted. To adopt another standard, it records a new decision. To move to a newer version, it reads the [Version history](#version-history) for the changes since its current version and records the move in a new decision, together with anything it has not yet done. A repository does not have to adopt a new version on the day it is published, but it checks for one at least once a quarter.

A decision can be kept in whatever form the repository already uses for its decisions, such as a file in a `docs/decisions/` folder. Each one gives at least:

- a title and the date it was made;
- what was decided, in one or two sentences;
- the reason;
- for an exception, the rule it departs from and its end condition or review date (see [Exceptions](#exceptions));
- for a change to an earlier decision, which decision it replaces.

Changes to the standards are made by pull request to this document. Each merged change gives the whole document a new version: the date the change is merged, written as `YYYY-MM-DD`. When more than one change is merged on the same day, the second adds the suffix `.2`, the third `.3`, and so on, as in `2026-10-28.2`. The pull request updates the version line, adds an entry to the version history, and adds a decision to `decisions/` when it chooses or replaces a tool or sets a policy. A decision is a file named `DEC-N.md`, numbered in order, in the shape of [DEC-1](decisions/DEC-1.md). It is never rewritten afterwards: a later decision that changes it names it in `amends` or `supersedes`, and the earlier file gains the matching `amended_by` or `superseded_by`.

The document is reviewed once a quarter, along with the [Tools to watch](#tools-to-watch), and rules that no longer add value are removed.

## Principles

Every standard here serves the same four principles.

1. **Learners come first.** Most people using heruhane's work are learning a language, on whatever device and connection they have, which may be old, shared or slow.
2. **Wrong Japanese is a serious bug.** A learner cannot tell a mistake from the truth, and may carry it for years.
3. **Private by default.** The best personal data is data that was never collected. Most projects should collect none.
4. **Accessible by default.** If a learner can't use it, it doesn't work.

## Tooling standard

The tooling standard names one tool for each job in each language, and one way of running it. A project that mixes Python and TypeScript can easily end up with two toolchains that work differently, pin versions in different places and run their checks at different times. Using the same tools in the same way everywhere means that someone who knows one heruhane repository can work in any of them, and that a fix found in one repository can be copied to the others.

The first list below says how a tool is chosen for this standard. The second says how the chosen tools are run, and the [Rules](#rules) carry it out.

### Choosing a tool

A tool joins the tooling standard only when it meets all six criteria. Each is meant to be checked, not judged by impression.

1. **Openly licensed**, under a licence approved by the Open Source Initiative. If the project is abandoned, or its owner changes its licence, the last open release can still be used and forked.
2. **Proven.** It has either a stable release (1.0, or the project's stated equivalent) and at least a year of public releases, or at least three years of releases without breaking changes for its users. Its recent releases have not forced rework on their users. When a newer tool is faster but less proven, the proven tool stays until the newer one passes this test.
3. **Maintained, in proportion to the cost of replacing it.** It has had commits or answered issues in the last six months; a finished tool may release rarely. A tool that would be expensive to replace, such as a package manager, a type checker, a test runner or a formatter, is maintained by more than one person, so that it does not stop when one maintainer does, or depend on one company's continued interest. A small single-purpose tool may have one main maintainer, as long as an alternative or a workable fork exists if that maintainer stops, as prek is for pre-commit. Backing by a company or foundation helps when it pays for maintenance, but it is not a substitute for it.
4. **Widely used**: at least a million downloads a month from its main package registry, or comparable evidence for a tool distributed another way, such as a single binary. Wide use means documentation, answers to questions, editor support, plugins, and coding agents that already know the tool. The figure applies to the tool, not to a wrapper that packages it, such as a Python wheel that ships a Go binary.
5. **Fits the toolchain.** It installs through a lockfile the repository already has, behaves the same on Linux, macOS and CI, and needs no second language runtime just to run.
6. **Does several jobs, where one tool can.** One tool that lints and formats is preferred over two that each do half, as long as it does both well. Every extra tool is one more thing to pin, update and learn, and a small organisation feels that cost in every repository.

### Running tools

1. **Reproducible.** A given commit gives the same results on any machine, for any person or coding agent, because every tool and runtime has exactly one version pin, in a lockfile wherever one can hold it. When a tool can't live in a lockfile (uv, pnpm, Make, pre-commit run through `uvx` in a repository with no Python, a hook fetched from another repository, a GitHub Action, a container base image), it still has exactly one pin, and the repository's documentation says where.
2. **Each check runs at the earliest stage its cost allows.** Fast checks run before each commit, slower ones in CI, and checks whose result can change without any change to the code, such as vulnerability audits, run on a schedule.
3. **Strict, with exceptions written where they apply.** Checks are configured strictly. A finding is suppressed on its own line, with the reason, and a repository-wide departure is recorded as a decision.
4. **One way to run everything.** Every check is a `make` target, and people, coding agents and CI all run the same targets, so none of them does something the others can't repeat.
5. **Tools never rewrite authored content.** Prose belongs to its authors. Formatters and fixers work on code and configuration, never on writing.
6. **Nothing new is trusted by default.** New releases wait before they are installed, workflows get only the permissions they need, and every dependency is pinned exactly by a lockfile or, where none can hold it, by its one pin.
7. **Decisions are written down.** Adopting a standard, every exception, and every change to the standards are recorded where they can be found later, with their reasons.
8. **Enforced wherever possible.** A rule that nothing checks slowly stops being followed, so a rule that CI can check, CI checks.

### Core tools

A repository that adopts this standard uses these tools for the languages it contains.

| Job | TypeScript / Astro | Python |
|---|---|---|
| Packages and runtimes | pnpm, pinned in the `packageManager` field of `package.json`. Node.js pinned through `devEngines.runtime`, so `pnpm-lock.yaml` records the exact runtime. | uv. Python pinned in `.python-version`. |
| Sites | Astro, with static output | n/a |
| Lint | ESLint with typescript-eslint, plus eslint-plugin-astro for Astro code | Ruff |
| Format | Prettier, plus prettier-plugin-astro for Astro code | Ruff's formatter |
| Type checking | `astro check` for Astro projects, `tsc --noEmit` otherwise, with the strictest available preset | Pyrefly with the `strict` preset, installed through uv and run as `uv run --locked pyrefly check` ([DEC-1](decisions/DEC-1.md)) |
| Unit tests and coverage | Vitest with `@vitest/coverage-v8` | pytest with coverage.py |
| Browser tests | Playwright, for any repository that builds a site | n/a |
| Vulnerability audit | `pnpm audit` | pip-audit |

These apply whatever the language:

| Job | Tool |
|---|---|
| Task runner | Make |
| Git hooks | pre-commit, installed through uv, or through `uvx` with a pinned version in a repository with no Python |
| Dependency updates | Dependabot |
| CI workflow lint | actionlint |
| Site hosting | Cloudflare Workers static assets, deployed with Wrangler |

### Rules

#### Installing and pinning

- **Install through the lockfiles.** Development tools are declared as development dependencies (`devDependencies` in `package.json`, the `dev` group in `pyproject.toml`) and run with `pnpm exec` and `uv run --locked`. `make setup` installs them with `pnpm install --frozen-lockfile` and `uv sync --locked`. Don't install a tool globally, through `npx`, or through `uvx` without a version.
- **Keep every copy of a runtime version in step.** Python's version appears in `.python-version`, `requires-python`, Ruff's `target-version` and Pyrefly's `python-version`; Node.js's in `devEngines.runtime` and often in a container image and a CI step. One of them is the pin, and the others either read it or are checked against it by a test or a step of `make check`, so that they cannot drift apart unnoticed.
- **Stay on current runtimes.** Move to each new stable release of Python and each new Active LTS release of Node.js within six months of it coming out, once the repository's dependencies support it. Write TypeScript as ES modules.
- **Set up development containers with `make setup`**, so that a container, a contributor's machine and CI install the same tools the same way. A repository with code should have one, so that a new contributor can run the tests within ten minutes of cloning. The project also works without it on Linux, macOS and Windows through WSL.
- **Turn off tools' telemetry.** Export `ASTRO_TELEMETRY_DISABLED=1` and `WRANGLER_SEND_METRICS=false` from the `Makefile`, which every check runs through, and do the same for any other tool that reports usage.

#### Checking

- **Run checks strictly and keep them clean.** Pyrefly runs with `preset = "strict"` in `[tool.pyrefly]` and TypeScript in the strictest preset the framework ships, such as Astro's `strictest`. Both stay at zero errors. Suppress a finding explicitly, on the line, with a reason, rather than loosening the configuration.
- **Spell out rule sets.** List Ruff's rules in `select` and the type checker's settings explicitly, instead of relying on defaults. A default that changes in a new release otherwise changes what the checks report without anyone choosing it. Select at least `E`, `W`, `F`, `I`, `UP`, `B`, `SIM`, `RUF` and `S`, with `S101` allowed in tests. Use typescript-eslint's type-aware configurations, `strictTypeChecked` and `stylisticTypeChecked`.
- **Share settings.** Ruff and Prettier both wrap code at 100 columns, and Python tools keep their settings in `pyproject.toml` unless a tool cannot read it.
- **Keep the formatters off prose and generated content.** Each repository lists in its formatter configuration anything a formatter must never touch, such as Markdown and MDX prose, manuscripts, generated files and lockfiles. Hand-aligned data tables in Python may be wrapped in `# fmt: off` and `# fmt: on`. When a formatter is first adopted, list the one large reformatting commit in `.git-blame-ignore-revs`.
- **Keep the file-fixing hooks off prose too.** `trailing-whitespace` removes the two trailing spaces that make a line break in Markdown, and both it and `end-of-file-fixer` rewrite files in place. Limit them with `files:` or `exclude:` to code, in any repository whose prose or generated files they could change.
- **Measure branch coverage and set a floor.** Turn on branch coverage in both languages and set a minimum that the tests fail below. Each repository chooses its own floor, raises it as coverage grows, and lowers it only by recording a decision. Treat coverage as a map of untested code, not as a score.

#### Running

- **Give every check a `make` target.** At minimum: `make setup`, `make lint`, `make format`, `make typecheck`, `make test` and `make check`, where `make check` runs everything CI runs. `make help` lists them all.
- **Run only local hooks, plus the standard file checks.** In `.pre-commit-config.yaml`, every lint, format, type-check and test hook is a `repo: local` hook with `language: system` that calls a `make` target, `uv run --locked` or `pnpm exec`, so it uses the lockfile versions. The one fetched repository allowed is `pre-commit/pre-commit-hooks`, for the standard file checks (valid JSON, YAML and TOML, merge-conflict markers, large files, private keys, trailing whitespace and final newlines), pinned by its `rev:`.
- **Put fast checks at commit time.** The commit stage runs at least the formatters (in check mode), the linters and the type checkers. A repository may run more there, including its tests. A repository that runs its tests at push time instead records that as a decision.
- **Let CI make the final call.** CI runs every part of `make check` on every push and pull request, as separate steps or jobs so that one failure doesn't hide another, and repeats the commit-stage hooks over every file, because anyone can skip a hook with `--no-verify`. CI should finish within ten minutes. A site deploys only from a commit whose CI passed.
- **Audit on a schedule.** A scheduled CI workflow runs `pnpm audit` and pip-audit, through a `make audit` target, at least weekly, and can be started by hand. They don't run before each commit, because new advisories appear without any change to the code. An advisory that has to be ignored is ignored by its ID, with the reason written next to it.

#### Supply chain

- **Pin GitHub Actions to a full commit SHA**, with the version in a comment after it, as in `uses: actions/checkout@<sha> # vX.Y.Z`. A tag can be moved to different code; a SHA cannot. Dependabot updates the SHA and the comment together.
- **Give workflows only the permissions they need.** Set `permissions: contents: read` at the top of every workflow and grant more to a single job only when it needs it. Check out with `persist-credentials: false` unless a later step pushes.
- **Lint the CI workflows.** actionlint checks `.github/workflows/` in the commit stage and in CI. Pin it in `uv.lock` through the `actionlint-py` wheel, once that wheel has been checked like any other new dependency.
- **Wait before installing a new release.** In `pnpm-workspace.yaml`, set `minimumReleaseAge` explicitly (1440 minutes, one day, or longer), so that a release that is hijacked and then pulled is never installed, and set `trustPolicy: no-downgrade`, so that a version published with weaker provenance than an earlier one is refused. Give Dependabot a `cooldown` at least as long, so that it doesn't propose a version pnpm will refuse.
- **Allow install scripts by name.** pnpm runs only the install scripts listed in `pnpm-workspace.yaml`.
- **Update at least monthly, and review every update.** Dependabot covers every ecosystem the repository uses, including GitHub Actions, any development container images and any Dockerfiles. Every update is reviewed and merged only once CI passes. An upgrade that changes what a linter or type checker reports is reviewed like any other change.
- **Add a dependency only when it is maintained, widely used and openly licensed**, judged as in [Choosing a tool](#choosing-a-tool), and say in the pull request what it is for. Don't add one for something the platform or a few lines of code already does.

### Conditional tools

A repository adopts these once it has the code they apply to:

| Tool | Adopt when |
|---|---|
| Vitest | The repository has TypeScript logic to unit test beyond what browser tests check efficiently |
| Hypothesis | Python code processes text or other open-ended input, such as parsing, counting, rewriting links or resolving references |
| `@axe-core/playwright` | The repository builds a site with Playwright tests. Add accessibility checks to those tests. |
| pytest-xdist | The Python test suite is slow enough that running it in parallel helps |
| ShellCheck, through the `shellcheck-py` wheel ([DEC-2](decisions/DEC-2.md)) | The repository has shell scripts, such as a development container's setup script or a CI helper |
| Starlight | The project publishes documentation or a long reference work as a site |
| React | Part of an Astro page needs more interaction than a few lines of browser JavaScript can give, as an island (see the [site standard](#site-standard)), or, built with Vite, the project is an app in which almost every screen is dynamic |

### Not yet covered

This standard names no tool yet for these. A repository that needs one chooses it against [Choosing a tool](#choosing-a-tool) and records the choice as a decision. When a second repository needs the same thing, the choice is proposed for this standard by pull request.

- Linting Dockerfiles
- Checking YAML beyond whether it parses
- Linting or formatting Markdown, which must never reflow prose
- Formatting shell scripts
- Storing large data files outside Git, for which DVC is the first candidate to measure
- Stripping outputs from Jupyter notebooks
- Validating structured data at a program's boundary, in Python or outside Astro's content collections
- Relational databases and their migrations
- Analysing Japanese text, such as tokenisers and dictionaries, whose licences need checking too (see the [licensing standard](#licensing-standard))
- Publishing packages to PyPI or npm, which uses trusted publishing rather than a stored token

### Tools to watch

These newer tools could replace or join their category in a later version of this document. Each entry says what it is waiting for. The list is reviewed once a quarter: a tool that now meets every criterion in [Choosing a tool](#choosing-a-tool), and does its job at least as well as the current tool when tried on a heruhane repository, is proposed by pull request, and a tool that has stopped being developed is removed.

| Tool | Would replace or add | Waiting for |
|---|---|---|
| ty (Astral) | Pyrefly, for Python type checking | A stable release, and the proven and widely used criteria |
| Oxlint and Biome | ESLint and Prettier | Support for Astro files and for the rules heruhane repositories use |
| prek | pre-commit | The proven and widely used criteria |
| Renovate | Dependabot | Evidence that Dependabot cannot keep up with current pnpm releases |
| zizmor | Adds security checks of CI workflows | A trial on heruhane workflows showing that its findings are worth acting on |

## Testing standard

- **Test what changes.** New behaviour arrives with tests, and a bug fix arrives with a test that fails without the fix.
- **Test at the cheapest level that proves the point.** Most logic gets fast unit tests. Integration tests use real dependencies, such as a real database or file format, rather than mocks where they can. Browser tests cover the journeys that matter most, and run against the built site, never the development server.
- **Keep `make check` independent of the outside world.** Its tests do not reach the internet, and do not depend on the date or on the order they run in. A test of a deployed site is a separate target, outside `make check`.
- **Don't live with flaky tests.** A flaky test is fixed or removed within a week. Retrying it until it passes only hides the failure.

## Review standard

- **Have every software change reviewed by someone other than its author** before it reaches `main`: another person, or a separate coding agent that is told the change's purpose and nothing about how it was written.
- **Review what the change claims.** The reviewer checks that the change does what it says and has tests, that names and structure are clear to someone new to the code, that any new dependency is justified, that user-facing changes come with documentation and an accessibility check, and that no personal data is logged, stored or sent anywhere unexpected. A claim that something was tested is checked by running it.
- **Have a person read the riskiest changes.** Security fixes, and changes to CI permissions, deployment, releases or the handling of secrets, should also be read by a person before they reach `main`.

## Coding agent standard

- **The same rules apply to coding agents.** The person who asks for a change is responsible for it, whoever wrote it.
- **Name the agent.** Every commit an agent makes ends with a `Co-Authored-By:` trailer naming the agent and model, and anything an agent posts under a person's account says who wrote it.
- **Don't paste private data into AI tools.** That covers personal data, learner data and secrets.

## Japanese language content standard

These rules apply to any repository that shows, teaches or processes Japanese.

- **Have a native speaker check the Japanese that learners read.** No automated check can tell a natural sentence from one nobody would say. A project names who has the final word on its language content, and records what has been checked. Content is not finished until it has been, and only a speaker's own judgement marks it as checked.
- **Keep known answers for anything that scores, marks or explains Japanese.** A fixture set of questions and answers, checked by a language expert, runs in the tests, and a fixture changes only with that expert's agreement.
- **Mark the language.** Put `lang="ja"` on every run of Japanese in a page whose language is something else. A screen reader may otherwise use the wrong voice, and a browser may draw the text with Chinese forms of the characters. Do this in the build where possible, and check it with a browser test.
- **Use real text.** Japanese is text, never an image of text, and furigana set above the line uses `<ruby>`, `<rt>` and `<rp>` rather than positioned elements, so that it reflows and can be searched, copied and read aloud.
- **Don't send a whole Japanese font.** A complete one runs to several megabytes. Give `:lang(ja)` a stack of system fonts, or self-host a font split into Unicode-range subsets, as Fontsource packages are, and try the page on a slow connection.
- **Compare and count deliberately.** が can be one code point or two, so normalise to NFC before comparing, searching or counting, without rewriting the stored text. Count characters with `Intl.Segmenter` rather than `.length`, which counts 𠮷 as two. Japanese has no spaces between words, so splitting on whitespace does not find them.

## Site standard

These rules apply to any repository that builds a website or web app. Those that name Astro features apply to sites built with it.

- **Build static output**, and deploy it with Wrangler, with its settings in `wrangler.jsonc`. Keep the output portable to another static host. A server adapter needs a decision (see the [service standard](#service-standard)).
- **Send no JavaScript unless a feature needs it.** Pages work with JavaScript turned off, and a feature that can't shows a sensible message. Use plain `<script>` tags for small behaviour such as toggles, and React islands for more, never a second UI framework. Choose the latest-loading directive that works: none, then `client:visible`, then `client:idle`, and `client:load` only at the top of a page, with the reason in the pull request.
- **Stay under 200 KB of compressed JavaScript on first load** on every page the public visits. CI should check this against the build output. An editor's admin pages are not counted.
- **Validate content at build time.** Content of a repeated shape, such as lessons or posts, belongs to a content collection with a schema using `z` from `astro/zod`, so that a page with missing or invalid frontmatter fails the build. Use Markdown by default, and MDX only where a page embeds components.
- **Keep logic out of components.** Put it in plain TypeScript modules in `src/lib/`, where Vitest can test it, and keep `.astro` files to markup.
- **Use one base layout** that sets `lang`, a skip link, landmarks and a visible focus style.
- **Use `<Image />` and `<Picture />` from `astro:assets` for content images**, which makes them responsive and compressed and requires `alt`. Use `alt=""` only for decorative images.
- **Load nothing from third parties.** Fonts and scripts are self-hosted, and nothing on a page calls another company's servers while it loads.
- **Set security headers.** Serve HTTPS only, with HSTS, a strict Content Security Policy using Astro's built-in support, and `X-Content-Type-Options: nosniff`. Use `set:html` only with sanitised content.
- **Help search engines and links.** Set `site`, and `base` if needed, in `astro.config.mjs`, and add `@astrojs/sitemap` and a `robots.txt` to public sites.

## Accessibility standard

These rules apply to every web interface.

- **Meet WCAG 2.2 Level AA.** `@axe-core/playwright` checks the key pages in the browser tests, and a violation fails them.
- **Check new interface features by hand** before release: with the keyboard only, with a screen reader (VoiceOver or NVDA), at 200% and 400% zoom, and with reduced motion turned on. Animation and view transitions respect `prefers-reduced-motion`.
- **Don't rely on colour or time.** Colour alone never carries meaning, such as a right or wrong answer. Avoid time limits, or let the user turn them off.
- **Write to be read.** Use plain language for the audience, a logical heading order, descriptive link text and alt text, in documentation as well as on pages.
- **Publish an accessibility statement** for each public site or service.

## Privacy standard

- **Collect as little as possible.** Prefer software that runs on the user's device and keeps its data there.
- **Don't track.** No third-party advertising or trackers, no profiling, no public learner profiles, and the most private option as the default setting. Usage analytics need a decision, are opt-in and anonymous, and are documented in the README. Distributed software sends no telemetry by default.
- **Review before handling personal data.** Any feature that processes personal data, including a contact form or a third party that receives it, first gets a privacy review, recorded as a decision: what data, why, where it is stored, how long it is kept and who can access it. heruhane works from the UK, so UK GDPR applies.
- **Take more care with children.** A service likely to be accessed by under-18s also completes a Data Protection Impact Assessment and follows the ICO's Children's Code.
- **Keep personal data out of logs, fixtures and tools.** Logs contain no personal data, learner answers, free-text input, tokens or full IP addresses. Test fixtures and demo data are synthetic, and reporters are asked to remove personal data from bug reports. Learner data never trains an AI model.

## Security standard

- **Protect accounts.** Everyone with admin or publishing rights uses phishing-resistant two-factor authentication (a passkey or hardware key) on GitHub, Cloudflare, domain registrars and package registries.
- **Keep two people able to recover everything.** At least two people have admin access to every repository, domain, hosting account and package. Recovery codes are kept in a shared password manager, and it is written down who to contact if the lead maintainer becomes unavailable.
- **Keep secrets out of the repository.** Don't put them in code, issues or CI logs. Use GitHub environment secrets, and OIDC wherever a provider supports it. If a secret leaks, revoke it first and clean up afterwards. A secret pushed to a public repository has leaked, however quickly it is removed.
- **Validate data from outside the program** at the boundary, before anything relies on it. That covers files, API responses and `localStorage` as well as user input.

## Licensing standard

These rules apply from the first time a repository uses third-party material, because where something came from is hard to work out later. A repository has its own licences by the time it [goes public](#public-repository-standard).

- **Code is MIT. Books and question banks are CC BY-SA 4.0.** A repository with both keeps them in `LICENSE` for the content and `LICENSE-CODE` for the code. Anything else, including lessons, needs a decision.
- **Record third-party content.** Images, datasets and fonts have their source and licence recorded in `ATTRIBUTION.md` or its equivalent. Check the licence before using them: a share-alike dataset, such as JMdict, binds what is built from it.
- **Keep what ships compatible.** Anything distributed to users has a licence compatible with the repository's. GPL, AGPL, SSPL or a non-commercial licence in distributed code needs a decision. A development tool only needs an open licence (see [Choosing a tool](#choosing-a-tool)).

## Release standard

These rules apply to any repository whose output other people use.

- **Version with Semantic Versioning 2.0.** Record in a decision what the public interface is: a site's URLs and anchors, a tool's commands or a package's API.
- **Release as one unit.** A release is a version bump, an entry in `CHANGELOG.md` in the Keep a Changelog format, a `vX.Y.Z` tag, and a GitHub release with that entry as its notes.

## Documentation standard

- **Name repositories** in lowercase kebab-case.
- **Give every repository a README** that says what it is, who it is for and how to start, with commands given by their `make` target.
- **Change the documentation with the code**, in the same change.
- **Record significant decisions**: anything expensive to reverse, that affects users, adds a major dependency or changes how data is handled.
- **Keep out of Git:** build output, secrets, `.env` files and large binaries.
- **Prefer open data formats**, such as JSON, CSV, Markdown and SQLite, so that people can take their data elsewhere. Data shared between Python and TypeScript has one schema, not two kept in step by hand.

## Service standard

Prefer a library or a static site, which need nobody to keep them running. A project that needs a server or stores users' data first records a decision saying why, with a privacy review and a short threat model. Then:

- **Run it safely.** Serve HTTPS only, with HSTS and a strict Content Security Policy. Follow the OWASP Top 10 and validate all input on the server. Prefer sign-in through an identity provider, and hash any passwords that can't be avoided with Argon2id.
- **Keep it running.** Define infrastructure as code in the repository, and keep only preview and production environments, with no real data in preview. Give it a health check and uptime alerts, daily backups of anything it stores with a restore test every six months, and a `docs/runbook.md` covering deployment, rollback and restoring.
- **Say what users can expect.** The README states the support on offer, such as "Best effort, working days, UK time".
- **Handle incidents openly.** Fix the problem, tell the people affected, and publish a short blameless review within ten working days. Report a personal data breach to the ICO within 72 hours where the law requires it.

## Public repository standard

Before a repository becomes public:

1. **Search its whole history** for secrets, personal data and private material, not only the latest commit. Going public publishes all of it.
2. **Add its licences** (see the [licensing standard](#licensing-standard)), and a support statement in the README.
3. **Provide `SECURITY.md`, `CODE_OF_CONDUCT.md` and `CONTRIBUTING.md`**, its own or the organisation's defaults in `heruhane/.github`. The code of conduct is the Contributor Covenant.
4. **Plan to turn on GitHub's security features** the moment it is public: private vulnerability reporting, secret scanning with push protection, Dependabot security updates, CodeQL, and dependency review that blocks known high or critical vulnerabilities. They are free on public repositories and paid or unavailable on private ones.
5. **Check its workflows for `pull_request_target`**, and never use it with code from forks.

## Exceptions

A repository may differ from a standard it has adopted only through a decision in its own decision record. Not adopting a standard is not an exception. The decision names the rule or tool it departs from, says why, and says when the exception ends:

- **A temporary exception** sets a condition that ends it, such as a stable release, a measured threshold or the completion of a migration. Adopting Pyrefly's `strict` preset in a large existing codebase one directory at a time is an example.
- **A lasting exception** is for a difference in kind between the repository and the code the standard was written for, such as a repository whose main content is prose. It gives a review date no more than a year away, and is reviewed then.

When the same exception appears in more than one repository, it is a sign that the standard is wrong, and belongs in a pull request to this document instead.

## Version history

- **Draft.** Moved the tooling standard into this document, `STANDARDS.md`, as the first of a set of separate standards with one version between them. It keeps the rules of version 2026-10-28.3, and the new standards cover testing, review, coding agents, Japanese language content, sites, accessibility, privacy, security, licensing, releases, documentation, services and public repositories, with principles behind them all. A repository can adopt any of them. Added Astro and Cloudflare hosting to the core tools, and Starlight and React to the conditional tools. Added private-key detection to the standard file checks, a minimum Ruff rule set, type-aware linting, one line width, current runtimes, and telemetry turned off. A repository that adopted an earlier version has adopted the tooling standard, and moves to this version in the usual way.
- **2026-10-28.3.** Split the principles into criteria for choosing a tool, each with a test, and principles for running tools, with the maintenance a tool needs scaled to the cost of replacing it; moved the Pyrefly rationale to [DEC-1](decisions/DEC-1.md), the first file in [`decisions/`](decisions/). Added a scope section covering repositories of every kind, rules for moving to a newer version, and what a decision record contains. Grouped the rules, and added [Supply chain](#supply-chain) rules: actions pinned to a commit SHA, least-privilege workflow permissions, a minimum release age for pnpm and a matching Dependabot cooldown. Required runtime versions to be kept in step, rule sets to be spelled out, file-fixing hooks to be kept off prose, and coverage floors only to rise. Changed updates from monthly to at least monthly. Added ShellCheck as a conditional tool ([DEC-2](decisions/DEC-2.md)), a list of jobs not yet covered, and a review process for the tools to watch. Allowed lasting exceptions with a yearly review.
- **2026-10-28.2.** Made Pyrefly the standard Python type checker, in place of Pyright, and replaced version numbers with the merge date.
- **Version 1**, 28 October 2026. First version.
