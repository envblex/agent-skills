# Project Name

Briefly describe the Node.js API project and the current approved scope.

## Current Scope

The current approved scope is defined in `SPEC.md`.

Do not treat ideas, notes, or deferred tasks as current scope unless they are explicitly included in `SPEC.md` or `PATCH_REQUEST.md`.

## Requirements

- Node.js
- pnpm

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

Build the project:

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

Example:

```txt
PORT=3000
EXAMPLE_API_KEY=
```

## API Routes

Document implemented endpoints only.

Example format:

```txt
METHOD /path

Purpose:
Request:
Response:
Errors:
```

Do not document routes that are not implemented.

## Error Format

Document the actual error response format after implementation.

Example:

```json
{
  "error": {
    "code": "invalid_request",
    "message": "Invalid request"
  }
}
```

## Deployment

Document deployment only after the deployment path is implemented.

Default policy:

- deploy through GitHub Actions from `main`
- do not deploy from local machines
- store secrets in GitHub Secrets or the approved secret manager

## Limitations

Document known limitations here.

## Deferred Work

Deferred work belongs in `TODO.md`, not in this README unless it affects current usage.
