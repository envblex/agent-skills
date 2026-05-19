---
name: spec-skill
description: Convert rough Notion-style product or implementation notes into focused project documents. Use when the user provides messy notes, ideas, requirements, or planning text and wants SPEC.md, TODO.md, README.md, and AGENT.md generated for later implementation by an AI agent. Keep v0.1.0 small, define non-goals and completion criteria, and move deferred work into TODO.md. Do not edit application code.
metadata:
  author: Envblex
---

# Spec Skill

Convert rough notes into a small, implementable `v0.1.0` project scope.

## Purpose

Create clear project documents for a later implementation agent.

This skill turns messy planning notes into:

- `SPEC.md`
- `TODO.md`
- `README.md`
- `AGENT.md`

It does not implement code.

Generated AGENT.md must include:

- Package manager: pnpm only
- Language: TypeScript only
- Runtime/framework constraints
- Forbidden tools: npm, yarn, bun unless explicitly required
- Deployment policy
- Branch policy

## Workflow

1. Read the user's notes.
2. Identify:
   - intended user
   - problem to solve
   - smallest useful version
   - explicit constraints
   - unclear requirements
3. Define the current scope as `v0.1.0`.
4. Cut anything uncertain, large, optional, or nice-to-have from `v0.1.0`.
5. Write or update only:
   - `SPEC.md`
   - `TODO.md`
   - `README.md`
   - `AGENT.md`
6. Do not edit application code.
7. Do not implement TODO/Later items.
8. Do not invent requirements that are not supported by the notes.

## Scope Rule

`v0.1.0` must be small enough for one focused implementation pass.

Prefer a boring complete version over a broad incomplete one.

Move these to `TODO.md`:

- integrations not required for first use
- automation that can be manual for now
- optional polish
- unclear or speculative ideas
- large architectural changes
- multi-step workflows that can be split later
- features that require credentials, external services, or deployment unless explicitly required

## SPEC.md

`SPEC.md` is the implementation source of truth.

Include:

```md
# SPEC

## Purpose

## Background

## Current Scope: v0.1.0

## Non-Goals

## User Stories / Usage Flow

## Functional Requirements

## Output Files / Interfaces

## Constraints

## Completion Criteria

## Test / Check Expectations
````

### SPEC Rules

Write requirements as concrete behavior.

Use:

* `must`
* `must not`
* `should` only when the behavior is recommended but not required

Avoid vague requirements such as:

* make it better
* user-friendly
* clean implementation
* flexible design
* good UX

Translate vague notes into observable behavior.

## TODO.md

`TODO.md` is for deferred work only.

Include:

```md
# TODO

## Later

## Open Questions

## Rejected For Now
```

### TODO Rules

`Later` items are not part of `v0.1.0`.

Do not write `TODO.md` as an implementation checklist for the current pass.

Use `Open Questions` for requirements that cannot be implemented safely without more information.

Use `Rejected For Now` for ideas intentionally excluded from the first version.

## README.md

`README.md` is for the human user.

Include:

```md
# <Project Name>

## What This Is

## Problem It Solves

## Current Scope

## How To Use

## Important Files

## Out Of Scope For Now
```

### README Rules

The README must describe only the planned `v0.1.0` behavior.

Do not document TODO/Later items as if they exist.

Do not include internal agent reasoning unless it helps the user operate the project.

## AGENT.md

`AGENT.md` defines working rules for future agents.

Include:

```md
# AGENT

## Required Reading

Before working, read:

- SPEC.md
- TODO.md
- README.md
- AGENT.md

## Rules

- Implement only the current scope in SPEC.md.
- Treat TODO.md as deferred work, not current work.
- Do not implement Later items unless explicitly requested.
- Keep changes small and reviewable.
- Follow the existing project structure.
- Update README.md only when behavior, commands, setup, or file layout changes.
- Run available build/check/test commands before claiming completion.
- Do not hide failing checks.
- Do not commit unrelated changes.
```

## Completion Criteria Rules

Completion criteria must be observable.

Good criteria include:

* specific files generated
* specific behavior implemented
* specific command passes
* specific interface exists
* specific invalid behavior is rejected
* README matches actual behavior

Bad criteria include:

* implementation is clean
* UX is good
* code is flexible
* project is complete

## Handling Messy Notes

When the notes contain multiple projects, choose one coherent project and move the rest to `TODO.md`.

When the notes contain multiple independent features, choose the smallest useful feature set for `v0.1.0`.

When the notes are ambiguous, do not guess. Put the ambiguity in `TODO.md` under `Open Questions`.

When the user gives implementation preferences, preserve them as constraints only if they affect the current scope.

## Prohibited Actions

Do not:

* edit application code
* implement the project
* add features not supported by the notes
* treat TODO/Later items as current work
* make the first version large
* include speculative architecture as required scope
* write completion criteria that cannot be checked
* claim the project is implemented

## Final Response

Report:

* which files were created or updated
* the `v0.1.0` scope summary
* what was moved to `TODO.md`
* unresolved open questions
* any scope risks
