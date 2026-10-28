# heruhane tooling standard

*Version 2026-10-28.3.*

This standard names one tool for each job in each language, and one way of running it, for every heruhane repository. A project that mixes Python and TypeScript can easily end up with two toolchains that work differently, pin versions in different places and run their checks at different times. Using the same tools in the same way everywhere means that someone who knows one heruhane repository can work in any of them, and that a fix found in one repository can be copied to the others.

## Scope

heruhane repositories come in a few kinds: TypeScript or Astro sites, Python tools and data projects, projects that combine the two, and repositories that are mostly writing, such as books and course material, with a build system around them. The standard applies to each as follows:

- **The language rows** of [Core tools](#core-tools) apply to each language a repository contains. A Python-only repository has no Node.js toolchain, and a site with no Python has no uv.
- **The rules that do not depend on a language** apply to every repository that has the files they cover: Make, pre-commit with the standard file checks, Dependabot for GitHub Actions, actionlint for workflows, and the [Supply chain](#supply-chain) rules. A repository with no code yet adopts these when it gets its first workflow or script.
- **Writing is never formatted by a tool.** Where a repository's main content is prose, the formatters and the file-fixing hooks are kept off it (see [Rules](#rules)).

## Adopting the standard

A repository adopts the standard by recording a decision that says so, naming the version it follows and listing any exceptions (see [Exceptions](#exceptions)). To move to a newer version, it reads the [Version history](#version-history) for the changes since its current version and records the move in a new decision, together with anything it has not yet done. A repository does not have to adopt a new version on the day it is published, but it checks for one at least once a quarter.

Changes to the standard are made by pull request to this document. Each merged change gives it a new version: the date the change is merged, written as `YYYY-MM-DD`. When more than one change is merged on the same day, the second adds the suffix `.2`, the third `.3`, and so on, as in `2026-10-28.2`. The pull request updates the version line and adds an entry to the version history.

## Principles

The tools are chosen on five criteria:

1. **Open source.**
2. **Mature and stable.** When a newer tool is faster but less proven, the proven tool wins until the newer one has been stable for a while.
3. **Fast enough** to run on every commit.
4. **Widely used with a clear future**, ideally backed by a well-funded organisation.
5. **One version pin per tool, in a lockfile wherever one can hold it**, so that development containers, contributors' machines, coding agents and CI all run the same version. When a tool can't live in a lockfile (uv, pnpm, Make, a hook fetched from another repository, a GitHub Action, a container base image), it still has exactly one pin, and the repository's documentation says where.

## Core tools

Every repository uses these tools for the languages it contains.

| Job | TypeScript / Astro | Python |
|---|---|---|
| Packages and runtimes | pnpm, pinned in the `packageManager` field of `package.json`. Node.js pinned through `devEngines.runtime`, so `pnpm-lock.yaml` records the exact runtime. | uv. Python pinned in `.python-version`. |
| Lint | ESLint with typescript-eslint, plus eslint-plugin-astro for Astro code | Ruff |
| Format | Prettier, plus prettier-plugin-astro for Astro code | Ruff's formatter |
| Type checking | `astro check` for Astro projects, `tsc --noEmit` otherwise, with the strictest available preset | Pyrefly with the `strict` preset, installed through uv |
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

Pyrefly replaced Pyright as the Python type checker because it now meets the criteria better:

- **Mature and stable.** Pyrefly reached a stable 1.0 in May 2026 after a public beta, and has had regular releases since. Its type checking is in daily use on large codebases, including Instagram, PyTorch and JAX.
- **Fast.** It is written in Rust and checks a project in a fraction of the time Pyright takes, so it stays comfortably fast enough for the commit stage as a repository grows.
- **Backed by a well-funded organisation.** Meta develops it in the open under the MIT licence, and it is widely used across the Python ecosystem.
- **Simpler to install.** It ships as a platform wheel containing a single native binary, so it needs no Node.js runtime alongside it.

ty, Astral's Rust-based checker, remains under watch until it has a stable release.

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

- **Pin GitHub Actions to a full commit SHA**, with the version in a comment after it, as in `uses: actions/checkout@<sha> # v5.0.0`. A tag can be moved to different code; a SHA cannot. Dependabot updates the SHA and the comment together.
- **Give workflows only the permissions they need.** Set `permissions: contents: read` at the top of every workflow and grant more to a single job only when it needs it. Check out with `persist-credentials: false` unless a later step pushes.
- **Lint the CI workflows.** actionlint checks `.github/workflows/` in the commit stage and in CI. Pin it in `uv.lock` through the `actionlint-py` wheel, once that wheel has been checked like any other new dependency.
- **Wait before installing a new release.** In `pnpm-workspace.yaml`, set `minimumReleaseAge` explicitly (1440 minutes, one day, or longer), so that a release that is hijacked and then pulled is never installed, and set `trustPolicy: no-downgrade`, so that a version published with weaker provenance than an earlier one is refused. Give Dependabot a `cooldown` at least as long, so that it doesn't propose a version pnpm will refuse.
- **Allow install scripts by name.** pnpm runs only the install scripts listed in `pnpm-workspace.yaml`.
- **Update at least monthly, and review every update.** Dependabot covers every ecosystem the repository uses, including GitHub Actions, any development container images and any Dockerfiles. Every update is reviewed and merged only once CI passes. An upgrade that changes what a linter or type checker reports is reviewed like any other change.
- **Add a dependency only when it meets the criteria above.** Check that a new package is maintained, widely used and openly licensed before adding it, and say in the pull request what it is for.

## Conditional tools

A repository adopts these once it has the code they apply to:

| Tool | Adopt when |
|---|---|
| Vitest | The repository has TypeScript logic to unit test beyond what browser tests check efficiently |
| Hypothesis | Python code processes text or other open-ended input, such as parsing, counting, rewriting links or resolving references |
| `@axe-core/playwright` | The repository builds a site with Playwright tests. Add accessibility checks to those tests. |
| pytest-xdist | The Python test suite is slow enough that running it in parallel helps |
| ShellCheck, through the `shellcheck-py` wheel | The repository has shell scripts, such as a development container's setup script or a CI helper |

## Exceptions

A repository may differ from this standard only through a decision in its own decision record. The decision names the rule or tool it departs from, says why, and says when the exception ends:

- **A temporary exception** sets a condition that ends it, such as a stable release, a measured threshold or the completion of a migration. Adopting Pyrefly's `strict` preset in a large existing codebase one directory at a time is an example.
- **A lasting exception** is for a difference in kind between the repository and the code the standard was written for, such as a repository whose main content is prose. It gives a review date no more than a year away, and is reviewed then.

When the same exception appears in more than one repository, it is a sign that the standard is wrong, and belongs in a pull request to this document instead.

## Tools to watch

These newer tools are faster, but they don't yet meet the maturity criterion. Each could replace or join its category in a later version of the standard:

- **ty** (Astral), for Python type checking
- **Oxlint** and **Biome**, for TypeScript linting and formatting
- **prek**, a faster drop-in replacement for pre-commit
- **Renovate**, if Dependabot can't keep up with current pnpm releases
- **zizmor**, for security checks of CI workflows

## Version history

- **2026-10-28.3.** Added a scope section covering repositories of every kind, and rules for moving to a newer version. Split the rules into sections, and added [Supply chain](#supply-chain) rules: actions pinned to a commit SHA, least-privilege workflow permissions, a minimum release age for pnpm and a matching Dependabot cooldown. Required runtime versions to be kept in step, rule sets to be spelled out, file-fixing hooks to be kept off prose, and coverage floors only to rise. Changed updates from monthly to at least monthly. Added ShellCheck as a conditional tool. Allowed lasting exceptions with a yearly review.
- **2026-10-28.2.** Made Pyrefly the standard Python type checker, in place of Pyright, and replaced version numbers with the merge date.
- **Version 1**, 28 October 2026. First version.
