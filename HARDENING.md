<!-- markdownlint-disable -->

# Hardening Report: machulav--ec2-github-runner/v2.6.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **machulav--ec2-github-runner/v2.6.1** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in package.yml are pinned to mutable version tags rather than full 40-character commit SHAs, making the workflow vulnerable to supply-chain attacks if the referenced action tags are moved. Failing references: `actions/checkout@v3` and `actions/setup-node@v4`.

Locations:

- `.github/workflows/package.yml:12`
- `.github/workflows/package.yml:16`

### unpinned-uses (severity: high)

All `uses:` references in pr.yml are pinned to mutable version tags rather than full 40-character commit SHAs, making the workflow vulnerable to supply-chain attacks if the referenced action tags are moved. Failing references: `actions/checkout@v3` and `actions/cache@v3`.

Locations:

- `.github/workflows/pr.yml:9`
- `.github/workflows/pr.yml:11`

### missing-permissions (severity: medium)

The workflow file package.yml has no top-level `permissions:` block and the single job (`package`) also has no job-level `permissions:` block. Without explicit permissions, the workflow inherits the default repository token permissions, which may be overly broad (write access to contents, etc.).

Locations:

- `.github/workflows/package.yml:1`

### missing-permissions (severity: medium)

The workflow file pr.yml has no top-level `permissions:` block and the single job (`lint-code`) also has no job-level `permissions:` block. Without explicit permissions, the workflow inherits the default repository token permissions, which may be overly broad.

Locations:

- `.github/workflows/pr.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all four findings across two workflow files:

**package.yml**:
- Pinned `actions/checkout@v3` → `@f43a0e5ff2bd294095638e18286ca9a3d1956744 # v3`
- Pinned `actions/setup-node@v4` → `@49933ea5288caeca8642d1e84afbd3f7d6820020 # v4`
- Added top-level `permissions: contents: write` (required for the git push step that commits dist/ files)

**pr.yml**:
- Pinned `actions/checkout@v3` → `@f43a0e5ff2bd294095638e18286ca9a3d1956744 # v3`
- Pinned `actions/cache@v3` → `@6f8efc29b200d32929f49075959781ed54ec270c # v3`
- Added top-level `permissions: contents: read` (minimum needed for checkout on a PR workflow)

