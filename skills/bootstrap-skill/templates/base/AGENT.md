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

- `npm install`
- `yarn`
- `bun install`
- `package-lock.json`
- `yarn.lock`
- `bun.lockb`

Use:

- `pnpm install`
- `pnpm add <package>`
- `pnpm add -D <package>`
- `pnpm run <script>`

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
- Do not hide failing checks.
- Do not claim completion without verification.
- Do not introduce secrets, tokens, credentials, or private configuration into tracked files.

## Git Authority Model

This repository separates human authority from agent authority.

### Human-only operations

The following operations are reserved for the repository owner or a human maintainer:

- Pushing directly to `main`
- Committing directly on `main`
- Merging pull requests
- Approving pull requests as the final repository authority
- Creating, editing, or deleting repository secrets
- Creating, rotating, or exposing deploy tokens
- Changing GitHub Actions deployment settings
- Changing branch protection rules or repository rules
- Changing repository visibility, access, collaborators, or permissions
- Running production deployment commands outside GitHub Actions
- Force-pushing shared branches
- Deleting remote branches unless explicitly requested by the user
- Rewriting shared Git history unless explicitly requested by the user

### Agent-allowed operations

An agent may perform the following operations only on a non-main working branch:

- Inspect repository state with read-only Git commands
- Create a feature, fix, docs, chore, or refactor branch
- Edit files within the approved task scope
- Run format, lint, typecheck, build, and test commands
- Commit task-scoped changes
- Push a non-main branch only when explicitly allowed by the user
- Create a pull request targeting `main` only when explicitly allowed by the user

### Agent-forbidden operations

An agent must not:

- Push to `main`
- Commit directly on `main`
- Merge pull requests
- Use `--force` or `--force-with-lease` unless the user explicitly requests history repair
- Modify Git remotes without explicit approval
- Store secrets in files, logs, commits, issues, pull requests, or generated documentation
- Run production deployment commands from a local machine
- Bypass checks, hooks, reviews, branch rules, or documented gates
- Expand scope beyond the current task
- Change repository permissions, tokens, secrets, deploy settings, or branch protection
- Convert a documentation-only task into an implementation task
- Delete files outside the approved scope

### Required Git gate

Before changing files, the agent must run:

```bash
git status -sb
git branch --show-current
```

If the current branch is `main`, the agent must stop and create a non-main branch before modifying files.

Before committing, the agent must run:

```bash
git status -sb
git diff --check
```

After committing, the agent must run:

```bash
git status -sb
```

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

Documentation must distinguish between:

- implemented behavior
- planned behavior
- deferred work
- operational policy
- local development instructions

Do not present planned or deferred work as available functionality.

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

Do not hide failing checks. Do not replace a failing verification result with a weaker command unless the reason is explicitly documented.

## Git Rules

Before modifying files:

1. Run `git status -sb`.
2. Run `git branch --show-current`.
3. Confirm the current branch is not `main`.
4. If on `main`, create a task branch before editing.

Before committing:

1. Run `git status -sb`.
2. Review the diff.
3. Run `git diff --check`.
4. Confirm only intended files changed.
5. Exclude unrelated user changes.
6. Commit with a concise message describing the completed scope.

Do not commit if:

- the current branch is `main`
- required checks fail because of the current change
- unrelated unresolved changes are present
- the scope is ambiguous enough that implementation would be guesswork
- secrets, tokens, credentials, or private configuration appear in the diff

Do not push unless the user explicitly requests it.

If pushing is requested, push only the current non-main branch:

```bash
git push -u origin <current-branch>
```

Never push to `main`.

## Deployment Rules

Production deployment must be controlled by GitHub Actions from `main`.

Agents must not:

- deploy from a local machine
- deploy from a feature branch
- create deploy credentials
- edit deploy credentials
- print deploy credentials
- bypass GitHub Actions
- weaken CI or deployment gates

If deployment requires secrets, store them only in GitHub Secrets or the approved secret manager for the project.

Do not write secrets into:

- `.env`
- `.env.example`
- README files
- issue bodies
- pull request bodies
- commit messages
- logs
- generated documentation

`.env.example` may document variable names only. It must not contain real secret values.

## Final Response

Report:

- what changed
- files changed
- verification commands and results
- whether a commit was created
- whether a branch was pushed
- whether a pull request was created
- remaining risks or gaps
