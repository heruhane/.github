---
id: DEC-10
title: "The tooling standard gains stricter shared defaults"
date: 2026-10-30
topics: tooling
holding: "The tooling standard adds a principle that rules are enforced wherever possible, a minimum Ruff rule set, type-aware linting, one line width, Python tool settings in pyproject.toml, private-key detection, a six-month window for new runtimes, ES modules, a development container by default, telemetry turned off, a ten-minute target for CI, and no dependency for what the platform or a few lines of code already does."
amended_by:
superseded_by:
amends:
supersedes:
applies:
codified_in: STANDARDS.md, Rules
---

## 10. The tooling standard gains stricter shared defaults

*Decided 30 October 2026, in standards version 2026-10-30.1.*

**Decided.** The tooling standard adds these defaults, each with its reason:

- **Rules are enforced wherever possible.** A rule that nothing checks slowly stops being followed.
- **A minimum Ruff rule set**, `E`, `W`, `F`, `I`, `UP`, `B`, `SIM`, `RUF` and `S`. Ruff's default set is small, and `S` finds security mistakes.
- **Type-aware linting**, with typescript-eslint's `strictTypeChecked` and `stylisticTypeChecked`. They find mistakes that untyped rules cannot, such as a promise nobody awaits.
- **One line width and one place for Python settings**, so that someone who knows one repository finds the same setup in the next.
- **Private-key detection** in the standard file checks. It is free and works in private repositories, where GitHub's secret scanning is paid.
- **Six months to move to a new runtime**, once the repository's dependencies support it. That is long enough for dependencies to catch up, and short enough that upgrades stay small.
- **ES modules for TypeScript**, so that there is one module format to think about.
- **A development container by default**, with the project also working without one, so that a new contributor can run the tests within ten minutes.
- **Telemetry turned off** in tools that report usage, in keeping with the [privacy standard](../STANDARDS.md#privacy-standard).
- **A ten-minute target for CI**, so that its results still arrive while the change is in mind.
- **No dependency for what the platform or a few lines of code already does**, because every dependency is one more thing to pin, update and trust.

The alternative was to leave these to each repository. That would give up the point of the tooling standard, which is that the repositories work alike.

**Obligations.**

- **Follow the [Rules](../STANDARDS.md#rules)** of the tooling standard, and record an exception for any default a repository cannot meet.
