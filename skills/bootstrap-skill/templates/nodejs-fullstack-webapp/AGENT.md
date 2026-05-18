# AGENT: nodejs-fullstack-webapp

## Stack

Use this stack unless `SPEC.md` explicitly says otherwise:

- Node.js
- TypeScript
- pnpm
- React Router / Remix-style full-stack app
- Cloudflare Workers runtime
- Tailwind CSS
- API routes under `/api`

## Project Rules

- Use Cloudflare Workers as the deployment/runtime target.
- Do not introduce a Node-only server runtime unless `SPEC.md` requires it.
- Keep browser code and server-only code separated.
- Keep API behavior explicit and documented.
- Keep UI changes limited to the current scope.
- Do not add authentication, database, analytics, queues, background jobs, or external services unless `SPEC.md` requires them.
- Do not add large UI libraries unless the project already uses them or `SPEC.md` requires them.

## Routing Rules

- Use React Router / Remix-style route conventions already present in the project.
- Keep user-facing pages and API handlers separated.
- Put API endpoints under `/api` when the project structure supports it.
- Do not change existing route URLs unless required by `SPEC.md`.

## API Rules

- Validate request input at the boundary.
- Return consistent JSON responses for API endpoints.
- Use proper HTTP status codes.
- Avoid leaking stack traces or internal errors to clients.
- Do not change response shapes unless required by `SPEC.md`.

## UI Rules

- Prefer simple, accessible UI over decorative complexity.
- Use existing components and styling patterns before creating new ones.
- Keep Tailwind classes readable.
- Do not implement optional polish outside the current scope.
- Do not introduce animations unless required by `SPEC.md`.

## Cloudflare Rules

- Keep Worker-compatible code.
- Avoid Node-specific APIs that are unavailable in Cloudflare Workers.
- Do not add secrets to source files.
- Use environment bindings only when required by `SPEC.md`.
- Update `wrangler.toml` only when runtime configuration actually changes.

## README Update Triggers

Update `README.md` when changing:

- setup commands
- dev/build/deploy commands
- environment variables
- routes
- API endpoints
- configuration
- runtime assumptions
- user-visible behavior

## Verification

Run available commands, usually:

```bash
pnpm install
pnpm check
pnpm build
pnpm test
```

If the project uses different scripts, inspect package.json and run the relevant ones.

At minimum, check:

```bash
pnpm lint
pnpm typecheck
pnpm build
```

when those scripts exist.

Common Failure Points

Watch for:

- code that works in Node.js but not Cloudflare Workers
- server-only code imported into client bundles
- client code importing secrets or environment bindings
- API response shape drift
- README documenting routes or commands that do not exist
- Tailwind/config changes unrelated to the current scope
