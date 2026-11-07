# shared

Shared configuration for the `tuxpeople` repositories.

> **Changes here are live everywhere.** Repositories extend these presets without a ref, i.e. from `master`. A change applies to all of them on their next Renovate run, so change presets via pull request and add new behaviour as an opt-in preset first.

## Renovate presets

| Preset | Content | Used by |
|---|---|---|
| `renovate/renovate.json5` | Base config: `config:best-practices`, automerge of minor/patch/digest updates once all checks pass, pinned GitHub Actions, dependency dashboard, includes `commitMessage`, `autoMerge`, `customManagers` | most repositories |
| `renovate/semanticCommits.json5` | Conventional Commit types/scopes per datasource (e.g. Docker minor → `feat(container)`) | opt-in |
| `renovate/labels.json5` | Labels per update type and datasource | opt-in |
| `renovate/minimumReleaseAge.json5` | Wait 3 days before proposing new PyPI and GitHub releases, only for new versions (major/minor/patch, not pin/digest updates; Docker excluded, npm is in `config:best-practices`) | opt-in |
| `renovate/autoMerge.json5`, `commitMessage.json5`, `customManagers.json5` | Parts of the base config | via `renovate.json5` |

Usage:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": [
    "github>tuxpeople/shared//renovate/renovate.json5",
    "github>tuxpeople/shared//renovate/minimumReleaseAge.json5"
  ]
}
```

## Workflows

`workflows/` contains workflow templates to copy into a repository.

`.github/workflows/` contains reusable workflows, called with a pinned reference (Renovate keeps it current):

| Workflow | Purpose |
|---|---|
| `ghcr-cleanup.yml` | Monthly GHCR cleanup: tagged releases after 36 months, untagged and `sha-*` after 12 months, multi-arch safe. Usage in the file header |
| `release-please.yml` | Maintains the release PR, creates tag and GitHub Release. Usage in the file header |
| `container-release.yml` | Multi-arch image to GHCR, Cosign, Trivy, SBOM, attestations; public repositories only. Tests stay in the caller. Usage in the file header |

Changes here reach a repository only when its pin is updated, so a mistake does not hit all repositories at once.

### Updating the reusable workflows

1. Renovate keeps the actions pinned in these workflows current and merges them here on its own. Nothing reaches a caller yet.
2. Set a new tag **by hand**, about once a month or after a change that matters. Each tag is a deliberate step: without it, the callers keep their pin and the actions in it age unnoticed.
3. Renovate then opens a PR per calling repository. These are **not automerged** (`renovate/autoMerge.json5`): the release and cleanup workflows do not run in PR checks, so a mistake would only show up at the next release, in all repositories.
4. Merge one repository first and wait for a real run, then the others.
