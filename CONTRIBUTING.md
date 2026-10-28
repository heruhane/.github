# Contributing to heruhane repositories

This guide applies to this repository, `heruhane/.github`, and to every heruhane repository that has adopted it. GitHub shows it on any heruhane repository that has no `CONTRIBUTING.md` of its own. A repository that adds its own guide keeps the rules below and adds its own after them.

## Adopting this guide

A repository adopts this guide by recording a decision that says so, in the same way it adopts the [tooling standard](TOOLING.md#adopting-the-standard). Once the decision is recorded, an organisation owner adds branch protection to the repository's `main` on GitHub, so that a change can reach it only through a pull request. `heruhane/.github` already has this protection.

## Every change reaches `main` through a pull request

In a repository that has adopted this guide, nobody commits directly to `main`. This applies to everyone with write access, to coding agents, and to small changes such as a typo or a version bump. It also applies locally: a commit made on a local `main` has to be moved to a branch before it can be pushed, so make the branch first.

Working through pull requests means that every change to `main` has been seen as a whole before it lands, that CI has run on it before it is merged rather than after, and that the pull request records why the change was made, where anyone can find it later. The reason for the rule is recorded in [DEC-3](decisions/DEC-3.md).

## Making a change

1. Bring your local `main` up to date with `git switch main` and `git pull --ff-only`. Local `main` should always equal `origin/main`; if `--ff-only` fails, something was committed to it, and that commit belongs on a branch.
2. Create a branch for the change with `git switch -c <branch-name>`. Name it for what it does, in lowercase words joined by hyphens, such as `extend-tooling-standard`.
3. Commit to the branch, and run `make check` before pushing in a repository that has one.
4. Push the branch with `git push -u origin <branch-name>` and open a pull request against `main`.
5. Merge the pull request once its checks pass, then delete the branch.

## Writing a pull request

- **One change per pull request.** Two unrelated changes go in two pull requests, so that each can be reviewed, merged or reverted on its own.
- **Say what changes and why.** The description is the record of the change. Name the issue or decision it carries out, if there is one.
- **Do not merge while a check fails.** Fix the failure on the branch and push again. If a check is wrong, fix the check in its own pull request first.
- **Keep the branch current.** If `main` moves on while the pull request is open, bring the change up to date before merging, so that CI has run on what will actually land.

## Code standards

Repositories with code follow the [heruhane tooling standard](TOOLING.md), which names the tools each repository uses and how it runs them.
