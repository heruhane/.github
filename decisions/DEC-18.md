---
id: DEC-18
title: "The security standard: protected accounts, recoverable access and no secrets in repositories"
date: 2026-10-30
topics: security
holding: "Accounts with admin or publishing rights use phishing-resistant two-factor authentication, at least two people can recover every repository, domain, hosting account and package, secrets stay out of repositories and are revoked first if they leak, and data from outside a program is validated at the boundary."
amended_by:
superseded_by:
amends:
supersedes:
applies:
codified_in: STANDARDS.md, Security standard
---

## 18. The security standard: protected accounts, recoverable access and no secrets in repositories

*Decided 30 October 2026, in standards version 2026-10-30.1.*

**Decided.** Accounts with admin or publishing rights use phishing-resistant two-factor authentication. At least two people can recover every repository, domain, hosting account and package. Secrets stay out of repositories, and a leaked secret is revoked first and cleaned up afterwards. Data from outside a program is validated at the boundary.

A small organisation has few accounts, and losing or misusing any one of them could do serious harm. Access held by one person stops with that person. Revoking comes first because removing a secret from a repository does not make it secret again.

The alternative was to publish response times for security reports. They are left out for now, because a published promise has to be one the organisation can keep. [`SECURITY.md`](../SECURITY.md) says so plainly, and this can be revisited.

**Obligations.**

- **Follow the [security standard](../STANDARDS.md#security-standard)** for every account a repository depends on.
