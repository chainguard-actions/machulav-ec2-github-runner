<!-- markdownlint-disable -->

# Hardening Report: machulav--ec2-github-runner/v2.5.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **machulav--ec2-github-runner/v2.5.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow files reference GitHub Actions using mutable version tags instead of pinned full-length SHA digests. This exposes the workflow to supply-chain attacks if a tag is moved or a repository is compromised. Affected references: `actions/checkout@v3`, `actions/setup-node@v4`, `actions/cache@v3`. All should be pinned to their full 40-character commit SHA (e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3`).

Locations:

- `.github/workflows/package.yml:12`
- `.github/workflows/package.yml:15`
- `.github/workflows/pr.yml:10`
- `.github/workflows/pr.yml:12`

### missing-permissions (severity: medium)

Neither workflow file defines a top-level `permissions:` block, and no individual job within either file defines its own `permissions:` block. Without explicit permissions, workflows run with the repository's default token permissions, which may be broader than necessary (e.g. `write-all` if the repo default is set that way). Both `package.yml` and `pr.yml` should declare minimal required permissions (e.g. `contents: read` for PR linting, `contents: write` only where the commit-and-push step in `package.yml` requires it).

Locations:

- `.github/workflows/package.yml:1`
- `.github/workflows/pr.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files: (1) Pinned all unpinned action references to full SHA digests — actions/checkout@v3 → f43a0e5ff2bd294095638e18286ca9a3d1956744, actions/setup-node@v4 → 49933ea5288caeca8642d1e84afbd3f7d6820020, actions/cache@v3 → 6f8efc29b200d32929f49075959781ed54ec270c — with original tags preserved as inline comments. (2) Added top-level permissions blocks to both files: package.yml gets `contents: write` (required for the git push step that commits dist/ files), and pr.yml gets `contents: read` (only needs to checkout and lint code).

