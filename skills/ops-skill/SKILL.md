---
name: ops-skill
description: Enforce repository operations discipline, Git safety gates, verification reporting, deployment boundaries, and completion criteria.
---

# Ops Skill

Use this skill when checking repository state, preparing commits, reviewing operational safety, or deciding whether work is complete.

This skill is about operations discipline. It does not implement product features.

## Core Principle

Repository operations must be explicit, auditable, and reversible.

Agents may assist with local work on non-main branches, but humans retain authority over protected operations.

## Required Reading

Before making operational decisions, read:

- `AGENT.md`
- `SPEC.md`
- `TODO.md`
- `README.md`

If `PATCH_REQUEST.md` exists, read it first and treat it as the current patch scope.

## Git Authority Model

Separate human authority from agent authority.

### Human-only operations

The following are reserved for the repository owner or a human maintainer:

- pushing directly to `main`
- committing directly on `main`
- merging pull requests
- approving pull requests as final authority
- creating, editing, or deleting repository secrets
- creating, rotating, or exposing deploy tokens
- changing GitHub Actions deployment settings
- changing branch protection or repository rules
- changing repository visibility, collaborators, or permissions
- running production deployments outside GitHub Actions
- force-pushing shared branches
- deleting remote branches unless explicitly requested
- rewriting shared Git history unless explicitly requested

### Agent-allowed operations

An agent may perform these operations only on a non-main working branch:

- inspect repository state with read-only Git commands
- create a feature, fix, docs, chore, or refactor branch
- edit files within approved scope
- run verification commands
- commit scoped changes when requested or allowed
- push a non-main branch only when explicitly allowed
- create a pull request only when explicitly allowed

### Agent-forbidden operations

An agent must not:

- push to `main`
- commit directly on `main`
- merge pull requests
- force-push unless explicitly requested for history repair
- modify remotes without explicit approval
- store secrets in files, logs, commits, issues, pull requests, or generated documentation
- run production deployment commands locally
- bypass checks, hooks, reviews, branch rules, or documented gates
- expand scope beyond the current task
- change repository permissions, tokens, secrets, deploy settings, or branch protection

## Git Start Gate

Before changing files, run:

```bash
git status -sb
git branch --show-current
```

If the current branch is `main`, stop and create a non-main branch before editing.

Recommended branch names:

```txt
docs/<short-description>
feat/<short-description>
fix/<short-description>
chore/<short-description>
refactor/<short-description>
```

Do not work directly on `main`.

## Scope Gate

Before editing, identify the approved scope from:

1. `PATCH_REQUEST.md`, if present
2. `SPEC.md`
3. the user's current instruction
4. `AGENT.md`
5. `README.md`
6. `TODO.md`

`TODO.md` is deferred work. It is not current scope unless promoted by `SPEC.md` or `PATCH_REQUEST.md`.

Reject or defer work that is outside scope.

## Change Discipline

Keep changes small and reviewable.

Do not:

- mix unrelated tasks
- perform broad refactors without explicit scope
- rewrite files unnecessarily
- change formatting across unrelated files
- delete files outside the approved scope
- modify generated files unless required
- revert user changes without explicit reason

## Verification Gate

Before claiming completion, run available verification commands.

Common commands:

```bash
git diff --check
```

Project-specific commands may include:

```bash
pnpm run format
pnpm run lint
pnpm run typecheck
pnpm run build
pnpm test
go test ./...
go vet ./...
```

Run commands that apply to the current project.

If a command does not exist, report that clearly.

If a command fails, report:

- command
- failure summary
- likely cause
- whether it appears caused by the current change

Do not hide failing checks.

## Commit Gate

Before committing, run:

```bash
git status -sb
git diff --check
```

Then review the diff.

Confirm:

- the branch is not `main`
- only intended files changed
- no secrets or credentials appear in the diff
- documentation matches actual behavior
- verification has been run or explicitly reported as unavailable

Do not commit if:

- the branch is `main`
- required checks fail because of the current change
- unrelated unresolved changes are present
- the scope is ambiguous enough that implementation would be guesswork
- secrets, tokens, credentials, or private configuration appear in the diff

## Push Gate

Do not push unless explicitly requested or clearly allowed by the user's workflow.

If pushing is allowed, push only the current non-main branch:

```bash
git push -u origin <current-branch>
```

Never push to `main`.

## Pull Request Gate

Create a pull request only when explicitly requested or clearly expected.

The pull request must target `main`.

A pull request body should include:

- summary
- files changed
- verification commands and results
- risks or gaps
- whether deployment is affected

Do not merge the pull request.

## Deployment Gate

Production deployment must be controlled by GitHub Actions from `main`.

Agents must not:

- deploy from a local machine
- deploy from a feature branch
- create deploy credentials
- edit deploy credentials
- print deploy credentials
- bypass GitHub Actions
- weaken CI or deployment gates

If deployment requires secrets, store them only in GitHub Secrets or the approved secret manager.

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

## Completion Report

Final reports must include:

- what changed
- files changed
- verification commands and results
- whether a commit was created
- whether a branch was pushed
- whether a pull request was created
- remaining risks or gaps

Do not claim completion without reporting verification status.

## Failure Handling

If blocked, report:

- blocker
- command or file involved
- current repository state
- safest next action

Do not guess around missing authority, missing secrets, or ambiguous scope.
