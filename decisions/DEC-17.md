---
id: DEC-17
title: "The privacy standard: collect as little as possible, and review before handling personal data"
date: 2026-10-30
topics: privacy
holding: "Projects collect as little as possible and do not track. Any feature that processes personal data, including a contact form or a third party that receives it, first gets a privacy review recorded as a decision. A service likely to be accessed by under-18s also completes a Data Protection Impact Assessment and follows the ICO's Children's Code."
amended_by:
superseded_by:
amends:
supersedes:
applies:
codified_in: STANDARDS.md, Privacy standard
---

## 17. The privacy standard: collect as little as possible, and review before handling personal data

*Decided 30 October 2026, in standards version 2026-10-30.1.*

**Decided.** Projects collect as little as possible, and do not track. Any feature that processes personal data, including a contact form or a third party that receives it, first gets a privacy review recorded as a decision. A service likely to be accessed by under-18s also completes a Data Protection Impact Assessment and follows the ICO's Children's Code. Logs, fixtures and AI tools stay free of personal data.

Data that was never collected cannot leak, and needs no one to look after it. heruhane works from the UK, so UK GDPR applies to any personal data it handles. A review before the feature exists is cheaper than undoing one afterwards.

Two alternatives were considered. **Applying the children's rules to every repository** was rejected, because they fit services that under-18s are likely to use, and most projects collect nothing at all. **Naming United States rules such as COPPA and FERPA** was left out until a product is aimed at schools there.

This standard says what a repository does, and does not replace legal advice. A project that handles personal data should check what the law requires of it.

**Obligations.**

- **Follow the [privacy standard](../STANDARDS.md#privacy-standard)**, and record a privacy review before merging a feature that processes personal data.
