# SPEC

## Project Type

Node.js TypeScript fullstack web application.

## Purpose

Build a small, maintainable fullstack web application using Node.js and TypeScript.

## Current Scope

The initial scope should include only:

- TypeScript project setup
- pnpm-based dependency management
- minimal application shell
- explicitly listed pages or routes
- explicitly listed API endpoints, if needed
- request validation for implemented endpoints
- clear error handling
- build and verification commands
- documentation matching implemented behavior

## Out of Scope

Do not add unless explicitly requested:

- authentication
- authorization
- database integration
- queues
- background jobs
- admin dashboards
- billing or payments
- email sending
- webhooks
- analytics
- telemetry
- rate limiting
- caching
- file uploads
- real-time features
- deployment automation
- multi-service architecture

## Expected Layout

```txt
src/
  ...
public/
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

Framework-specific layouts are allowed only when the chosen framework requires them.

Do not create unnecessary architecture.

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

## Frontend Requirements

The frontend should:

- implement only approved screens and flows
- keep UI simple and accessible
- avoid unnecessary state management libraries
- avoid unnecessary client-side persistence
- avoid hidden network requests
- avoid documenting unimplemented UI

## Backend Requirements

The backend should:

- validate inputs
- return predictable status codes
- return structured errors for API routes
- avoid leaking internal errors
- avoid logging secrets
- keep route behavior documented

## Error Handling

Errors should be explicit and consistent.

The application must not:

- expose stack traces in normal production responses
- silently ignore invalid input
- claim success when a partial failure occurred
- hide server-side failures behind misleading UI messages

## Security Rules

The application must not:

- store secrets in tracked files
- print secrets in logs
- add permissive CORS unless explicitly required
- add unused environment variables
- perform hidden external network calls
- include authentication bypasses
- collect telemetry unless explicitly approved
- store sensitive user data unless explicitly approved

## Environment Variables

Use `.env.example` for variable names only.

Do not commit real secrets.

Document every required variable in `README.md`.

## Verification

Expected commands:

```bash
pnpm run format
pnpm run lint
pnpm run typecheck
pnpm run build
pnpm test
```

If any command is unavailable, document the reason.

## Completion Criteria

A change is complete only when:

- implemented pages and endpoints match this specification
- undocumented behavior is not added
- validation behavior is tested where practical
- typecheck passes
- build passes
- no secrets are introduced
- documentation matches actual behavior
- the diff is scoped and reviewable
