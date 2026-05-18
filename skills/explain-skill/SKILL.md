---
name: explain-skill
description: Explain SPEC.md, README.md, source code, errors, logs, or diffs in practical terms for the project owner. Use this when the user wants to understand purpose, important points, risks, inconsistencies, or next inspection targets. Do not translate entire files, modify files, or propose large implementation plans unless explicitly requested.
category: Envblex
---

# Explain Skill

Explain project material so the project owner can make decisions quickly.

## When To Use

Use this skill when the user asks to understand one of these:

- `SPEC.md`, `README.md`, `TODO.md`, `AGENT.md`, or other project docs
- source code, config files, package files, or directory structure
- errors, logs, stack traces, CI output, deploy output, or command output
- diffs, commits, pull requests, or generated changes

## Do Not Use

Do not use this skill when the user explicitly asks you to:

- rewrite, fix, or implement code
- fully translate a file
- create a full plan or specification
- make broad architecture decisions without inspecting the relevant material

## Workflow

1. Read only the files, diff, error, or log needed for the question.
2. State the purpose first.
3. Summarize the important points.
4. Identify risks, confusing parts, contradictions, or missing decisions.
5. Point to the next files, lines, or commands worth checking.
6. If the material is insufficient, say exactly what is missing.

## Output Format

Use this structure by default:

1. **Purpose** — what this material is for.
2. **Important points** — the facts the user needs to know.
3. **Risks / unclear parts** — failure points, ambiguity, or bad assumptions.
4. **Next checks** — concrete files, lines, commands, or questions.

Skip sections that do not apply.

## Style

- Be concise and concrete.
- Prefer practical explanation over full translation.
- Use the user's terminology where it is accurate.
- Explain why each point matters.
- Do not praise the material.
- Do not invent implementation details that are not present.
- Mark guesses as guesses.
- Avoid adding implementation ideas unless the user asks for them.

## Coverage Checklist

### For specs

- what will be built
- what is out of scope
- completion criteria
- unclear, risky, or conflicting requirements

### For README files

- how the project is meant to be used
- assumptions the user must know
- setup or usage steps that are missing
- whether the README matches the implementation, if code is available

### For code

- role of the file or function
- key data flow
- important dependencies
- likely failure points
- security, performance, or maintainability risks when visible

### For errors and logs

- what failed
- likely cause
- where to inspect next
- safest next command or fix direction

### For diffs

- what changed
- behavior impact
- risk level
- files worth reviewing carefully
