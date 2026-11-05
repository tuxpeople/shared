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
