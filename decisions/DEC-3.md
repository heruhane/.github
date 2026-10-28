---
id: DEC-3
title: "Every change reaches main through a pull request"
date: 2026-10-28
topics: process
holding: "In every heruhane repository, every change reaches main through a pull request from a branch. Nobody commits directly to main, locally or on GitHub, including coding agents and small changes."
amended_by:
superseded_by:
amends:
supersedes:
applies:
codified_in: CONTRIBUTING.md
---

## 3. Every change reaches main through a pull request

*Decided 28 October 2026.*

**Decided.** In every heruhane repository, every change reaches `main` through a pull request from a branch. Nobody commits directly to `main`, locally or on GitHub. The rule has no exception for small changes, and it applies to coding agents as it does to people.

A commit made straight to `main` is published before anyone else has seen it, and before CI has run on it, so a mistake is found only after it has landed. A pull request puts the whole change in one place to be read before it is merged, runs CI on it first, and keeps the reason for the change with the change. The [tooling standard](../TOOLING.md) already relies on pull requests: it is changed only by pull request, it gives each merged change a new version, and it has CI run on every pull request. This decision makes the same rule apply to all work in every repository.

The alternative was to allow direct commits for small changes such as typo fixes. It was rejected because "small" is a judgement each person makes differently, and a single rule is easier for people and coding agents to follow than a rule with a size limit.

**Obligations.**

- **Work on a branch.** Every change is committed to a branch, never to `main`, and local `main` is kept equal to `origin/main`.
- **Merge through a pull request**, only once its checks pass, as [`CONTRIBUTING.md`](../CONTRIBUTING.md) describes.
- **Keep the rule in a repository's own guide.** A repository that adds its own `CONTRIBUTING.md`, which replaces this organisation's guide on GitHub, states this rule in it.
