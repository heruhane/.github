---
id: DEC-2
title: "ShellCheck checks shell scripts, installed through the shellcheck-py wheel"
date: 2026-10-28
topics: tooling
holding: "A repository with shell scripts checks them with ShellCheck, installed through the shellcheck-py wheel as a development dependency so that uv.lock pins it. It is a conditional tool, adopted once a repository has shell scripts."
amended_by:
superseded_by:
amends:
supersedes:
applies:
codified_in: TOOLING.md, Conditional tools
---

## 2. ShellCheck checks shell scripts, installed through the shellcheck-py wheel

*Decided 28 October 2026, in standard version 2026-10-28.3.*

**Decided.** A repository that has shell scripts, such as a development container's setup script or a CI helper, checks them with **ShellCheck**. It is installed through the **`shellcheck-py`** wheel, so that it is pinned in `uv.lock` like the other Python development tools.

Repositories often have shell scripts, for development containers and CI, and nothing in the standard checked them. Shell is easy to get subtly wrong: an unquoted variable that splits on spaces, or a failed command that the script carries on past. ShellCheck finds these mistakes.

Measured against [Choosing a tool](../TOOLING.md#choosing-a-tool) on 28 October 2026:

1. **Openly licensed.** ShellCheck is GPL-3.0; the `shellcheck-py` wrapper is MIT. Running ShellCheck on a repository's scripts places no licence obligation on the scripts.
2. **Proven.** Its repository dates from November 2012, and it has released steadily since without breaking changes for its users. It has never released a 1.0 (the latest release is 0.11.0, of 4 August 2025), so it passes on the three-year test, not the stable-release test.
3. **Maintained.** It had commits in the last six months, most from its original author and some from other contributors. It is a small single-purpose tool, and it is open source and can be forked, so one main maintainer is enough under the criterion.
4. **Widely used.** The `shellcheck-py` wheel alone had 4,255,062 downloads in the month to 28 October 2026 (pypistats.org), and ShellCheck is also packaged by the major Linux distributions and Homebrew.
5. **Fits the toolchain.** The wheel ships the ShellCheck binary and tracks ShellCheck's own version (0.11.0.1 for 0.11.0), so uv installs and pins it with no second runtime.
6. **Does several jobs.** It does one job, and no other tool in the standard does that job.

The alternative was to leave shell unchecked, or to install ShellCheck from the operating system's packages. That would give a different version on each machine, against the reproducibility principle.

**Obligations.**

- **Declare `shellcheck-py` as a development dependency** and run ShellCheck through `uv run --locked`, from a `make` target that `make check` and the commit stage both run.
- **Check every shell script in the repository**, including those in `.devcontainer/` and `.github/`. Suppress a finding on its line, with ShellCheck's `# shellcheck disable=` directive and a reason.
