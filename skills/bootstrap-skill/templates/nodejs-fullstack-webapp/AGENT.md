# AGENT

This file defines additional rules for Node.js fullstack web applications.

It extends the base `AGENT.md` template. These rules must not weaken the base rules.

## Project Type

This project is a Node.js TypeScript fullstack web application.

A fullstack web application may include:

- browser UI
- server-side routes or API handlers
- shared validation/schema code
- build and deployment configuration

Do not add these parts unless they are required by the approved scope.

## Required Reading

Before working, read:

- `SPEC.md`
- `TODO.md`
- `README.md`
- `AGENT.md`

If `PATCH_REQUEST.md` exists, read it first and treat it as the current patch scope.

## Language

Use TypeScript.

Rules:

- Prefer `.ts` and `.tsx`.
- Do not introduce `.js` unless required by the framework or build system.
- Keep type checking enabled.
- Do not weaken TypeScript strictness without explicit approval.

## Package Manager

Use pnpm only.

Forbidden:

- `npm install`
- `yarn`
- `bun install`
- `package-lock.json`
- `yarn.lock`
- `bun.lockb`

Allowed:

- `pnpm install`
- `pnpm add <package>`
- `pnpm add -D <package>`
- `pnpm run <script>`

## Source Layout

Prefer a small layout.

Example:

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

Do not create unnecessary layers.

Avoid premature directories such as:

- `services/`
- `repositories/`
- `providers/`
- `plugins/`
- `adapters/`
- `domain/`
- `infrastructure/`

unless the implementation actually needs them.

## Frontend Rules

The frontend must:

- implement only approved screens and flows
- keep UI state simple
- validate user input before submission when practical
- avoid unnecessary client-side persistence
- avoid analytics or tracking unless explicitly approved
- avoid hidden network requests
- avoid large UI dependencies unless justified by scope

Do not add:

- account UI
- dashboards
- settings pages
- billing UI
- onboarding flows
- admin screens

unless explicitly required by the current scope.

## Backend Rules

The backend must:

- validate request input
- return predictable status codes
- avoid leaking internal errors
- avoid logging secrets
- document implemented endpoints
- keep server behavior aligned with `SPEC.md`

Do not add:

- authentication
- database integration
- queues
- background jobs
- caching
- rate limiting
- webhooks
- admin APIs

unless explicitly required by the current scope.

## Dependency Rules

Add dependencies only when needed by the approved scope.

Do not add dependencies for hypothetical future features.

Avoid heavy frameworks or libraries when the project only needs a small implementation.

## Security Rules

Do not:

- store secrets in tracked files
- print secrets in logs
- expose stack traces in production responses
- add permissive CORS by default
- add authentication bypasses
- add unused environment variables
- add hidden external network calls
- collect telemetry unless explicitly approved
- store sensitive user data unless explicitly approved

## Environment Variables

Use `.env.example` for variable names only.

`.env.example` must not contain real secrets.

Document required variables in `README.md`.

## Verification

Before claiming completion, run available commands.

Expected commands:

```bash
pnpm run format
pnpm run lint
pnpm run typecheck
pnpm run build
pnpm test
```

If a command does not exist, report that clearly.

## Deployment Rules

Follow the base deployment policy.

Default policy:

- production deployment must run through GitHub Actions from `main`
- agents must not deploy from local machines
- agents must not create or rotate deploy credentials
- secrets must be stored in GitHub Secrets or the approved secret manager

Do not add deployment configuration unless deployment is in the approved scope.

## Git Rules

Follow the base Git authority model.

Do not:

- commit on `main`
- push to `main`
- force-push
- merge pull requests
- create or edit secrets
- change repository settings
- deploy locally
