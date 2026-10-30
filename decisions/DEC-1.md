---
id: DEC-1
title: "Pyrefly replaces Pyright as the Python type checker"
date: 2026-10-28
topics: tooling
holding: "Python code is type checked with Pyrefly, using its strict preset, installed through uv as a development dependency and run as uv run --locked pyrefly check. It replaces Pyright."
amended_by:
superseded_by:
amends:
supersedes:
applies:
codified_in: STANDARDS.md, Core tools
---

## 1. Pyrefly replaces Pyright as the Python type checker

*Decided 28 October 2026, in standard version 2026-10-28.2.*

**Decided.** Python code is type checked with **Pyrefly**, using its `strict` preset, installed through uv. It replaces **Pyright**.

Pyrefly now meets the criteria better than Pyright:

- **Mature and stable.** Pyrefly reached a stable 1.0 on 12 May 2026 after a public beta that began with 0.1.0 on 19 February 2025, and has had regular releases since. Its type checking is in daily use on large codebases, including Instagram, PyTorch and JAX.
- **Fast.** It is written in Rust and checks a project in a fraction of the time Pyright takes, so it stays comfortably fast enough for the commit stage as a repository grows.
- **Backed by a well-funded organisation.** Meta develops it in the open under the MIT licence, and it is widely used across the Python ecosystem.
- **Simpler to install.** It ships as a platform wheel containing a single native binary, so it needs no Node.js runtime alongside it.

Two alternatives were considered. **Pyright**, the previous standard, is mature and widely used, but it runs on Node.js, so a Python-only repository needed a second runtime just to type check. **ty**, Astral's Rust-based checker, remains under watch until it has a stable release.

**Obligations.**

- **Declare Pyrefly as a development dependency** in `pyproject.toml` and run it as `uv run --locked pyrefly check`, so that `uv.lock` pins it and checking Python needs no Node.js or `node_modules`.
- **Set `preset = "strict"` in `[tool.pyrefly]`** and keep the check at zero errors, suppressing a finding on its line with a reason rather than loosening the configuration.
