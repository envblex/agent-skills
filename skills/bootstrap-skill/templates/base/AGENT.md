# AGENT

## Required Reading

Before working, read:

- `SPEC.md`
- `TODO.md`
- `README.md`
- `AGENT.md`

If `PATCH_REQUEST.md` exists, read it before `SPEC.md` and treat it as the current patch scope.

## Package Manager

This repository uses pnpm only.

Forbidden:

- npm install
- yarn
- bun install
- package-lock.json
- yarn.lock
- bun.lockb

Use:

- pnpm install
- pnpm add <package>
- pnpm add -D <package>
- pnpm run <script>

## Language

This repository uses TypeScript.

- Prefer `.ts` and `.tsx`.
- Do not introduce `.js` unless the project already requires it.
- Keep type checking enabled.

## Source Of Truth

Use documents in this order:

1. `PATCH_REQUEST.md`, if present
2. `SPEC.md`
3. `AGENT.md`
4. `README.md`
5. `TODO.md`

`TODO.md` is deferred work. Do not treat it as current implementation scope unless `SPEC.md` or `PATCH_REQUEST.md` explicitly includes an item.

## Core Rules

- Implement only the approved current scope.
- Keep changes small and reviewable.
- Prefer simple local changes over broad abstractions.
- Follow the existing project structure.
- Do not add optional features.
- Do not perform broad refactors unless required by the current scope.
- Do not revert unrelated user changes.
- Do not push to `main`.
- Do not hide failing checks.

## Documentation Rules

Update `README.md` only when actual behavior changes, including:

- setup steps
- commands
- usage flow
- generated files
- configuration
- limitations
- public behavior
- file layout

Do not document features that are not implemented.

## Verification Rules

Run available verification commands before claiming completion.

Examples:

- format
- lint
- typecheck
- build
- test

If a command fails, report:

- the command
- the failure summary
- likely cause
- whether the failure is caused by the current change

If no automated verification exists, state that clearly.

## Git Rules

Before committing:

1. Run `git status`.
2. Review the diff.
3. Confirm only intended files changed.
4. Exclude unrelated user changes.
5. Commit with a concise message describing the completed scope.

Do not commit if:

- required checks fail because of the current change
- unrelated unresolved changes are present
- the scope is ambiguous enough that implementation would be guesswork

## Final Response

Report:

- what changed
- files changed
- verification commands and results
- whether a commit was created
- remaining risks or gaps
