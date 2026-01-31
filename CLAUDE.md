# CLAUDE.md

Quick-reference for Claude Code when working in the loaders.gl repository.

## Project overview

loaders.gl is a monorepo with 48 modules under `modules/`, part of the vis.gl ecosystem. It provides framework-independent loaders for visualization, 3D graphics, and geospatial formats. Managed with Lerna + Yarn 4 workspaces.

## Setup

```bash
yarn install
```

Requires Node >= 20.18.0 and Yarn 4.12.0.

## Build commands

| Command | Description |
|---|---|
| `yarn build` | Build all modules (alias for `yarn build-modules`) |
| `yarn build-workers` | Build worker bundles only |
| `yarn build-apps` | Build applications |

## Test commands

| Command | Description |
|---|---|
| `yarn test` | Run all module tests (alias for `yarn test-modules`) |
| `yarn test-node` | Run tests in Node.js only |
| `yarn test-fast` | Quick subset of tests |
| `yarn test-cover` | Tests with coverage |
| `yarn test-apps` | Run application tests |

## Lint

```bash
yarn lint        # Check formatting and lint
yarn lint fix    # Auto-fix formatting and lint issues
```

Always run `yarn lint fix` after making changes.

## Before committing

1. Run `yarn test node` to verify nothing is broken
2. Run `yarn lint fix` to ensure formatting is correct

## Code style conventions

- **TypeScript strict mode** with `noImplicitAny: false`
- **ESM** throughout (`"type": "module"` in package.json)
- **Prettier config**: 100 char width, semicolons, single quotes, no trailing commas, no bracket spacing
- **Naming**: camelCase (variables, functions, fields), PascalCase (types), CAPITAL_CASE (constants)
- **Functions/methods**: prefer verbNoun naming (e.g., `parseFile`, `loadTileset`)
- **Never abbreviate** variable names; always use full descriptive names
- **Semicolons required** at end of statements
- **Avoid Node-specific imports** unless in dual browser/Node functions (e.g., `fetchFile` handles both environments, so direct `fs` imports are almost never needed)

## Module structure

Each module lives under `modules/<name>/` with:

```
modules/<name>/
  src/          # Source code
  test/         # Tests
  package.json  # Module package config
  tsconfig.json # Module TS config
```

## Key architecture

- `@loaders.gl/core` is the main entry point; most users import `load`, `parse`, etc. from here
- Modules publish ESM + CJS + TypeScript declarations
- Path aliases are defined in the root `tsconfig.json` mapping `@loaders.gl/<name>` to `modules/<name>/src`
- Tests use ocular-test (tape-based test runner from the vis.gl ecosystem)
- Workers are built separately via `yarn build-workers` (runs `pre-build` scripts in each module)
