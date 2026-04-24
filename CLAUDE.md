# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

`@acehubert/testlink-mcp` is a TestLink MCP (Model Context Protocol) server that exposes TestLink test management operations to AI assistants. It ships as both an MCP server (`testlink-mcp`) and a CLI (`testlink`).

## Build & Development Commands

```bash
yarn install                  # Install dependencies
yarn build                    # Full build (types + ESM + CJS + CLI)
yarn dev                      # Run dev server via ts-node
yarn typecheck                # TypeScript type checking (tsc --noEmit)
yarn lint                     # Lint all TS files
yarn lint:fix                 # Auto-fix lint issues
yarn commit                   # Commit via commitizen (conventional commits)
```

Build produces four outputs: `dist/index.js` (ESM), `dist/index.cjs` (CJS), `dist/index.d.ts` (types), and `dist/cli.cjs` (CLI entry).

## Architecture

Three source files, each with a distinct role:

- **`src/api.ts`** — `TestLinkAPI` class wrapping `testlink-xmlrpc`. All TestLink XML-RPC calls go through `handleAPICall` which normalizes error codes (auth, permission, not-found) into descriptive errors. Test case IDs accept both numeric (`123`) and external format (`PREFIX-123`), resolved by `parseTestCaseId`.
- **`src/index.ts`** — MCP server entry. Defines 20+ MCP tools as a static `tools` array, routes `CallToolRequest` to the corresponding `TestLinkAPI` method. Also exports `createTestLinkMcpServer` and `TestLinkAPI` for programmatic use.
- **`src/cli.ts`** — yargs-based CLI. Each resource (projects, cases, suites, plans, builds, executions, requirements) is a `<resource> <action>` command pattern, delegating to the same `TestLinkAPI`.

Both entry points resolve connection config from `--url`/`--apiKey` flags or `TESTLINK_URL`/`TESTLINK_API_KEY` env vars. The RPC path is auto-derived as `{host}/lib/api/xmlrpc/v1/xmlrpc.php`.

## Conventions

- **Package manager**: Yarn 3 (`yarn@3.2.0`), PnP disabled (`.yarnrc.yml`)
- **Module format**: ESM (`"type": "module"`), compiled to both ESM and CJS via esbuild
- **TypeScript**: strict mode, `NodeNext` module resolution, target ES2022
- **Code style**: double quotes, semicolons, trailing commas, 100 char print width (see `prettier.config.cjs`)
- **Commits**: Conventional Commits enforced by commitlint + commitizen (`yarn commit`)
- **Pre-commit**: lint-staged runs eslint --fix on `.ts`, prettier on all text files
- **Publishing**: Lerna manages versioning; push to `master` triggers auto-publish via GitHub Actions (`deploy.yml`)
- **Error handling in api.ts**: XML-RPC error arrays are caught in `handleAPICall` and mapped to typed `Error` instances. Network errors (`ECONNREFUSED`, `ETIMEDOUT`) get specific messages.

## Environment Variables

Required for both MCP server and CLI:

```
TESTLINK_URL=http://your-testlink-host/testlink
TESTLINK_API_KEY=your-api-key
```

Copy `.env.example` to `.env` for local development.
