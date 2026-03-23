# test-renovatebot-post-upgrade-tasks

Minimal reproduction repository for investigating a Renovate bug where
`postUpgradeTasks` fail to execute when multiple branches are created
in a single Renovate run.

## Issue

When Renovate creates multiple new branches in a single run:

1. The first branch may get fully processed (postUpgradeTasks run)
2. Subsequent branches abort with `REPOSITORY_CHANGED` error
3. The postUpgradeTasks for those branches never execute

## Expected Behavior

Each mise dependency update PR should include changes to both:

- `mise.toml` (version bump)
- `mise.lock` (re-locked checksums)

## Actual Behavior

Some PRs only have `mise.toml` changes, missing the `mise.lock` updates.

## Setup

This repo uses:

- `mise.toml` with several tools pinned to older versions
- `renovate.json5` with `postUpgradeTasks` that run `mise lock` after version bumps
- GitHub Actions workflow to run Renovate with debug logging enabled

## Related

- Original issue observed in: https://github.com/altendky/onshape-mcp
