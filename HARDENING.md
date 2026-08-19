<!-- markdownlint-disable -->

# Hardening Report: port-labs--port-github-action/v1.7.8

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **port-labs--port-github-action/v1.7.8** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in .github/workflows/release.yml use mutable version tags instead of pinned 40-character commit SHAs, making the workflow vulnerable to supply-chain attacks if those tags are moved. Failing references: `actions/checkout@v3` (line 11), `Songmu/tagpr@v1` (line 12), `haya14busa/action-update-semver@v1` (line 16).

Locations:

- `.github/workflows/release.yml:11`
- `.github/workflows/release.yml:12`
- `.github/workflows/release.yml:16`

### unpinned-uses (severity: high)

The `uses:` reference in .github/workflows/test.yml uses a mutable version tag instead of a pinned 40-character commit SHA. Failing reference: `actions/checkout@v3` (line 12).

Locations:

- `.github/workflows/test.yml:12`

### missing-permissions (severity: medium)

.github/workflows/release.yml has no top-level `permissions:` key and no job-level `permissions:` key on the `release` job. Without explicit permissions, the workflow runs with the default (potentially broad) token permissions.

Locations:

- `.github/workflows/release.yml:1`

### missing-permissions (severity: medium)

.github/workflows/test.yml has no top-level `permissions:` key and no job-level `permissions:` key on the `test` job. Without explicit permissions, the workflow runs with the default (potentially broad) token permissions.

Locations:

- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all four findings across two workflow files:

**release.yml:**
- Pinned `actions/checkout@v3` → `@a37ce9120846195fa4ece8f58b268e6043cb2f26 # v3`
- Pinned `Songmu/tagpr@v1` → `@d1b8138b7a31075141b6cd64103de9485ced7ac9 # v1`
- Pinned `haya14busa/action-update-semver@v1` → `@7d2c558640ea49e798d46539536190aff8c18715 # v1`
- Added top-level `permissions: contents: write` (required for tagpr to create/push tags and for action-update-semver to update semver tags)

**test.yml:**
- Pinned `actions/checkout@v3` → `@a37ce9120846195fa4ece8f58b268e6043cb2f26 # v3`
- Added top-level `permissions: contents: read` (minimal permission needed for checkout and running npm tests)

