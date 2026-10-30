---
id: DEC-23
title: "The public repository standard: check the history, and provide the community files"
date: 2026-10-30
topics: process
holding: "Before a repository becomes public, its whole history is searched for secrets, personal data and private material, and it gains licences, a support statement, a security policy, a code of conduct and a contributing guide, its own or the organisation's defaults in heruhane/.github. The code of conduct is the Contributor Covenant 3.0."
amended_by:
superseded_by:
amends:
supersedes:
applies:
codified_in: STANDARDS.md, Public repository standard
---

## 23. The public repository standard: check the history, and provide the community files

*Decided 30 October 2026, in standards version 2026-10-30.1.*

**Decided.** Before a repository becomes public, its whole history is searched for secrets, personal data and private material. It gains licences, a support statement, a security policy, a code of conduct and a contributing guide, its own or the organisation's defaults in `heruhane/.github`. GitHub's security features are turned on the moment it is public, and its workflows are checked for `pull_request_target`. The code of conduct is the Contributor Covenant 3.0, its current version.

Going public publishes all of a repository's history, not only its latest commit, and that cannot be taken back. Keeping the defaults in one place means that a repository needs its own files only where it differs. GitHub's [code scanning, secret scanning and dependency review are free on public repositories](https://docs.github.com/en/get-started/learning-about-github/about-github-advanced-security) and paid on private ones, which is why they are turned on at this point rather than earlier.

Two alternatives were considered. **Requiring these files and settings from a repository's first commit** was rejected, because they are for people outside the project, and some cost money while a repository is private. **Requiring a Developer Certificate of Origin sign-off** was left out until a repository takes outside contributions.

**Obligations.**

- **Work through the [public repository standard](../STANDARDS.md#public-repository-standard)** before changing a repository's visibility.
