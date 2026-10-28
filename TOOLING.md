# heruhane tooling standard

*Version 2026-10-28.3.*

This standard names one tool for each job in each language, and one way of running it, for every heruhane repository. A project that mixes Python and TypeScript can easily end up with two toolchains that work differently, pin versions in different places and run their checks at different times. Using the same tools in the same way everywhere means that someone who knows one heruhane repository can work in any of them, and that a fix found in one repository can be copied to the others.

The reasons behind individual tool choices are recorded in [`decisions/`](decisions/), one file per decision, so that this document can say only what a repository must do.

## Scope

heruhane repositories come in a few kinds: TypeScript or Astro sites, Python tools and data projects, projects that combine the two, and repositories that are mostly writing, such as books and course material, with a build system around them. The standard applies to each as follows:

- **The language rows** of [Core tools](#core-tools) apply to each language a repository contains. A Python-only repository has no Node.js toolchain, and a site with no Python has no uv.
- **The rules that do not depend on a language** apply to every repository that has the files they cover: Make, pre-commit with the standard file checks, Dependabot for GitHub Actions, actionlint for workflows, and the [Supply chain](#supply-chain) rules. A repository with no code yet adopts these when it gets its first workflow or script.
- **Writing is never formatted by a tool.** Where a repository's main content is prose, the formatters and the file-fixing hooks are kept off it (see [Rules](#rules)).

## Adopting the standard

A repository adopts the standard by recording a decision that says so, naming the version it follows and listing any exceptions (see [Exceptions](#exceptions)). To move to a newer version, it reads the [Version history](#version-history) for the changes since its current version and records the move in a new decision, together with anything it has not yet done. A repository does not have to adopt a new version on the day it is published, but it checks for one at least once a quarter.

A decision can be kept in whatever form the repository already uses for its decisions, such as a file in a `docs/decisions/` folder. Each one gives at least:

- a title and the date it was made;
- what was decided, in one or two sentences;
- the reason;
- for an exception, the rule it departs from and its end condition or review date (see [Exceptions](#exceptions));
- for a change to an earlier decision, which decision it replaces.

Changes to the standard are made by pull request to this document. Each merged change gives it a new version: the date the change is merged, written as `YYYY-MM-DD`. When more than one change is merged on the same day, the second adds the suffix `.2`, the third `.3`, and so on, as in `2026-10-28.2`. The pull request updates the version line, adds an entry to the version history, and adds a decision to `decisions/` when it chooses or replaces a tool. A decision is a file named `DEC-N.md`, numbered in order, in the shape of [DEC-1](decisions/DEC-1.md). It is never rewritten afterwards: a later decision that changes it names it in `amends` or `supersedes`, and the earlier file gains the matching `amended_by` or `superseded_by`.

## Principles

The principles come in two kinds. The first list says how a tool is chosen for the standard. The second says how the chosen tools are run, and the [Rules](#rules) carry it out.

### Choosing a tool

A tool joins the standard only when it meets all six criteria. Each is meant to be checked, not judged by impression.

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
7. **Decisions are written down.** Adopting the standard, every exception, and every change to the standard are recorded where they can be found later, with their reasons.

## Core tools

Every repository uses these tools for the languages it contains.

| Job | TypeScript / Astro | Python |
|---|---|---|
| Packages and runtimes | pnpm, pinned in the `packageManager` field of `package.json`. Node.js pinned through `devEngines.runtime`, so `pnpm-lock.yaml` records the exact runtime. | uv. Python pinned in `.python-version`. |
| Lint | ESLint with typescript-eslint, plus eslint-plugin-astro for Astro code | Ruff |
| Format | Prettier, plus prettier-plugin-astro for Astro code | Ruff's formatter |
| Type checking | `astro check` for Astro projects, `tsc --noEmit` otherwise, with the strictest available preset | Pyrefly with the `strict` preset, installed through uv ([DEC-1](decisions/DEC-1.md)) |
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

## Rules

### Installing and pinning

- **Install through the lockfiles.** Development tools are declared as development dependencies (`devDependencies` in `package.json`, the `dev` group in `pyproject.toml`) and run with `pnpm exec` and `uv run --locked`. `make setup` installs them with `pnpm install --frozen-lockfile` and `uv sync --locked`. Don't install a tool globally, through `npx`, or through `uvx` without a version.
- **Keep every copy of a runtime version in step.** Python's version appears in `.python-version`, `requires-python`, Ruff's `target-version` and Pyrefly's `python-version`; Node.js's in `devEngines.runtime` and often in a container image and a CI step. One of them is the pin, and the others either read it or are checked against it by a test or a step of `make check`, so that they cannot drift apart unnoticed.
- **Install the Python checker through uv.** Pyrefly is a development dependency in `pyproject.toml` and runs as `uv run --locked pyrefly check`. Its wheel contains the native binary, so `uv.lock` pins it and checking Python needs no Node.js or `node_modules`.
- **Set up development containers with `make setup`**, so that a container, a contributor's machine and CI install the same tools the same way.

### Checking

- **Run checks strictly and keep them clean.** Pyrefly runs with `preset = "strict"` in `[tool.pyrefly]` and TypeScript in the strictest preset the framework ships, such as Astro's `strictest`. Both stay at zero errors. Suppress a finding explicitly, on the line, with a reason, rather than loosening the configuration.
- **Spell out rule sets.** List Ruff's rules in `select` and the type checker's settings explicitly, instead of relying on defaults. A default that changes in a new release otherwise changes what the checks report without anyone choosing it.
- **Keep the formatters off prose and generated content.** Each repository lists in its formatter configuration anything a formatter must never touch, such as Markdown and MDX prose, manuscripts, generated files and lockfiles. Hand-aligned data tables in Python may be wrapped in `# fmt: off` and `# fmt: on`. When a formatter is first adopted, list the one large reformatting commit in `.git-blame-ignore-revs`.
- **Keep the file-fixing hooks off prose too.** `trailing-whitespace` removes the two trailing spaces that make a line break in Markdown, and both it and `end-of-file-fixer` rewrite files in place. Limit them with `files:` or `exclude:` to code, in any repository whose prose or generated files they could change.
- **Measure branch coverage and set a floor.** Turn on branch coverage in both languages and set a minimum that the tests fail below. Each repository chooses its own floor, raises it as coverage grows, and lowers it only by recording a decision. Treat coverage as a map of untested code, not as a score.

### Running

- **Give every check a `make` target.** At minimum: `make setup`, `make lint`, `make format`, `make typecheck`, `make test` and `make check`, where `make check` runs everything CI runs. `make help` lists them all.
- **Run only local hooks, plus the standard file checks.** In `.pre-commit-config.yaml`, every lint, format, type-check and test hook is a `repo: local` hook with `language: system` that calls a `make` target, `uv run --locked` or `pnpm exec`, so it uses the lockfile versions. The one fetched repository allowed is `pre-commit/pre-commit-hooks`, for the standard file checks (valid JSON, YAML and TOML, merge-conflict markers, large files, trailing whitespace and final newlines), pinned by its `rev:`.
- **Put fast checks at commit time.** The commit stage runs at least the formatters (in check mode), the linters and the type checkers. A repository may run more there, including its tests. A repository that runs its tests at push time instead records that as a decision.
- **Let CI make the final call.** CI runs every part of `make check` on every push and pull request, as separate steps or jobs so that one failure doesn't hide another, and repeats the commit-stage hooks over every file, because anyone can skip a hook with `--no-verify`. A site deploys only from a commit whose CI passed.
- **Audit on a schedule.** A scheduled CI workflow runs `pnpm audit` and pip-audit, through a `make audit` target, at least weekly, and can be started by hand. They don't run before each commit, because new advisories appear without any change to the code. An advisory that has to be ignored is ignored by its ID, with the reason written next to it.

### Supply chain

- **Pin GitHub Actions to a full commit SHA**, with the version in a comment after it, as in `uses: actions/checkout@<sha> # vX.Y.Z`. A tag can be moved to different code; a SHA cannot. Dependabot updates the SHA and the comment together.
- **Give workflows only the permissions they need.** Set `permissions: contents: read` at the top of every workflow and grant more to a single job only when it needs it. Check out with `persist-credentials: false` unless a later step pushes.
- **Lint the CI workflows.** actionlint checks `.github/workflows/` in the commit stage and in CI. Pin it in `uv.lock` through the `actionlint-py` wheel, once that wheel has been checked like any other new dependency.
- **Wait before installing a new release.** In `pnpm-workspace.yaml`, set `minimumReleaseAge` explicitly (1440 minutes, one day, or longer), so that a release that is hijacked and then pulled is never installed, and set `trustPolicy: no-downgrade`, so that a version published with weaker provenance than an earlier one is refused. Give Dependabot a `cooldown` at least as long, so that it doesn't propose a version pnpm will refuse.
- **Allow install scripts by name.** pnpm runs only the install scripts listed in `pnpm-workspace.yaml`.
- **Update at least monthly, and review every update.** Dependabot covers every ecosystem the repository uses, including GitHub Actions, any development container images and any Dockerfiles. Every update is reviewed and merged only once CI passes. An upgrade that changes what a linter or type checker reports is reviewed like any other change.
- **Add a dependency only when it is maintained, widely used and openly licensed**, judged as in [Choosing a tool](#choosing-a-tool), and say in the pull request what it is for.

## Conditional tools

A repository adopts these once it has the code they apply to:

| Tool | Adopt when |
|---|---|
| Vitest | The repository has TypeScript logic to unit test beyond what browser tests check efficiently |
| Hypothesis | Python code processes text or other open-ended input, such as parsing, counting, rewriting links or resolving references |
| `@axe-core/playwright` | The repository builds a site with Playwright tests. Add accessibility checks to those tests. |
| pytest-xdist | The Python test suite is slow enough that running it in parallel helps |
| ShellCheck, through the `shellcheck-py` wheel ([DEC-2](decisions/DEC-2.md)) | The repository has shell scripts, such as a development container's setup script or a CI helper |

## Not yet covered

The standard names no tool yet for these. A repository that needs one chooses it against [Choosing a tool](#choosing-a-tool) and records the choice as a decision. When a second repository needs the same thing, the choice is proposed for this standard by pull request.

- Linting Dockerfiles
- Checking YAML beyond whether it parses
- Linting or formatting Markdown, which must never reflow prose
- Formatting shell scripts

## Exceptions

A repository may differ from this standard only through a decision in its own decision record. The decision names the rule or tool it departs from, says why, and says when the exception ends:

- **A temporary exception** sets a condition that ends it, such as a stable release, a measured threshold or the completion of a migration. Adopting Pyrefly's `strict` preset in a large existing codebase one directory at a time is an example.
- **A lasting exception** is for a difference in kind between the repository and the code the standard was written for, such as a repository whose main content is prose. It gives a review date no more than a year away, and is reviewed then.

When the same exception appears in more than one repository, it is a sign that the standard is wrong, and belongs in a pull request to this document instead.

## Tools to watch

These newer tools could replace or join their category in a later version of the standard. Each entry says what it is waiting for. The list is reviewed once a quarter: a tool that now meets every criterion in [Choosing a tool](#choosing-a-tool), and does its job at least as well as the current tool when tried on a heruhane repository, is proposed by pull request, and a tool that has stopped being developed is removed.

| Tool | Would replace or add | Waiting for |
|---|---|---|
| ty (Astral) | Pyrefly, for Python type checking | A stable release, and the proven and widely used criteria |
| Oxlint and Biome | ESLint and Prettier | Support for Astro files and for the rules heruhane repositories use |
| prek | pre-commit | The proven and widely used criteria |
| Renovate | Dependabot | Evidence that Dependabot cannot keep up with current pnpm releases |
| zizmor | Adds security checks of CI workflows | A trial on heruhane workflows showing that its findings are worth acting on |

## Version history

- **2026-10-28.3.** Split the principles into criteria for choosing a tool, each with a test, and principles for running tools, with the maintenance a tool needs scaled to the cost of replacing it; moved the Pyrefly rationale to [DEC-1](decisions/DEC-1.md), the first file in [`decisions/`](decisions/). Added a scope section covering repositories of every kind, rules for moving to a newer version, and what a decision record contains. Grouped the rules, and added [Supply chain](#supply-chain) rules: actions pinned to a commit SHA, least-privilege workflow permissions, a minimum release age for pnpm and a matching Dependabot cooldown. Required runtime versions to be kept in step, rule sets to be spelled out, file-fixing hooks to be kept off prose, and coverage floors only to rise. Changed updates from monthly to at least monthly. Added ShellCheck as a conditional tool ([DEC-2](decisions/DEC-2.md)), a list of jobs not yet covered, and a review process for the tools to watch. Allowed lasting exceptions with a yearly review.
- **2026-10-28.2.** Made Pyrefly the standard Python type checker, in place of Pyright, and replaced version numbers with the merge date.
- **Version 1**, 28 October 2026. First version.
