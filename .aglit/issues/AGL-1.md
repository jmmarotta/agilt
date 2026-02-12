---
schema: aglit.issue.md.v1
id: 019c525b-7b59-7000-94ad-c09f68a4c745
status: planned
priority: high
---

# Set up GitHub Actions CD publish pipeline

## Description

Create a GitHub Actions delivery workflow focused only on release and publish automation (npm and related package publishing) for this monorepo, using a manual operator-first flow.

## Acceptance

- Add a CD workflow (for example `.github/workflows/cd-publish.yml`) for publish/release jobs only.
- CD uses `workflow_dispatch` as the primary trigger, with an input mode (`dry-run` or `publish`).
- Workflow does not run on `pull_request`; CI/test automation remains tracked in `AGL-3`.
- Jobs use existing repo scripts (`bun run publish:dry`, `bun run publish`) rather than duplicating publish logic in YAML.
- Publish job is secret-gated and restricted to trusted context (`main` and protected GitHub Environment).
- Workflow includes concurrency control to prevent overlapping publish runs.
- Document triggers, required secrets, environment protection, and release operator steps in `DEVELOPMENT.md`.

## Constraints

- Use Bun and existing repo scripts; avoid duplicating command logic in workflow files.
- Do not include PR validation/test jobs here; that work is tracked separately.
- Keep publish flow safe for forks/untrusted branches and prevent accidental release.
- Support mixed publish tooling used by packages (`npm publish` and `bun publish`) via compatible auth env setup.
- Keep workflow permissions least-privilege and jobs auditable (clear names, explicit conditions, timeouts).

## Plan

- Define workflow contract: manual trigger, dispatch inputs (`mode`), trusted refs, environment name, and required secrets.
- Implement `dry-run` job: checkout, Bun setup, dependency install, and `bun run publish:dry`.
- Implement `publish` job that depends on `dry-run` and runs only when `mode=publish`, on `main`, with protected environment approval.
- Add release hardening: `concurrency`, minimal `permissions`, and `timeout-minutes`.
- Configure npm auth env vars for both toolchains (`NODE_AUTH_TOKEN` and `NPM_CONFIG_TOKEN`).
- Update `DEVELOPMENT.md` with CD usage, guardrails, and troubleshooting notes.

## Verification

- Trigger `dry-run` from a non-main branch and confirm it succeeds without publishing.
- Trigger `publish` from a non-main branch and confirm publish is blocked/skipped.
- Trigger `publish` on `main` with environment approval and confirm expected packages/versions are released.
- Validate missing/invalid secret behavior fails safely with actionable logs.
- Confirm docs match workflow triggers, required secrets, and operator steps.
