# How to Use Claude Code Skills

This template ships with a Claude Code skill in `.claude/skills/` that handles the one part of the workflow with rules worth encoding: environment variables.

## Available Skills

| Skill | Command | Purpose |
|-------|---------|---------|
| Environment Variables | `/environment-variables` | Add and validate environment variables |

## Environment Variables

```
/environment-variables
```

When you need a new env var at any point, this skill walks you through:

1. Adding the Zod schema to `src/lib/env.ts` — `client`, `server`, or `shared`
2. Placing the value in `.env` (defaults) or `.env.local` (secrets)
3. Adding a validation test to `src/lib/env.test.ts`

It also sets up `@t3-oss/env-core` + Zod from scratch if `src/lib/env.ts` does not exist yet.

### Example

```
/environment-variables
> "I need a WEATHER_API_KEY for the forecast client"
> Claude adds the schema, puts the secret in .env.local, and writes the test
```

## Everything Else

The rest of the workflow is plain Claude Code — no skill required:

- **Planning** — describe what you want to build and ask Claude Code to explore the codebase first
- **Implementation** — TDD with vertical slices: one failing test, minimal code to pass, refactor, repeat
- **Committing** — say `commit this`, or use the built-in `/commit`

See [README.md](README.md) for the full development workflow.
