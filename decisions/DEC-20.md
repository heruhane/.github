---
id: DEC-20
title: "The release standard: Semantic Versioning, and a release as one unit"
date: 2026-10-30
topics: releases
holding: "A repository whose output other people use versions it with Semantic Versioning 2.0 and records what its public interface is. A release is a version bump, a CHANGELOG.md entry in the Keep a Changelog format, a vX.Y.Z tag and a GitHub release with that entry as its notes."
amended_by:
superseded_by:
amends:
supersedes:
applies:
codified_in: STANDARDS.md, Release standard
---

## 20. The release standard: Semantic Versioning, and a release as one unit

*Decided 30 October 2026, in standards version 2026-10-30.1.*

**Decided.** A repository whose output other people use versions it with Semantic Versioning 2.0, and records in a decision what its public interface is. A release is a version bump, a `CHANGELOG.md` entry in the Keep a Changelog format, a `vX.Y.Z` tag and a GitHub release with that entry as its notes.

Semantic Versioning only means something once a project says what counts as breaking. For a site that may be its URLs and anchors, and for a tool its commands. Treating a release as one unit means that the version, the notes and the tag cannot disagree.

Two alternatives were considered. **Generating releases with release-please** would add a tool and require Conventional Commit titles, for little gain over a release made by hand. **A deprecation policy for public APIs** was left out until a repository publishes a library with users to protect.

**Obligations.**

- **Follow the [release standard](../STANDARDS.md#release-standard)**, and record the public interface before the first release.
