---
id: DEC-11
title: "The testing standard: tests prove the change and do not depend on the outside world"
date: 2026-10-30
topics: testing
holding: "New behaviour arrives with tests, and a bug fix arrives with a test that fails without the fix. make check does not depend on the internet, the date or the order tests run in. A flaky test is fixed or removed within a week, not retried until it passes."
amended_by:
superseded_by:
amends:
supersedes:
applies:
codified_in: STANDARDS.md, Testing standard
---

## 11. The testing standard: tests prove the change and do not depend on the outside world

*Decided 30 October 2026, in standards version 2026-10-30.1.*

**Decided.** New behaviour arrives with tests, and a bug fix arrives with a test that fails without the fix. Most logic gets fast unit tests, integration tests use real dependencies where they can, and browser tests run against the built site. `make check` does not depend on the internet, the date or the order tests run in. A flaky test is fixed or removed within a week.

A test that fails without the fix is the evidence that it tests the fix. Tests that depend on the outside world give different results for the same commit, which breaks the reproducibility the tooling standard asks for. Retrying a flaky test until it passes hides the failure, and teaches people to ignore red results.

Three alternatives were considered. **One coverage floor for every repository** was rejected, because the tooling standard already has each repository set and raise its own. **A limit on the number of browser tests** was rejected, because the right number depends on the site. **Retries in CI** were rejected for the reason above.

**Obligations.**

- **Follow the [testing standard](../STANDARDS.md#testing-standard)**, and keep any test of a deployed site as a separate target, outside `make check`.
