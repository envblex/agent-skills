---
name: bootstrap-skill
description: Initialize or normalize a repository by creating project documents, task structure, scripts, and development conventions before implementation.
---

# Bootstrap Skill

Use this skill to initialize or normalize a repository before implementation.

This skill creates or updates project documents and applies base or stack-specific template guidance.

It does not implement product features unless the approved scope explicitly says that bootstrapping includes minimal project files.

## Core Principle

Bootstrap establishes project structure and rules. It must not silently expand into implementation.

## Template Location

All templates are stored under this skill directory:

```txt
skills/bootstrap-skill/templates/
```

When this skill refers to `templates/...`, resolve that path relative to:

```txt
skills/bootstrap-skill/
```

Do not depend on or recreate a repository-root `templates/` directory.

## Supported Templates

Base template:

```txt
templates/base/
```

Stack-specific templates:

```txt
templates/go-cli/
templates/nodejs-api/
templates/nodejs-fullstack-webapp/
templates/chrome-extension/
```

The base template provides common project documents.

Stack-specific templates extend the base template. They must not weaken the base rules.

## Generated Project Documents

The base bootstrap output should include:

```txt
SPEC.md
TODO.md
README.md
AGENT.md
```

Roles:

- `SPEC.md` defines approved current scope.
- `TODO.md` tracks deferred work.
- `README.md` explains setup, usage, verification, and limitations.
- `AGENT.md` defines repository rules for future agents.

## Required Reading

Before bootstrapping, inspect:

- existing repository files
- existing `README.md`, if present
- existing `SPEC.md`, if present
- existing `TODO.md`, if present
- existing `AGENT.md`, if present
- user-provided project notes
- user-selected project type, if provided

If `PATCH_REQUEST.md` exists, read it first and treat it as the current patch scope.

## Project Type Selection

Select the closest supported template.

Use:

- `go-cli` for Go command-line applications
- `nodejs-api` for Node.js TypeScript APIs
- `nodejs-fullstack-webapp` for Node.js TypeScript fullstack web applications
- `chrome-extension` for Chrome extensions
- `base` when no stack-specific template is appropriate

If the project type is ambiguous, use `base` and keep stack-specific assumptions out.

Do not invent unsupported templates.

## Bootstrap Workflow

1. Check repository state.
2. Identify whether this is a new repository or an existing repository.
3. Identify project type.
4. Apply `templates/base/`.
5. Apply one stack-specific template if appropriate.
6. Preserve user content where possible.
7. Avoid overwriting meaningful existing documents without review.
8. Keep generated documents focused on the approved initial scope.
9. Run verification checks available for documentation-only changes.
10. Report changed files and remaining gaps.

## Git Start Gate

Before changing files, run:

```bash
git status -sb
git branch --show-current
```

If the current branch is `main`, stop and create a non-main branch before editing.

Do not bootstrap directly on `main`.

## Merge Rules

When applying templates:

- keep base rules unless there is a stronger stack-specific rule
- stack-specific templates may add constraints
- stack-specific templates must not remove base safety rules
- do not weaken Git, secret, deployment, or verification rules
- do not document unimplemented behavior as available
- do not treat `TODO.md` as current scope

If a file already exists:

- preserve project-specific facts
- replace empty placeholder text when appropriate
- avoid deleting useful existing documentation
- avoid broad rewrites unrelated to bootstrap scope

## Scope Rules

Bootstrap may create or update:

- `SPEC.md`
- `TODO.md`
- `README.md`
- `AGENT.md`
- minimal stack-specific project metadata only when explicitly required

Bootstrap must not add by default:

- authentication
- database integration
- deployment automation
- CI secrets
- production credentials
- broad framework architecture
- optional features
- analytics or telemetry
- generated code unrelated to initial scope

## Documentation Rules

`SPEC.md` must define:

- project type
- purpose
- current scope
- out-of-scope items
- expected layout
- verification requirements
- completion criteria

`TODO.md` must define:

- deferred work
- items not planned by default

`README.md` must define:

- current scope reference
- setup
- development commands
- verification commands
- usage
- configuration
- limitations

`AGENT.md` must define:

- required reading
- package manager rules
- language rules
- source-of-truth order
- Git authority model
- verification rules
- deployment rules
- final response requirements

## Package Manager Defaults

For Node.js projects, use pnpm only.

Forbidden:

- `npm install`
- `yarn`
- `bun install`
- `package-lock.json`
- `yarn.lock`
- `bun.lockb`

For Go projects, use Go modules.

Do not vendor dependencies unless explicitly required.

## Security Rules

Do not create or store secrets.

Do not write real secrets into:

- `.env`
- `.env.example`
- README files
- issue bodies
- pull request bodies
- commit messages
- logs
- generated documentation

`.env.example` may contain variable names only.

## Deployment Rules

Do not add deployment behavior unless it is in scope.

Default deployment policy:

- production deployment should run through GitHub Actions from `main`
- local production deployment is forbidden for agents
- secrets belong in GitHub Secrets or the approved secret manager
- agents must not create or rotate deploy credentials

## Verification

For documentation-only bootstrap changes, run:

```bash
git diff --check
git status -sb
```

If project-specific scripts exist, run relevant checks.

Examples:

```bash
pnpm run format
pnpm run lint
pnpm run typecheck
pnpm run build
pnpm test
go test ./...
go vet ./...
```

If no automated verification exists, state that clearly.

## Completion Criteria

Bootstrap is complete only when:

- generated documents exist
- generated documents match the selected project type
- current scope is explicit
- deferred work is separated into `TODO.md`
- agent rules are explicit
- no secrets are introduced
- no unimplemented features are documented as implemented
- verification status is reported
- changes are on a non-main branch

## Final Response

Report:

- selected template
- files created or changed
- important assumptions
- verification commands and results
- whether a commit was created
- remaining gaps or follow-up work
