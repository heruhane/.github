---
id: DEC-25
title: "Development containers install tools the way the project does, from an exactly pinned image with no language runtime"
date: 2026-10-30
topics: tooling
holding: "A development container is built from a Dockerfile on an exactly pinned base image with no language runtime. uv and pnpm install the runtimes from the repository's own pins, the image holds only what make setup cannot install, and CI builds the container whenever it changes."
amended_by:
superseded_by:
amends:
supersedes:
applies:
codified_in: STANDARDS.md, Development containers
---

## 25. Development containers install tools the way the project does, from an exactly pinned image with no language runtime

*Decided 30 October 2026, in standards version 2026-10-30.4.*

**Decided.** The tooling standard gains rules for how a development container is set up. It is built from a Dockerfile, on an exactly pinned image with no language runtime. uv and pnpm install the runtimes from the repository's own pins, and the image holds only what `make setup` can't install. The rules don't say what a container contains, because repositories differ: a site, a data project and a book need different system packages. For the same reason there is no shared Dockerfile, template or base image, and a shared copy would also need its own updates and would hold version numbers that go stale.

Until now the standard said only that a container is set up with `make setup`. That left the choices most likely to break the [reproducibility principle](../STANDARDS.md#running-tools) to each repository, and repositories that make them differently stop working alike.

**The runtimes keep one pin.** `.python-version` and `.node-version` are already the pins. A language image names Python again in its tag, and a Node.js feature names Node.js again in `devcontainer.json`. Each is one more copy to check, and neither replaces the pin that the rest of the project reads.

Three alternatives were checked on 30 October 2026:

- **A language image**, such as `devcontainers/python`. It is a supported image, and it would save uv one download. It also puts a system Python on the machine alongside the project's own, so `PATH` then decides which one runs.
- **The Node.js feature**, `ghcr.io/devcontainers/features/node`. It installs Node.js through nvm, fetches nvm's installer at the latest nvm version by default, and also installs the latest pnpm by default. Those defaults can be overridden, but they add another installer and more versions to pin. Dependabot's `docker` updates also can't see a version written in a feature's options.
- **Naming the image in `devcontainer.json`.** Dependabot's `docker` updates look for Dockerfiles, Containerfiles and YAML files, so a version named there is not updated. A short Dockerfile avoids this.

**System packages are the one thing not pinned exactly.** `apt-get` installs the current package from a Debian release, so two builds of one Dockerfile can differ slightly. Pinning each package isn't practical, because Debian's repositories drop old versions. The standard therefore treats the release as their pin, and accepts this small difference.

**CI builds the container only when it changes.** Without this, a broken container is found only when someone next rebuilds it, which can be long after the update that broke it. Building it on every push would take time from CI's ten-minute target for a part of the repository that rarely changes. The [`devcontainers/ci`](https://github.com/devcontainers/ci) action is one way to do it, and is pinned to a commit SHA like any other action.

This has three costs. A new container downloads its runtimes during setup instead of having them in the image, so it takes longer to create and needs network access. Pinning the base image exactly means more Dependabot updates, and in a repository that compares screenshots, one of them can mean recording the baselines again. And a screenshot comparison that runs only in the container doesn't run in a CI job outside it, unless that job also runs in the container.

These rules have not yet been tried together in a new repository. In particular, nothing has yet tested the CI build, or Dependabot's handling of the exact base image tag.

**Obligations.**

- **Follow the [Development containers](../STANDARDS.md#development-containers) rules** in any repository that has a development container, and record an exception for any required rule it departs from.
- **Move the base image and the Debian release deliberately**, through a reviewed update, and rebuild the container before merging the change.
