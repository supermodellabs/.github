# Contributing to Supermodel Labs

Thanks for considering a contribution. This document is the org-wide baseline — individual repos may add their own `CONTRIBUTING.md` with project-specific setup, vocabulary, or design principles that override or extend what's here.

## Getting started

Each repo's `README.md` covers its language, runtime, and local setup. Read that first. Then create a feature branch off `main`:

```bash
git switch -c fix/some-thing
```

## Commit style

We use [Conventional Commits](https://www.conventionalcommits.org/), enforced in CI. If you're new to the format, the linked spec is the canonical reference — the rest of this section is our project-specific take.

### Format

```text
type(scope): brief description in imperative mood

Optional body with rationale and context. Feel free to be detailed here.

[Co-author tags, issue references, etc.]
```

Scopes are optional. When present, they map to a meaningful construct within the project (a command, module, or subsystem).

### Types

- `feat` — new functionality
- `fix` — bug fixes
- `test` — adding or updating tests ONLY
- `refactor` — code restructuring or renaming without behavior change
- `perf` — can involve a variety of work, but if the explicit focus is improving performance, use this
- `style` — code style changes ONLY (formatting, indentation, etc.)
- `build` — changes to build system or deployment configs
- `ci` — updates to CI configuration, automated checks, pre-commit hooks, etc.
- `chore` — tooling, config changes, workflow, maintenance tasks
- `docs` — documentation ONLY

### Style

- **Imperative mood** — "add feature", not "added feature".
- **No period** at end of subject line.
- **Max 72 chars** for the subject.
- **Body optional** but encouraged for any of the following:
  - explaining reasoning for making a change (e.g., "we want to be able to do X, and this change is necessary to enable that")
  - explaining why a specific solution was chosen when there are multiple valid approaches (e.g., "we tried X and Y, both worked, but X is far more readable and we decided to prioritize long-term maintainability over short-term performance")
  - justifying a non-standard pattern or hack (e.g., "the blessed way of doing X is A, but we can't do this for Y reason, so we are doing B instead")
  - surprising findings from implementation (e.g., "we thought X would work better, but Y, while being much simpler, is also more effective")
  - specific and unusual learnings from research and experimentation (e.g., "we could not figure out why X was happening, but eventually tracked it down to GH user/repo/issues/72, which revealed the root cause was Y, so the solution was Z")
  - documenting a goal (e.g., "we expect feature X to increase user signups by 10%")

Examples:

```text
feat(cli): add --check flag to sync command
fix(sync): preserve symlinks across prefix renames
perf(parser): cache compiled regexes across calls
docs: clarify configuration precedence in README
```

## History

We require **linear history** on `main`. Rebase onto `main` rather than merging; only squash and rebase merges are available in PRs and merge commits in the history will get automatically rejected. If your branch falls behind:

```bash
git fetch origin
git rebase origin/main
git push --force-with-lease
```

Avoid `git push --force` (without `--with-lease`) — `--force-with-lease` prevents you from clobbering changes you didn't pull on your branch.

## PR process

1. **Push your branch and open a PR.** The PR title doubles as the eventual squash-commit message, so it must follow Conventional Commits style (see [PR titles](#pr-titles) below).
2. **CI runs automatically** for repository contributors. For new contributors, GHA workflows require manual approval before they run — this is a spam-control measure, not a judgment of you. Expect a short wait while a maintainer triages and either approves or declines the run. We try to do this within a few days.
3. **Address review feedback** by pushing additional commits to the branch. Don't squash mid-review; we want to see the iteration history during review and squash at merge time.
4. **Once approved**, a maintainer merges. We use `rebase` for small, focused PRs (each commit is meaningful and worth keeping) and `squash` for larger PRs where the intermediate commits are process-noise. For squash merges we use the PR title as the squash commit subject and edit the auto-generated commit list down to a tight summary in the body.

## PR titles

The title is the merged commit subject (for squash merges) or the top-of-branch subject (for rebase merges), so it must be:

- **Conventional Commits style** — `type(scope): subject`.
- **Under ~70 characters** — long titles get truncated in lists.
- **Imperative mood** — "add X", not "added X" or "adds X".

Use the PR description for the long-form: motivation, design choices, testing notes, screenshots. Don't pack it into the title.

## Tracking work: the POT system

Most Supermodel Labs repos plan and track work using **POT — Phases > Objectives > Tasks**. If a repo has both `TODO.md` and `WORKLOG.md` in its root (or a monorepo subdir), it's using POT. The repo's `AGENTS.md` has the project-specific take; this is the org-level summary.

| Level | Markdown | Role | Execution |
| --- | --- | --- | --- |
| **Phase** | `## Phase N: description` | Sequential project checkpoint | Serial — finish one before the next |
| **Objective** | `### description` | Declarative, PR-shaped goal | Parallel within a Phase |
| **Task** | `- [ ] description` | Imperative step inside an Objective | Sequential within an Objective |

Phase status markers: 🌀 active, ✅ completed, no marker = unstarted. **Only one Phase is active at a time.**

### Files

- `TODO.md` — active and upcoming Phases at the top, Backlog at the bottom. Backlog items are not active work; promote into a Phase before starting.
- `WORKLOG.md` — completed Phases in **reverse-chronological** order (newest at top), each with implementation notes capturing decisions, trade-offs, and ADR-style context.

### Phase lifecycle

When a Phase finishes:

1. Update its Objectives and Tasks to reflect what actually shipped — no stale descriptions.
2. Mark the Phase ✅.
3. Move it to the **top** of `WORKLOG.md` with implementation notes.
4. Delete it from `TODO.md`.

### Working rules

- **`#user` tags** mark items only the human can do (deploys, account changes, manual installs). Agents must stop and alert the user if blocked by one.
- **Don't commit pure `TODO.md` / `WORKLOG.md` updates** — fold them into the substantive commit. Squash any stray `docs(todo): ...` commits before merge.
- **Pause on ambiguity, risk, or low-value tasks.** Flag, agree on the change, persist to `TODO.md`, then do the work.
- **Subagent drift:** if an Objective comes back wrong, roll back and reassign rather than mutating the plan to match bad output.

See `tool-template`'s `TODO.md` and `WORKLOG.md` for the canonical worked examples.

## Issues and Discussions

Pick the right channel for your contribution:

- **Discussions** for ideas, questions, and exploratory "what if…?" threads. If you're not sure whether your thought is a concrete issue, start in Discussions. Each repo has four categories:
  - **🤗 Announcements** — updates from Supermodel Labs (read-only for most contributors).
  - **🧐 Ideas** — proposals for new features or improvements, plus any other general needs that don't fit Help or Share.
  - **🤠 Help** — questions and community support (Q&A format).
  - **😍 Share** — show off cool stuff you've built with or for the project.
- **Issues** for concrete, reproducible change requests: a bug with a repro, a missing flag with a clear shape, a doc inaccuracy. An issue should describe the change well enough that a contributor could pick it up cold.

We auto-close issues and PRs that don't engage with these guidelines. That's care for the OSS ecosystem, not gatekeeping — please reopen after addressing the feedback. The bot will point you at the section that needs work.

### Issue labels

Triage applies labels in three families. You don't need to set these yourself — maintainers will — but knowing the vocabulary helps you read the queue:

- **type:** `bug`, `feature`, `docs`, `chore`, `refactor` — what kind of change.
- **status:** `triage`, `in-progress`, `blocked`, `needs-info`, `wontfix` — where the issue is in its lifecycle.
- **meta:** `good first issue`, `help wanted`, `duplicate` — community signals.

If you're looking for a place to start, filter by `good first issue` or `help wanted`.

## Licensing

Most Supermodel Labs projects are dual-licensed under FSL-1.1-ALv2 — **source-available today, Apache 2.0 after two years.** Each repo has a `LICENSE` file with the specifics. By submitting a contribution, you agree to license it under the same terms as the repo.

If you're unsure whether your idea fits a particular project, start a Discussion in that repo before writing code. We'd rather talk shape with you than decline a finished PR for fundamental reasons.
