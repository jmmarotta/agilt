---
schema: aglit.issue.md.v1
id: 019c525b-7b79-7000-ba75-c4712db98c79
status: planned
priority: medium
---

# Run repository cleanup and consistency audit

## Description

Do a focused cleanup pass across code, tooling, and docs to ensure the repository is coherent, conventions are consistent, and unused artifacts are removed.

## Acceptance

- Identify and remove unused files, stale scripts, and dead code paths that are safe to delete.
- Reconcile naming and structure inconsistencies across packages, scripts, and documentation.
- Validate dependency usage and remove clearly unused dependencies.
- Update docs where behavior or conventions change.
- End state passes lint, typecheck, tests, and build.

## Constraints

- Preserve current behavior and public interfaces unless a change is explicitly documented.
- Avoid broad refactors unrelated to consistency/cleanup goals.
- Keep changes incremental and easy to review.

## Plan

- Inventory likely cleanup targets (files, scripts, deps, docs, package boundaries).
- Confirm usage before deletion via search/build/test signals.
- Apply cleanup in small, coherent commits grouped by concern.
- Run full validation suite and adjust docs to match reality.

## Verification

- `bun lint` passes.
- `bun typecheck` passes.
- `bun test` passes.
- `bun run build` passes.
- Manual spot-check of key CLI flows still works (`aglit --help`, `aglit list`).
