# 0001. Pyrefly replaces Pyright as the Python type checker

- **Date**: 2026-10-28
- **Standard**: [TOOLING.md](../TOOLING.md), version 2026-10-28.2

## Decision

Python code is type checked with Pyrefly, using its `strict` preset, installed through uv. It replaces Pyright.

## Reasons

Pyrefly now meets the criteria better than Pyright:

- **Mature and stable.** Pyrefly reached a stable 1.0 on 12 May 2026 after a public beta that began with 0.1.0 on 19 February 2025, and has had regular releases since. Its type checking is in daily use on large codebases, including Instagram, PyTorch and JAX.
- **Fast.** It is written in Rust and checks a project in a fraction of the time Pyright takes, so it stays comfortably fast enough for the commit stage as a repository grows.
- **Backed by a well-funded organisation.** Meta develops it in the open under the MIT licence, and it is widely used across the Python ecosystem.
- **Simpler to install.** It ships as a platform wheel containing a single native binary, so it needs no Node.js runtime alongside it.

## Alternatives

- **Pyright**, the previous standard. It is mature and widely used, but it runs on Node.js, so a Python-only repository needed a second runtime just to type check.
- **ty**, Astral's Rust-based checker. It remains under watch until it has a stable release (see [Tools to watch](../TOOLING.md#tools-to-watch)).
