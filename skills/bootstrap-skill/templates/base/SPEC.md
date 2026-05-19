# SPEC

## Project

This repository contains Envblex Skills: a local collection of AI-agent workflow skills for envblex projects.

## Purpose

Envblex Skills defines repeatable workflows for:

- repository bootstrap
- specification writing
- implementation
- review
- refinement
- explanation
- operations

The goal is to make agent work explicit, auditable, and constrained by project documents.

## Current Scope

The current approved scope is:

1. Maintain reusable skill instructions under `skills/`.
2. Maintain base and stack-specific templates for generated projects.
3. Define strict Git and deployment authority boundaries for agents.
4. Keep installation and usage instructions documented in `README.md`.
5. Ensure generated project templates include clear rules for future agents.

## Out of Scope

The following are not part of the current scope:

- building a package manager
- building a hosted skill registry
- building a deployment system
- replacing GitHub permissions, token restrictions, or branch protection
- adding secrets or credentials to this repository
- granting agents authority to push to `main`
- granting agents authority to merge pull requests
- granting agents authority to change repository settings

## Repository Model

Skills are plain files stored in this repository.

Each skill should have:

```txt
skills/<skill-name>/SKILL.md
```

A skill may also include agent-specific adapter files:

```txt
skills/<skill-name>/agents/
```

`SKILL.md` is the canonical source of truth. Adapter files must not weaken the rules in `SKILL.md`.

## Template Model

Generated project templates should provide at least:

```txt
SPEC.md
TODO.md
README.md
AGENT.md
```

These files have separate roles:

- `SPEC.md` defines approved current scope.
- `TODO.md` tracks deferred work.
- `README.md` explains project usage.
- `AGENT.md` defines repository rules for future agents.

## Git Authority Model

The project must preserve a strict authority boundary between humans and agents.

Agents may:

- inspect repository state
- create non-main branches
- edit files within approved scope
- run verification commands
- commit scoped changes
- push non-main branches only when explicitly allowed
- create pull requests only when explicitly allowed

Agents must not:

- push to `main`
- commit directly on `main`
- merge pull requests
- edit secrets
- create or rotate deploy tokens
- change repository permissions
- change branch protection or repository rules
- run production deployments locally
- bypass checks or documented gates

## Deployment Model

Production deployment must be controlled by GitHub Actions from `main`.

Agents must not deploy from:

- local machines
- feature branches
- manually created credentials
- bypassed CI paths

Secrets must be managed through GitHub Secrets or an approved secret manager.

## Documentation Requirements

Documentation must distinguish between:

- implemented behavior
- planned behavior
- deferred work
- operational policy
- local development instructions

Do not document unimplemented features as available behavior.

## Verification Requirements

Before claiming completion, run at minimum:

```bash
git status -sb
git diff --check
```

If scripts are added later, document and run the relevant commands, such as:

```bash
pnpm run format
pnpm run lint
pnpm run typecheck
pnpm run build
pnpm test
```

## Completion Criteria

A change is complete only when:

- the change matches this specification
- the diff contains only intended files
- documentation matches actual behavior
- no secrets or credentials are introduced
- verification commands have been run and reported
- work is committed on a non-main branch, if a commit is requested
