# Decisions behind the heruhane standards

Each file here records why a standard chose a tool or rule, so that the standards themselves can say only what a repository must do. A record is added by the pull request that makes the choice, and is never rewritten afterwards: when a choice changes, a new record replaces the old one, and the old one gains a line saying which record replaced it.

Files are numbered in order, as `NNNN-short-title.md`. Each record gives:

- **Date**: the date its pull request was merged.
- **Standard**: the document and version it applies to.
- **Decision**: what was chosen, in one or two sentences.
- **Reasons**: why, measured against the criteria in [Choosing a tool](../TOOLING.md#choosing-a-tool) where the choice is a tool.
- **Alternatives**: what else was considered, and why it lost.
- **Replaces** or **Replaced by**: an earlier or later record, if there is one.

## Records

- [0001](0001-pyrefly-replaces-pyright.md): Pyrefly replaces Pyright as the Python type checker.
