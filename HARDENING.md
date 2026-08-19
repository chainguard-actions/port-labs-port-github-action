<!-- markdownlint-disable -->

# Hardening Report: port-labs--port-github-action/v1.7.9

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **port-labs--port-github-action/v1.7.9** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and no job-level `permissions:` key on any job. Without explicit permissions, the GITHUB_TOKEN is granted default (potentially broad) permissions. A minimal permissions block should be added.

Locations:

- `.github/workflows/release.yml:1`
- `.github/workflows/test.yml:1`

### unpinned-uses (severity: high)

Multiple `uses:` references are pinned to mutable tags rather than immutable 40-character commit SHAs, making the workflow vulnerable to supply-chain attacks if the tag is moved.

In `.github/workflows/release.yml`:
- `uses: actions/checkout@v3` (tag: v3)
- `uses: Songmu/tagpr@v1` (tag: v1)
- `uses: haya14busa/action-update-semver@v1` (tag: v1)

In `.github/workflows/test.yml`:
- `uses: actions/checkout@v3` (tag: v3)

All should be replaced with their full 40-character commit SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/release.yml:9`
- `.github/workflows/release.yml:10`
- `.github/workflows/release.yml:14`
- `.github/workflows/test.yml:11`

## Iteration Notes

### Iteration 1

**Fixes applied:** missing-permissions, unpinned-uses

**Notes:**

Fixed both workflow files:

1. **missing-permissions** (release.yml, test.yml): Added top-level `permissions: {}` to restrict default GITHUB_TOKEN permissions, plus job-level minimal permissions: `contents: write` for the release job (needed to create tags/releases) and `contents: read` for the test job (only needs to checkout code).

2. **unpinned-uses** (release.yml, test.yml): Pinned all four mutable tag references to full 40-character commit SHAs:
   - `actions/checkout@v3` → `@f43a0e5ff2bd294095638e18286ca9a3d1956744 # v3` (both files)
   - `Songmu/tagpr@v1` → `@d1b8138b7a31075141b6cd64103de9485ced7ac9 # v1`
   - `haya14busa/action-update-semver@v1` → `@7d2c558640ea49e798d46539536190aff8c18715 # v1`

