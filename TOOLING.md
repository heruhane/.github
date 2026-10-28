# heruhane tooling standard

*Version 2026-10-28.2.*

This standard names one tool for each job in each language, for every heruhane repository that contains Python or TypeScript code. A project that mixes the two languages can easily end up with two toolchains that work differently, pin versions in different places and run their checks at different times. Using the same tools in the same way everywhere means that someone who knows one heruhane repository can work in any of them.

A repository adopts the standard by recording a decision that says so, naming the version it follows and listing any exceptions (see [Exceptions](#exceptions)). Changes to the standard are made by pull request to this document, and each change gives it a new version: the date the change is merged, written as `YYYY-MM-DD`. When more than one change is merged on the same day, the second adds the suffix `.2`, the third `.3`, and so on, as in `2026-10-28.2`.

## Principles

The tools are chosen on five criteria:

1. **Open source.**
2. **Mature and stable.** When a newer tool is faster but less proven, the proven tool wins until the newer one has been stable for a while.
3. **Fast enough** to run on every commit.
4. **Widely used with a clear future**, ideally backed by a well-funded organisation.
5. **One version pin per tool, in a lockfile wherever one can hold it**, so that development containers, contributors' machines, coding agents and CI all run the same version. When a tool can't live in a lockfile (uv, Make, a hook fetched from another repository, a GitHub Action), it still has exactly one pin, and the repository's documentation says where.

## Core tools

Every repository uses these tools for the languages it contains.

| Job | TypeScript / Astro | Python |
|---|---|---|
| Packages and runtimes | pnpm. Node.js pinned through `devEngines.runtime`, so `pnpm-lock.yaml` records the exact runtime. | uv. Python pinned in `.python-version`. |
| Lint | ESLint with typescript-eslint, plus eslint-plugin-astro for Astro code | Ruff |
| Format | Prettier, plus prettier-plugin-astro for Astro code | Ruff's formatter |
| Type checking | `astro check` for Astro projects, `tsc --noEmit` otherwise, with the strictest available preset | Pyrefly with the `strict` preset, installed through uv |
| Unit tests and coverage | Vitest with `@vitest/coverage-v8` | pytest with coverage.py |
| Browser tests | Playwright, for any repository that builds a site | n/a |
| Task runner | Make, for both languages | |
| Git hooks | pre-commit, for both languages | |
| Dependency updates | Dependabot, for both languages | |
| Vulnerability audit | `pnpm audit` | pip-audit |
| CI workflow lint | actionlint | |

Pyrefly replaced Pyright as the Python type checker because it now meets the criteria better:

- **Mature and stable.** Pyrefly reached a stable 1.0 in May 2026 after a public beta, and has had regular releases since. Its type checking is in daily use on large codebases, including Instagram, PyTorch and JAX.
- **Fast.** It is written in Rust and checks a project in a fraction of the time Pyright takes, so it stays comfortably fast enough for the commit stage as a repository grows.
- **Backed by a well-funded organisation.** Meta develops it in the open under the MIT licence, and it is widely used across the Python ecosystem.
- **Simpler to install.** It ships as a platform wheel containing a single native binary, so it needs no Node.js runtime alongside it.

ty, Astral's Rust-based checker, remains under watch until it has a stable release.

## Rules

- **Install through the lockfiles.** Development tools are declared as development dependencies (`devDependencies` in `package.json`, the `dev` group in `pyproject.toml`) and run with `pnpm exec` and `uv run --locked`. `make setup` installs them with `pnpm install --frozen-lockfile` and `uv sync --locked`. Don't install a tool globally, through `npx`, or through `uvx` without a version.
- **Allow install scripts by name.** pnpm runs only the install scripts listed in `pnpm-workspace.yaml`.
- **Install the Python checker through uv.** Pyrefly is a development dependency in `pyproject.toml` and runs as `uv run --locked pyrefly check`. Its wheel contains the native binary, so `uv.lock` pins it and checking Python needs no Node.js or `node_modules`.
- **Run checks strictly and keep them clean.** Pyrefly runs with `preset = "strict"` in `[tool.pyrefly]` and TypeScript in the strictest preset the framework ships, such as Astro's `strictest`. Both stay at zero errors. Suppress a finding explicitly, with a reason, rather than loosening the configuration.
- **Keep the formatters off prose and generated content.** Each repository lists in its formatter configuration anything a formatter must never touch, such as Markdown prose, manuscripts, generated files and lockfiles. Hand-aligned data tables in Python may be wrapped in `# fmt: off` and `# fmt: on`. When a formatter is first adopted, list the one large reformatting commit in `.git-blame-ignore-revs`.
- **Measure branch coverage and set a floor.** Turn on branch coverage in both languages and set a minimum that the tests fail below. Each repository chooses its own floor. Treat coverage as a map of untested code, not as a score.
- **Give every check a `make` target.** At minimum: `make setup`, `make lint`, `make format`, `make typecheck`, `make test` and `make check`, where `make check` runs everything CI runs. `make help` lists them all.
- **Run only local hooks, plus the standard file checks.** In `.pre-commit-config.yaml`, every lint, format, type-check and test hook is a `repo: local` hook with `language: system` that calls a `make` target, `uv run --locked` or `pnpm exec`, so it uses the lockfile versions. The one fetched repository allowed is `pre-commit/pre-commit-hooks`, for the standard file checks (valid JSON, YAML and TOML, merge-conflict markers, large files, trailing whitespace and final newlines), pinned by its `rev:`. pre-commit itself is installed through uv.
- **Put fast checks at commit time.** The commit stage runs at least the formatters (in check mode), the linters and the type checkers. A repository may run more there, including its tests. A repository that runs its tests at push time instead records that as a decision.
- **Let CI make the final call.** CI runs `make check` on every push, as separate steps so that one failure doesn't hide another, and repeats the commit-stage hooks over every file, because anyone can skip a hook with `--no-verify`.
- **Audit on a schedule.** A scheduled CI workflow runs `pnpm audit` and pip-audit at least weekly. They don't run before each commit, because new advisories appear without any change to the code.
- **Lint the CI workflows.** actionlint checks `.github/workflows/` in the commit stage and in CI. Pin it in `uv.lock` through the `actionlint-py` wheel, once that wheel has been checked like any other new dependency.
- **Update monthly, and review every update.** Dependabot covers every ecosystem the repository uses, including GitHub Actions and any development container images. Every update is reviewed and merged only once CI passes. An upgrade that changes what a linter or type checker reports is reviewed like any other change.

## Conditional tools

A repository adopts these once it has the code they apply to:

| Tool | Adopt when |
|---|---|
| Vitest | The repository has TypeScript logic to unit test beyond what browser tests check efficiently |
| Hypothesis | Python code processes text or other open-ended input, such as parsing, counting, rewriting links or resolving references |
| `@axe-core/playwright` | The repository builds a site with Playwright tests. Add accessibility checks to those tests. |
| pytest-xdist | The Python test suite is slow enough that running it in parallel helps |

## Exceptions

A repository may differ from the core tools only through a decision in its own decision record. The decision names the standard tool it replaces, says why, and sets a condition that ends the exception, such as a stable release or a measured threshold. An exception without an end condition is really a proposal to change the standard, and belongs in a pull request to this document instead.

## Tools to watch

These newer tools are faster, but they don't yet meet the maturity criterion. Each could replace or join its category in a later version of the standard:

- **ty** (Astral), for Python type checking
- **Oxlint** and **Biome**, for TypeScript linting and formatting
- **prek**, a faster drop-in replacement for pre-commit
- **Renovate**, if Dependabot can't keep up with current pnpm releases
- **zizmor**, for security checks of CI workflows
