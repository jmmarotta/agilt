# AGLIT - Agentic Long-context Issue Tracker

AGLIT is a repo-local issue tracker built for OpenCode. It stores issues and
projects as Markdown in `.aglit/` and exposes a minimal CLI plus an OpenCode
plugin (protocol-only).

See `plans/major_overhaul.md` for the current design plan.

## Packages

- `packages/sdk` (@jmmarotta/aglit-sdk): ids, storage, headers, views
- `packages/cli` (aglit): CLI wrapper over the SDK
- `packages/opencode-plugin` (@jmmarotta/opencode-aglit): OpenCode protocol injection

## Setup

```bash
bun install
```

Requires Bun 1.3+ (uses Bun YAML/Markdown APIs).

## Common commands

```bash
bun test
bun lint
bun fmt:check
bun typecheck
bun run build
```

## CLI

Install (published):
```bash
bun install -g aglit
aglit --help
```

Run without installing (published):
```bash
bunx aglit --help
```

Local development from a checkout (`bun link`):
```bash
# from this repo root
bun run --cwd packages/cli build
bun link --cwd packages/cli

# now available in your shell
aglit --help
```

Optional: in another project directory, link it as a dependency:
```bash
bun link aglit
```

Publish-like local install check (tarball):
```bash
bun run --cwd packages/cli build
TARBALL="$(bun pm --cwd packages/cli pack --quiet --ignore-scripts --destination "$PWD/packages/cli" | tr -d '\n')"
bun remove -g aglit
bun add -g "$TARBALL"
aglit --help
```

Use this tarball flow as a packaging smoke test. For day-to-day local CLI usage,
prefer `bun link --cwd packages/cli`.

```bash
aglit init --prefix ABC
aglit new "Title" [--status planned] [--priority high] [--project major-refactor]
aglit list [--status active] [--project major-refactor] [--group none]
aglit project new "Major refactor" [--slug major-refactor]
aglit projects [--status active]
aglit check
```

Issue prefix is required; set it via `aglit init --prefix` or pass `--prefix` to `aglit new`.

## OpenCode workflow

- Use `bash` to run `aglit list`, `aglit projects`, `aglit new`, `aglit project new`.
- Use file tools to edit `.aglit/issues/*.md` and `.aglit/projects/*.md` directly.

## Git hooks

If `lefthook` is installed, pre-commit runs `bun fmt:check` + `bun lint` and
pre-push runs `bun typecheck` + `bun test`.
