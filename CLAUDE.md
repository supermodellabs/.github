# Supermodel Labs `.github` — Agent Context

This is the org-level `.github` repo for [Supermodel Labs](https://github.com/supermodellabs). GitHub uses it as the **fallback source of truth** for community health files across every repo in the org: any repo without its own `LICENSE`, `CONTRIBUTING.md`, `SECURITY.md`, issue templates, or PR template inherits the version here.

A change in this repo silently propagates to every repo that hasn't overridden the file locally. Treat edits accordingly.

## What Supermodel Labs is

Supermodel Labs ships a small family of source-available and open source CLIs for **agentic analytics engineering**. The brand brief:

- Single portable binary, distributed via Homebrew (`supermodellabs/tap`) and Linux package managers.
- Friendly, ergonomic CLI UX: pretty output, helpful errors that link to docs, agent-friendly verbose modes, and just enough delight (animated splash screens, ASCII art, tasteful color) without tipping into gimmick.
- Tools compose where it makes sense — assume a user may have several Supermodel tools installed and lean on shared conventions.

Per-tool repos use their own `AGENTS.md` (see `tool-template/AGENTS.md` for the canonical baseline). This file covers only what's special about *this* repo.

## What lives here

| File | Role |
| --- | --- |
| `LICENSE.md` | Default license inherited by repos without their own. |
| `CONTRIBUTING.md` | Default contributor guide. Per-repo overrides only when a tool genuinely diverges. |
| `README.md` | Shown on the org's GitHub landing if `profile/README.md` is absent. |
| `profile/README.md` | The org profile page rendered at github.com/supermodellabs. |
| `.markdownlint.yaml` | Lint config for markdown in this repo. |
| `.github/labels.yml` | Org-wide label manifest. Synced to every non-archived repo in the org. |
| `.github/workflows/sync-labels.yml` | Pushes `.github/labels.yml` to all org repos on change, weekly, and on manual dispatch. |

Issue templates, PR templates, and `SECURITY.md` are not yet present — when added, they also become org-wide defaults.

## Org-wide label sync

`.github/labels.yml` is the **exclusive** label set for every repo in the org. The workflow runs with `prune: true`, so any label not in the manifest gets deleted from every target repo on the next sync. Add labels here, not per-repo.

- **Auth:** org secret `ORG_ACTIONS_TOKEN` — a fine-grained token scoped to all org repos with `Issues: read/write`, `Contents: read/write`, `Actions: read/write`, `Deployments: read/write`, `Pull requests: read/write`, `Workflows: read/write`, `Metadata: read`. Shared across cross-org workflows. Swap for a GitHub App token when the org App lands.
- **Targets:** discovered dynamically via `gh api /orgs/supermodellabs/repos`, filtered to non-archived. No opt-out — uniformity is the point.
- **Source overlap:** `~/.local/share/chezmoi/.utils/provision-repo.ts` also defines these labels. Keep them in step until that script drops its label pass; it still owns Discussions categories and private vulnerability reporting.

## Editing rules

- **Default-only.** Anything added here applies to every repo without its own copy. Before adding a file, confirm it should be a universal default — if it's tool-specific, it belongs in that tool's repo.
- **Override, don't fork.** If a per-repo file is drifting from the org default, prefer updating the default here and deleting the per-repo copy. Only keep per-repo overrides when the tool genuinely diverges.
- **Profile vs root README.** `profile/README.md` is the org landing page (public-facing pitch). Root `README.md` describes this repo itself (for collaborators landing on github.com/supermodellabs/.github).
- **Markdown.** Follow the user's global markdown rules — language tags on all code fences, spaces around inner table pipes, no manual line wrapping.
