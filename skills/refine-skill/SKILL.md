---
name: refine-skill
description: Convert rough fix notes into PATCH_REQUEST.md. Use when the user provides messy change requests, bug notes, review notes, or desired adjustments and wants a focused one-commit patch instruction. The output must define required changes, non-goals, affected files, completion criteria, and verification steps. Do not edit application code.
---

# Refine Skill

Convert rough fix notes into a focused `PATCH_REQUEST.md` for a later implementation pass.

## Purpose

Clarify what should be fixed next without editing code.

This skill exists to turn messy notes into a small, reviewable patch request that another implementation agent can execute in one commit.

## Workflow

1. Read the user's fix notes.
2. Read existing project documents when available:
   - `SPEC.md`
   - `TODO.md`
   - `README.md`
   - `AGENT.md`
   - existing `PATCH_REQUEST.md`
3. Identify the actual required change.
4. Reduce the request to the smallest coherent patch.
5. Write or update `PATCH_REQUEST.md`.
6. Do not edit application code.
7. Do not update `SPEC.md`, `README.md`, or `TODO.md` unless the user explicitly asks.

## PATCH_REQUEST.md Format

`PATCH_REQUEST.md` must include:

```md
# PATCH_REQUEST

## Summary

## Background

## Required Changes

## Non-Goals

## Affected Files

## Completion Criteria

## Verification

## Notes for Implementation Agent
````

## Section Rules

### Summary

State the patch goal in 1-3 sentences.

### Background

Explain why the patch is needed.

Do not add unrelated context.

### Required Changes

List only changes required for this patch.

Each item must be concrete enough for an implementation agent to act on.

### Non-Goals

List anything that should not be done in this patch, including:

* unrelated improvements
* broad refactors
* optional polish
* new features
* speculative ideas
* changes that should be separate commits

### Affected Files

List files or directories likely to change.

Use `Unknown` only when the project structure has not been inspected.

### Completion Criteria

Define observable criteria that prove the patch is complete.

Good criteria include:

* specific behavior changes
* specific files updated
* specific command outputs
* specific tests passing
* specific documentation updates when needed

Do not use vague criteria such as:

* make it better
* clean up the code
* improve UX
* fix everything

Translate vague notes into concrete checks.

### Verification

List commands the implementation agent should run.

If no commands are known, write:

```md
No automated verification command is known. The implementation agent must inspect the project and run the available check/build/test commands.
```

### Notes for Implementation Agent

Include constraints, risks, edge cases, and warnings.

Do not include implementation details unless they are necessary to prevent mistakes.

## Scope Control

When the rough notes contain multiple independent fixes:

1. Choose the smallest coherent patch.
2. Put the rest in `Non-Goals`.
3. Do not merge unrelated fixes into one patch request.

When the rough notes are ambiguous:

1. Preserve only the part that can be stated concretely.
2. Mark unclear parts as non-goals or implementation notes.
3. Do not invent requirements.

## Prohibited Actions

Do not:

* edit application code
* implement the patch
* update `SPEC.md` unless explicitly requested
* expand the patch into a redesign
* include future TODO items as required changes
* claim a file is affected without checking or clearly marking it as expected
* write requirements that cannot be verified

## Final Response

Report:

* whether `PATCH_REQUEST.md` was created or updated
* the patch summary
* what was intentionally left out of scope
* any ambiguity that remains
