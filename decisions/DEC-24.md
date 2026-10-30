---
id: DEC-24
title: "The Git hook tool depends on the language: Lefthook for TypeScript, pre-commit for Python"
date: 2026-10-30
topics: tooling
holding: "A repository's Git hook tool follows its language: Lefthook, installed through pnpm, for TypeScript, and pre-commit, installed through uv, for Python. A repository with both uses the tool for the language of its main build, and one with neither uses pre-commit through uvx. The standard file checks come from pre-commit-hooks in both tools."
amended_by:
superseded_by:
amends:
supersedes:
applies:
codified_in: STANDARDS.md, Core tools and Rules
---

## 24. The Git hook tool depends on the language: Lefthook for TypeScript, pre-commit for Python

*Decided 30 October 2026, in standards version 2026-10-30.3.*

**Decided.** The Git hook tool moves from the tools that apply whatever the language to the tools for each language. TypeScript repositories use **Lefthook**, installed through pnpm. Python repositories keep **pre-commit**, installed through uv. A repository with both languages uses the tool for the language its main build is written in, so that it has one set of hooks. A repository with neither keeps pre-commit, through `uvx` with a pinned version.

pre-commit is a Python program, so a TypeScript repository could only run it by adding uv and a separate version pin, against the "fits the toolchain" criterion, which asks for a tool that installs through a lockfile the repository already has and needs no second runtime. Lefthook does that for TypeScript, as pre-commit does for Python.

Measured against [Choosing a tool](../STANDARDS.md#choosing-a-tool) on 30 October 2026:

1. **Openly licensed.** MIT.
2. **Proven.** Its first release was in November 2018, and 1.0 in June 2022. Version 2.0, in October 2025, removed deprecated options and regular expressions in `exclude`, which are small changes to a configuration file rather than rework.
3. **Maintained.** Its latest release was on 13 October 2026. Most of its commits come from one main maintainer (692, with the next contributor at 41), which the criterion allows for a small single-purpose tool with an alternative. Its jobs only call commands, so moving to pre-commit would be a small change.
4. **Widely used.** 24,306,842 npm downloads in the month to 29 October 2026 (npmjs.org).
5. **Fits the toolchain.** pnpm installs and pins it through `pnpm-lock.yaml`. Its npm package ships a native binary for each platform, so it needs no second runtime. Its install script is not needed, so it stays off the list of allowed scripts, and a `make` target installs the hooks.
6. **Does several jobs.** It does one job, and takes the place of another tool in that job.

**The standard file checks stay the same in both tools.** Lefthook jobs run the commands of the `pre-commit-hooks` package, which pre-commit fetches from the same project. The package is MIT licensed, with 2,028,976 PyPI downloads in the month to 30 October 2026 (pypistats.org). This means every repository looks for private keys before a commit, and checks files the same way.

This has three costs. A TypeScript repository with no Python now needs uv to run those checks, although not a Python project of its own, so the fit with the toolchain is good rather than perfect. `uvx` pins `pre-commit-hooks` but not what it depends on, and nothing moves that pin automatically, so a repository moves it by hand. And Lefthook does not skip binary files by itself as pre-commit does, so the jobs that fix files need `file_types: text`.

This was tried on 30 October 2026 with Lefthook 2.2.2 and `pre-commit-hooks` 6.0.0, in throwaway repositories. Through `uvx`, each check stopped a commit with bad input, including a private key and a real merge conflict, left binary files and excluded Markdown unchanged, and passed when run over every file with `lefthook run pre-commit --all-files`. Through `uv run --locked`, the TOML, large file, private key and whitespace checks did the same. An invalid `lefthook.yml` stops the hook from installing, so a mistake there can leave a checkout with no hook at all. CI finds this, because it runs the hooks over every file and fails when the configuration does not parse.

Three alternatives were considered. **Lefthook everywhere** would change Python repositories, where pre-commit already works, for no gain. **Node.js equivalents of the file checks** would avoid uv, but would add several small tools and let repositories check files differently. **Running the file checks only in CI** would be simpler, but would find a private key only after it had been committed.

**Obligations.**

- **Use the Git hook tool for the repository's language**, and move to it when moving to this version.
- **Run the standard file checks from `pre-commit-hooks`**, as the [Rules](../STANDARDS.md#rules) describe, with one pin for its version.
- **Repeat the hooks over every file in CI**, with `lefthook run pre-commit --all-files` or `pre-commit run --all-files`.
