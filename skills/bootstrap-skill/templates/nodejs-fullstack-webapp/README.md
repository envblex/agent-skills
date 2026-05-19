# Project Name

Briefly describe the Node.js fullstack web application and the current approved scope.

## Current Scope

The current approved scope is defined in `SPEC.md`.

Do not treat ideas, notes, or deferred tasks as current scope unless they are explicitly included in `SPEC.md` or `PATCH_REQUEST.md`.

## Requirements

- Node.js
- pnpm
- Git

Document the exact Node.js and pnpm versions after implementation.

Example:

```bash
node --version
pnpm --version
```

## Setup

Install dependencies:

```bash
pnpm install
```

## Development

Run the development server:

```bash
pnpm run dev
```

Document the local URL after implementation.

Example:

```txt
http://localhost:3000
```

## Build

Build the application:

```bash
pnpm run build
```

## Test

Run tests:

```bash
pnpm test
```

## Verification

Before claiming completion, run the implemented checks.

Expected commands:

```bash
pnpm run format
pnpm run lint
pnpm run typecheck
pnpm run build
pnpm test
```

If a command does not exist, either add it or document that no automated check exists yet.

## Configuration

Use `.env.example` for variable names only.

Do not put real secrets in:

- `.env.example`
- README files
- commit messages
- logs
- issues
- pull requests
- generated documentation

Example:

```txt
PUBLIC_BASE_URL=http://localhost:3000
EXAMPLE_API_KEY=
```

## Application Routes

Document implemented pages and routes only.

Example format:

```txt
GET /

Purpose:
User-visible behavior:
Server behavior:
```

## API Routes

Document implemented API endpoints only.

Example format:

```txt
METHOD /api/path

Purpose:
Request:
Response:
Errors:
```

Do not document routes that are not implemented.

## Data Storage

Document storage only after it is implemented.

If the project has no database, state that clearly.

If storage is added, document:

- storage provider
- local development setup
- required environment variables
- backup or migration notes, if applicable

## Authentication

Document authentication only after it is implemented.

If the project has no authentication, state that clearly.

## Deployment

Document deployment only after the deployment path is implemented.

Default policy:

- deploy through GitHub Actions from `main`
- do not deploy from local machines
- store secrets in GitHub Secrets or the approved secret manager

## Limitations

Document known limitations here.

## Deferred Work

Deferred work belongs in `TODO.md`, not in this README unless it affects current setup, usage, or limitations.
