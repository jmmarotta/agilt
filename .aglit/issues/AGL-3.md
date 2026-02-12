---
schema: aglit.issue.md.v1
id: 019c52a1-ef8e-7000-9835-fb607ca28b17
status: planned
priority: high
---

# Add GitHub Actions CI workflow for running tests

## Description

Create a dedicated GitHub Actions CI workflow that runs the repository test suite automatically on pull requests and default-branch pushes.

## Acceptance

- Add a CI workflow under `.github/workflows/` triggered by `pull_request` and pushes to the default branch.
- CI installs dependencies with Bun and runs tests via existing repo script (`bun test` or `bun run test`).
- CI uses cache strategy for Bun dependencies to improve repeat run performance.
- Test results appear as GitHub checks and fail the workflow when tests fail.
- Document CI test triggers and local equivalent command in repository docs.

## Constraints

- Keep this issue scoped to test execution only (no publish/release steps).
- Do not require secrets so workflow is safe on forked PRs.
- Reuse existing scripts and avoid duplicating test logic in YAML.

## Plan

- Confirm test command and any environment assumptions.
- Implement CI workflow with Bun setup, cache, install, and test step.
- Add concise docs for when tests run and how to run them locally.

## Verification

- Open a test PR and confirm the CI test workflow runs and passes.
- Introduce an intentional failing test on a branch and confirm CI fails.
- Re-run workflow to confirm cache hits and stable behavior.
