# Development and Publishing

This document defines how we version and publish AGLIT packages during the
temporary personal-scope phase.

## Package Names (Temporary)

- CLI: `aglit`
- SDK: `@jmmarotta/aglit-sdk`
- OpenCode plugin: `@jmmarotta/opencode-aglit`

Notes:

- `aglit` stays unscoped so global installs and `bunx` are simple.
- Package versions are independent.
- The CLI is bundled so local file installs do not require workspace resolution
  of SDK dependencies.

## Versioning Strategy

- Use per-package semantic versioning.
- Bump only packages that changed.
- Keep package versions in each package's `package.json` (no shared `VERSION`
  file).

Common bump commands:

```bash
# patch bump
bun --cwd packages/cli pm version patch --no-git-tag-version
bun --cwd packages/sdk pm version patch --no-git-tag-version
bun --cwd packages/opencode-plugin pm version patch --no-git-tag-version

# explicit version
bun --cwd packages/cli pm version 0.1.0 --no-git-tag-version
```

If you want git tags/commits from version bumps, remove
`--no-git-tag-version`.

## Pre-Publish Checklist

Run from repo root:

```bash
bun fmt
bun lint
bun typecheck
bun test
bun run build
```

Confirm npm auth:

```bash
bun pm whoami
```

## Publish Flow

Publish from each package directory.

### SDK

```bash
cd packages/sdk
bun publish --dry-run
bun publish --access public
```

### OpenCode Plugin

```bash
cd packages/opencode-plugin
bun publish --dry-run
bun publish --access public
```

### CLI

```bash
cd packages/cli
bun publish --dry-run
bun publish --access public
```

Post-publish verification:

```bash
bunx aglit --help
bun install -g aglit
aglit --help
```

## Local Install and Smoke Test

From this repo:

```bash
bun run build -C packages/cli
bun add -g file:./packages/cli
aglit --help
```

Alternative (publish-like tarball install):

```bash
cd packages/cli
TARBALL=$(bun pm pack --quiet)
bun add -g "./$TARBALL"
aglit --help
```

## Turbo Publish Automation

This repo is configured with:

- `publish:dry` and `publish:run` scripts per package
- `publish:dry` and `publish:run` tasks in `turbo.json`
  - `cache: false`
  - `dependsOn`: `build`

From repo root, run filtered, serial publishes:

```bash
turbo run publish:dry --concurrency 1 --filter aglit --filter '@jmmarotta/aglit-sdk' --filter '@jmmarotta/opencode-aglit'
turbo run publish:run --concurrency 1 --filter aglit --filter '@jmmarotta/aglit-sdk' --filter '@jmmarotta/opencode-aglit'
```

Convenience aliases also exist at root:

```bash
bun run publish:dry
bun run publish
```
