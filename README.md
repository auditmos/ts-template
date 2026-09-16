# ts-template

TypeScript template for building tool and service projects with modern tooling, strict type-checking, and automated releases.

## Prerequisites

- [Node.js](https://nodejs.org/) >= 22
- [pnpm](https://pnpm.io/)

## Setup

```bash
pnpm install
```

This installs dependencies and configures git hooks automatically via the `prepare` script.

## Scripts

| Command | Description |
|---------|-------------|
| `pnpm build` | Build with tsup (ESM + declarations) |
| `pnpm lint` | Check code with Biome |
| `pnpm lint:fix` | Auto-fix lint/format issues |
| `pnpm types` | Type-check with tsc --noEmit |
| `pnpm test` | Run tests with Vitest |
| `pnpm test:watch` | Run tests in watch mode |
| `pnpm unused` | Detect unused code with Knip |
| `pnpm update` | Interactive dependency updates with Taze |

## Architecture

Code here follows **deep modules** (Ousterhout): a small interface over a large implementation. A module hides complexity behind one entry point rather than scattering it across many tiny files.

- A domain starts as a single file — `src/lib/{domain}.ts`
- Once it grows internals, it becomes `src/lib/{domain}/` with `index.ts` as its only entry
- Export only what a caller needs; `pnpm unused` fails CI on exports nobody imports

Full rules — boundaries, growth path, enforcement — are in [AGENTS.md](AGENTS.md).

## Development Workflow

1. Write tests co-located with source files (`*.test.ts`)
2. Use TDD: write a failing test, make it pass, refactor
3. Commit using [Conventional Commits](https://www.conventionalcommits.org/) (`feat:`, `fix:`, etc.)
4. Pre-commit hooks automatically run linting and tests
5. Push to `main` triggers CI checks and semantic-release

### Skills

The template ships one Claude Code skill — `/environment-variables`, which walks you through adding a validated env var. See **[HOWTO.md](HOWTO.md)**.

### Committing

When you're done implementing, just ask Claude Code to commit:

```
commit this
```

Claude Code will stage the relevant files, write a conventional commit message based on the changes, and run the pre-commit hook (lint + tests) automatically. If the hook fails, it will fix the issues and retry.

You can also use the built-in shortcut:

```
/commit
```

## Environment Variables

Environment configuration uses `@t3-oss/env-core` with Zod validation:

- `.env` — Development defaults (committed)
- `.env.local` — Secrets and overrides (gitignored)

Define schemas in `src/lib/env.ts`.

## CI/CD

GitHub Actions runs on push to `main`:

1. Lint, type-check, test, and unused code detection
2. If all checks pass, semantic-release creates a GitHub Release with tag

## Contributing

1. Create a feature branch
2. Make changes following the existing patterns
3. Ensure `pnpm lint && pnpm types && pnpm test && pnpm unused` all pass
4. Open a PR against `main`
