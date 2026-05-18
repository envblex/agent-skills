---
name: review-skill
description: Review an implementation against SPEC.md, TODO.md, README.md, and AGENT.md. Use when the user wants an AI agent to verify that code matches the approved current scope, avoids TODO/Later work, keeps README.md accurate, and passes available build/check/test commands. Report findings only. Do not edit files or propose new features.
category: Envblex
---

# Review Skill

Review an implementation for scope conformance, regressions, documentation accuracy, and verification status.

## Purpose

Check whether the implementation correctly satisfies the approved current scope without expanding it.

This skill is for review only. It must not edit files, implement fixes, or introduce new requirements.

## Required Review Checks

Reject or flag the implementation if:

- It uses npm/yarn/bun in a pnpm project.
- It creates package-lock.json, yarn.lock, or bun.lockb.
- It adds JavaScript files where TypeScript should be used.
- It ignores existing tsconfig/package.json conventions.
- It bypasses AGENT.md rules.

## Workflow

1. Read the project documents:
   - `SPEC.md`
   - `TODO.md`
   - `README.md`
   - `AGENT.md`
   - `PATCH_REQUEST.md`, if present
2. Inspect the implementation diff.
3. Inspect relevant source files when the diff alone is not enough.
4. Identify the approved current scope.
5. Check whether the implementation satisfies the required behavior.
6. Check whether any out-of-scope TODO/Later item was implemented.
7. Check whether `README.md` matches the actual implemented behavior.
8. Run available verification commands when possible.
9. Report findings only.
10. Do not edit files unless the user explicitly asks.

## Source of Truth

Use documents in this priority order:

1. `PATCH_REQUEST.md`, if the review is for a patch
2. `SPEC.md`
3. `AGENT.md`
4. `README.md`
5. `TODO.md`

`TODO.md` is not an implementation source unless another approved document explicitly includes an item from it.

## Review Priorities

Report issues in this order:

1. Behavior that violates `SPEC.md` or `PATCH_REQUEST.md`
2. Missing completion criteria
3. Regressions in existing behavior
4. Extra features outside approved scope
5. README inaccuracies
6. Failing verification commands
7. Missing verification commands
8. Maintainability risks that directly affect the approved scope

Do not report these as findings:

- new feature ideas
- optional polish
- broad redesigns
- TODO/Later items not included in the approved scope
- personal style preferences that do not affect correctness or maintainability

## Finding Format

Lead with findings.

Each finding must include:

```md
## Findings

### <Severity>: <Short title>

- Location: `<file>:<line>` or `Unknown`
- Requirement: `<document/section>` or `Unknown`
- Problem: <what is wrong>
- Impact: <why it matters>
- Fix direction: <minimal practical fix>
````

Use these severities:

* `Blocker`: prevents the approved scope from working or makes verification impossible
* `High`: violates a required behavior or completion criterion
* `Medium`: causes incorrect docs, partial behavior, or meaningful regression risk
* `Low`: maintainability or clarity issue that affects the current scope

## No-Finding Format

If there are no findings, say so directly:

```md
## Findings

No findings.

## Verification

- `<command>`: passed

## Remaining Gaps

- <gap, if any>
```

Do not say the implementation is fully verified if checks were not run.

## Verification Rules

Run the project's available commands when possible, such as:

* install/dependency check
* format
* lint
* typecheck
* build
* test

For each command, report:

* command
* result
* relevant failure summary, if failed

If a command cannot be run, state the exact reason.

Valid reasons include:

* command is not defined
* dependency installation is unavailable
* required environment variable is missing
* external service is unavailable
* repository state prevents safe execution

Do not claim verification passed unless the command was actually run and passed.

## Scope Control

Check for scope expansion.

Flag implementation as out of scope when it:

* implements TODO/Later work not approved by `SPEC.md` or `PATCH_REQUEST.md`
* adds unrelated features
* performs broad refactors not required for the current change
* changes public behavior beyond the approved scope
* updates README.md to document unimplemented behavior

Do not suggest out-of-scope work as a fix.

Fix directions must be minimal and limited to satisfying the approved scope.

## Prohibited Actions

Do not:

* edit files
* implement fixes
* commit changes
* propose new features
* expand the scope
* rewrite architecture unless required by the approved scope
* treat `TODO.md` as required work
* approve an implementation without checking relevant files
* hide failing checks
* claim verification without running commands

## Final Response

Report:

* findings, if any
* verification commands and results
* README accuracy status
* scope-control status
* remaining risks or gaps
