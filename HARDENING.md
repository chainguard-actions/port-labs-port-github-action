<!-- markdownlint-disable -->

# Hardening Report: port-labs--port-github-action/v1.7.10

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **port-labs--port-github-action/v1.7.10** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow uses action references pinned to mutable tags instead of immutable full-length commit SHAs. In release.yml: `actions/checkout@v3`, `Songmu/tagpr@v1`, `haya14busa/action-update-semver@v1`. In test.yml: `actions/checkout@v3`. These can be silently updated by the action author, enabling supply-chain attacks.

Locations:

- `.github/workflows/release.yml:9`
- `.github/workflows/release.yml:10`
- `.github/workflows/release.yml:14`
- `.github/workflows/test.yml:12`

### missing-permissions (severity: medium)

Neither workflow file defines a top-level `permissions:` key, and no job in either file defines a job-level `permissions:` key. Without explicit permissions, workflows run with the default (potentially broad) token permissions. Both `.github/workflows/release.yml` and `.github/workflows/test.yml` are affected.

Locations:

- `.github/workflows/release.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files: (1) Pinned all unpinned action references to full commit SHAs — actions/checkout@v3 → a37ce9120846195fa4ece8f58b268e6043cb2f26, Songmu/tagpr@v1 → d1b8138b7a31075141b6cd64103de9485ced7ac9, haya14busa/action-update-semver@v1 → 7d2c558640ea49e798d46539536190aff8c18715 — with original tags preserved as inline comments. (2) Added top-level permissions blocks to both files: release.yml gets `contents: write` (required for tagpr to create tags and action-update-semver to update semver refs), test.yml gets `contents: read` (only needs checkout).

