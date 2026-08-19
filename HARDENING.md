<!-- markdownlint-disable -->

# Hardening Report: port-labs--port-github-action/v1.7.11

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **port-labs--port-github-action/v1.7.11** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files use mutable tag-based refs instead of full 40-character SHA commit digests, making them vulnerable to supply-chain attacks if the referenced action tags are moved or compromised. Failing references: release.yml uses actions/checkout@v3, Songmu/tagpr@v1, haya14busa/action-update-semver@v1; test.yml uses actions/checkout@v6.

Locations:

- `.github/workflows/release.yml:9`
- `.github/workflows/release.yml:10`
- `.github/workflows/release.yml:14`
- `.github/workflows/test.yml:13`

### missing-permissions (severity: medium)

Neither workflow file defines a top-level permissions: key, and no job in either file defines a job-level permissions: key. Without explicit permissions, the GITHUB_TOKEN is granted its default (broad) permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/release.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all four unpinned action references by resolving their full 40-character SHA digests via lookup_action_sha and pinning them (keeping the original tag as a comment). Added top-level permissions blocks to both workflow files: release.yml gets contents:write and pull-requests:write (required by Songmu/tagpr and haya14busa/action-update-semver), and test.yml gets contents:read (minimum needed for checkout).

