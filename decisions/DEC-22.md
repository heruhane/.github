---
id: DEC-22
title: "The service standard: prefer no server, and record why when one is needed"
date: 2026-10-30
topics: services
holding: "A library or static site is preferred. A project that needs a server or stores users' data first records why, with a privacy review and a short threat model, and then meets rules on running safely, keeping the service running, stating its support and handling incidents."
amended_by:
superseded_by:
amends:
supersedes:
applies:
codified_in: STANDARDS.md, Service standard
---

## 22. The service standard: prefer no server, and record why when one is needed

*Decided 30 October 2026, in standards version 2026-10-30.1.*

**Decided.** A library or static site is preferred, because neither needs anyone to keep it running. A project that needs a server or stores users' data first records why, with a privacy review and a short threat model. It then meets rules on running safely, keeping the service running, stating the support on offer and handling incidents.

A server is a lasting commitment for a small organisation: it has to be patched, watched and backed up, and it can lose people's data. Recording the reason first makes that commitment a choice. The rules scale with what a service stores, so backups cover only what it holds.

Two alternatives were considered. **Applying these rules to every site** was rejected, because health checks and backups do little for static files. **Promising round-the-clock support** was rejected as something a small organisation cannot keep, so a README states what support is on offer.

**Obligations.**

- **Follow the [service standard](../STANDARDS.md#service-standard)** from the decision that adds the server onwards.
