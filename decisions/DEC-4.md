---
id: DEC-4
title: "The standards are separate standards in one document, with one version"
date: 2026-10-30
topics: process
holding: "STANDARDS.md holds a set of separate standards, such as the tooling standard and the security standard. A repository adopts whichever it chooses. The document has one version, which covers every standard in it, written as the merge date followed by a count of that day's changes, starting from 1."
amended_by:
superseded_by:
amends:
supersedes:
applies:
codified_in: STANDARDS.md, Adopting the standards
---

## 4. The standards are separate standards in one document, with one version

*Decided 30 October 2026, in standards version 2026-10-30.1.*

**Decided.** `STANDARDS.md` replaces `TOOLING.md` and holds a set of separate standards, of which the tooling standard is one. A repository adopts whichever of them it chooses, by recording a decision. The document has one version, which covers every standard in it, so a repository follows one version for everything it has adopted. A version is the merge date followed by a count of the changes merged that day, starting from 1, as in `2026-10-30.1`.

`TOOLING.md` said which tools a repository uses, but not what the software owes the people who use it, such as privacy, accessibility and correct Japanese. Those rules need the same adoption, exceptions and version history as the tooling rules, so they share a document. They are separate standards because a repository may be ready for one long before it is ready for another. One version for the whole document means there is one version to read, cite and move to. Counting from 1 gives every version the same shape, where before the first version of a day had no suffix.

Two alternatives were considered. **One file per standard, each with its own version**, would let a repository move at a different pace for each standard, but it would have many versions to track, and rules that depend on each other could disagree. **Adopting the whole document or none of it** is simpler to state, but a repository would have to record exceptions for every rule it is not ready for, or adopt nothing.

Moving the document changed the paths in the links and `codified_in` fields of DEC-1, DEC-2 and DEC-3. Nothing else in them changed, and they still call the document the tooling standard where they did.

**Obligations.**

- **Name the standards and the version** in the decision that adopts them.
- **Give each merged change to the document a new version**, and add it to the version history.
