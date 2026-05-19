# SPEC

## Project Type

Node.js TypeScript API.

## Purpose

Build a small, maintainable HTTP API using Node.js and TypeScript.

## Current Scope

The initial scope should include only:

- TypeScript project setup
- pnpm-based dependency management
- minimal HTTP server
- explicitly listed API routes
- request validation for implemented routes
- structured error responses
- tests for core behavior
- documented local development commands

## Out of Scope

Do not add unless explicitly requested:

- authentication
- authorization
- database integration
- queues
- background jobs
- admin dashboards
- OpenAPI generation
- rate limiting
- caching
- telemetry
- analytics
- observability stack
- deployment automation
- multi-service architecture

## Expected Layout

```txt
src/
  index.ts
  ...
tests/
  ...
package.json
tsconfig.json
README.md
SPEC.md
TODO.md
AGENT.md
```

Do not create unnecessary layers.

Add structure only when the implementation needs it.

## Package Manager

Use pnpm only.

Forbidden:

- `npm install`
- `yarn`
- `bun install`
- `package-lock.json`
- `yarn.lock`
- `bun.lockb`

## API Requirements

The API should:

- validate inputs
- return predictable status codes
- return JSON responses
- return JSON errors
- avoid leaking internal errors
- avoid logging secrets
- keep route behavior documented

## Error Handling

Errors should be explicit and consistent.

The API must not:

- expose stack traces in normal responses
- return HTML error pages for JSON endpoints
- silently ignore invalid input
- claim success when a partial failure occurred

## Security Rules

The API must not:

- store secrets in tracked files
- print secrets in logs
- add permissive CORS unless explicitly required
- add unused environment variables
- perform hidden external network calls
- include authentication bypasses

## Verification

Expected commands:

```bash
pnpm run format
pnpm run lint
pnpm run typecheck
pnpm run build
pnpm test
```

## Completion Criteria

A change is complete only when:

- implemented endpoints match this specification
- undocumented endpoints are not added
- validation behavior is tested
- typecheck passes
- no secrets are introduced
- documentation matches actual behavior
- the diff is scoped and reviewable
