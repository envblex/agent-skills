---
name: implement-skill
description: Implement the approved current scope from SPEC.md. Use when the user wants an AI agent to read SPEC.md, TODO.md, README.md, and AGENT.md; inspect the codebase; edit only the code needed for the current scope; run available verification commands; update README.md when behavior changes; and commit the completed work.
metadata:
  author: Envblex
---

# Implement Skill

Implement only the approved current scope defined in `SPEC.md`.

## Purpose

Turn the current project specification into working code without expanding scope.

`SPEC.md` is the source of truth. `TODO.md` is only a parking lot for later work.

## Mandatory Stack Rules

- Always use `pnpm` in Node.js projects.
- Do not use `npm`, `yarn`, or `bun` unless explicitly stated in an existing project.
- Use TypeScript instead of JavaScript.
- New files should, in principle, be named `.ts` or `.tsx`.
- Do not create `package-lock.json`, `yarn.lock`, or `bun.lockb`.
- Always use `pnpm add` or `pnpm add -D` to add dependencies.
- Check `package.json`, `pnpm-lock.yaml`, `tsconfig.json`, and `AGENT.md` before implementation.

## Workflow

1. Read `SPEC.md`, `TODO.md`, `README.md`, and `AGENT.md`.
2. Identify:
   - current scope
   - non-goals
   - completion criteria
   - required commands or checks
3. Inspect the existing codebase before editing.
4. Plan the smallest code change that satisfies `SPEC.md`.
5. Implement only that change.
6. Do not implement items marked as later, future, optional, or TODO.
7. Update `README.md` only if usage, behavior, commands, generated files, limitations, or file layout changed.
8. Run available verification commands.
9. Review `git status` and the diff.
10. Commit only the completed implementation if the repository is ready.

## Scope Rules

- Follow `SPEC.md` over all other documents when requirements conflict.
- Use `README.md` to understand expected usage, not to invent missing features.
- Use `AGENT.md` for repository-specific rules.
- Treat `TODO.md` as out of scope unless `SPEC.md` explicitly includes an item.
- Do not add optional features, integrations, refactors, redesigns, or polish unless required by `SPEC.md`.
- Do not make broad architectural changes when a local change is enough.
- Do not revert unrelated user changes.

## Implementation Rules

- Inspect existing patterns before creating new ones.
- Follow the current project structure and code style.
- Prefer simple, explicit code over premature abstraction.
- Keep changes small and reviewable.
- Add or update tests only when the project already has a test pattern or `SPEC.md` requires it.
- Do not hide failing checks.
- Do not claim verification passed unless the command was actually run and passed.

## README Updates

Update `README.md` when the implementation changes any of the following:

- setup steps
- commands
- usage flow
- generated files
- configuration
- examples
- limitations
- file layout
- behavior visible to users

Do not document features that are not implemented.

## Verification

Run the commands provided by the project, such as:

- dependency install/check commands
- format
- lint
- typecheck
- build
- test

If commands are missing, inspect the project and state that no automated verification command is available.

If a command fails, report:

- the command
- the failure summary
- the likely cause
- whether the implementation is still complete

## Commit Rules

Before committing:

1. Run `git status`.
2. Review the diff.
3. Confirm only intended files changed.
4. Exclude unrelated user changes.
5. Commit with a concise message describing the implemented scope.

Use a commit message like:

```txt
Implement <current scope>
````

Do not commit if:

* required checks fail because of the new implementation
* the repository has unrelated unresolved changes
* the current scope is ambiguous enough that implementation would be guesswork

## Final Response

Report:

* what was implemented
* what files changed
* what verification commands were run
* whether the commit was created
* any remaining risks or skipped items
