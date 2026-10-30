---
id: DEC-12
title: "The review standard: someone other than the author reviews every software change"
date: 2026-10-30
topics: process
holding: "Every software change is reviewed by someone other than its author before it reaches main: another person, or a separate coding agent that is told the change's purpose and nothing about how it was written. Security fixes and changes to CI permissions, deployment, releases or secrets should also be read by a person."
amended_by:
superseded_by:
amends:
supersedes:
applies:
codified_in: STANDARDS.md, Review standard
---

## 12. The review standard: someone other than the author reviews every software change

*Decided 30 October 2026, in standards version 2026-10-30.1.*

**Decided.** Every software change is reviewed by someone other than its author before it reaches `main`. The reviewer is another person, or a separate coding agent that is told the change's purpose and nothing about how it was written. Security fixes, and changes to CI permissions, deployment, releases or the handling of secrets, should also be read by a person.

heruhane is small, so a second person is not always available, and work should not stop for that. A reviewer that has not seen the author's reasoning does not share its assumptions, and can check what a change claims by running it. The riskiest changes are the ones where a person's judgement is still worth waiting for.

How a change reaches `main` is a separate rule, in [`CONTRIBUTING.md`](../CONTRIBUTING.md) and [DEC-3](DEC-3.md). This decision is about who reviews it.

Two alternatives were considered. **Requiring an approval from a second person on every change** would stop work whenever that person is away. **Letting an author merge their own work after a waiting period** was rejected, because time passing is not a review.

**Obligations.**

- **Follow the [review standard](../STANDARDS.md#review-standard)**, including its list of what a reviewer checks.
