<!-- markdownlint-disable -->

# Hardening Report: machulav--ec2-github-runner/v2.4.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **machulav--ec2-github-runner/v2.4.3** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files reference GitHub Actions using mutable version tags instead of full 40-character commit SHA digests, making them vulnerable to supply-chain attacks if the tag is moved. Failing references: package.yml — `actions/checkout@v3`, `actions/setup-node@v4`; pr.yml — `actions/checkout@v3`, `actions/cache@v3`.

Locations:

- `.github/workflows/package.yml:12`
- `.github/workflows/package.yml:15`
- `.github/workflows/pr.yml:8`
- `.github/workflows/pr.yml:10`

### missing-permissions (severity: medium)

Neither workflow file declares a top-level `permissions:` block, and no individual job declares its own `permissions:` block. Without explicit permissions, the GITHUB_TOKEN is granted its default (broad) permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/package.yml:1`
- `.github/workflows/pr.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files: (1) Pinned all action references to full 40-char SHAs — actions/checkout@v3 → a37ce9120846195fa4ece8f58b268e6043cb2f26, actions/setup-node@v4 → 49933ea5288caeca8642d1e84afbd3f7d6820020, actions/cache@v3 → 6f8efc29b200d32929f49075959781ed54ec270c — with original tags preserved as inline comments. (2) Added top-level permissions blocks: package.yml gets `contents: write` (required for git push), pr.yml gets `contents: read` (read-only checkout and lint).

