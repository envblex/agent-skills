mkdir -p skills/bootstrap-skill

cat > skills/bootstrap-skill/SKILL.md <<'EOF'
---
name: bootstrap-skill
description: Initialize or normalize a repository before implementation by establishing Git baseline, project documents, task structure, scripts, conventions, and repo-specific operating rules.
---

# Bootstrap Skill

Initialize or normalize a repository before implementation.

This skill prepares a repository so that later implementation, review, and
operations work can proceed safely and predictably. It is not an implementation
skill. It must create or normalize the repository baseline, documentation,
branch model, task structure, and development conventions without expanding the
approved initial scope.

## Core Responsibility

The bootstrap step must produce a repository that has:

- a canonical Git branch model
- clear project scope
- initial project documents
- task tracking
- development and verification commands
- security and secret-handling rules
- deployment boundaries
- agent operating rules
- enough structure for an implementation skill to continue safely

Bootstrap is complete only when a future agent can read the repository and know:

- what the project is
- what v0.1.0 includes
- what v0.1.0 excludes
- how to install dependencies
- how to run checks
- how to run tests
- how to build
- how to create a safe pull request
- what must not be touched

## Non-Goals

Do not implement product features during bootstrap.

Do not silently create production infrastructure.

Do not deploy.

Do not add secrets to the repository.

Do not push directly to `main` after the initial repository baseline exists.

Do not invent unsupported stack templates.

Do not create or depend on a repository-root `templates/` directory.

Do not expand the project beyond the user's approved initial scope.

## Priority Order

When information conflicts, use this priority order:

1. explicit user instructions
2. existing repository files
3. project notes supplied by the user
4. stack-specific template guidance
5. base template guidance
6. conservative defaults

If existing repository content conflicts with templates, preserve the existing
content unless it is clearly broken, unsafe, or inconsistent with the approved
scope.

## Template Location

All templates are stored under this skill directory:

    skills/bootstrap-skill/templates/

When this skill refers to `templates/...`, resolve that path relative to:

    skills/bootstrap-skill/

Do not resolve template paths relative to the repository root.

Do not recreate a repository-root `templates/` directory.

Supported template directories are:

- `templates/base/`
- `templates/nodejs-api/`
- `templates/nodejs-fullstack-webapp/`
- `templates/chrome-extension/`
- `templates/go-cli/`

Unsupported project types must use `templates/base/` only, plus conservative
manual guidance in the generated documents.

## Expected Template Contents

The base template should provide or inform these files:

- `README.md`
- `SPEC.md`
- `TODO.md`
- `AGENT.md`
- `.gitignore`
- `.env.example`
- `docs/ops.md`
- `docs/deploy.md`

Stack templates may provide additional guidance for:

- package manager
- runtime
- build commands
- test commands
- lint/typecheck commands
- directory layout
- deployment assumptions
- stack-specific risks

Stack templates must not override user scope.

## Project Type Detection

Infer the project type from user notes and repository files.

Use these mappings:

- Node.js API:
  - API server
  - REST API
  - Hono
  - Express
  - Fastify
  - Cloudflare Worker API
  - backend-only TypeScript service

- Node.js fullstack web app:
  - React
  - Astro
  - Next.js
  - Remix
  - React Router
  - fullstack TypeScript web app

- Chrome extension:
  - manifest.json
  - browser extension
  - Chrome extension
  - content script
  - background script

- Go CLI:
  - Go command line tool
  - `go.mod`
  - Cobra
  - urfave/cli
  - single binary CLI

If detection is ambiguous, use `templates/base/` and write the ambiguity into
`SPEC.md` and `TODO.md`.

Do not ask for clarification unless bootstrap cannot proceed without it.

## Git Branch Baseline

The canonical default branch is `main`.

Before editing files, inspect the Git state:

    git status -sb
    git branch -a
    git remote -v
    git ls-remote --heads origin

If the repository is not initialized, initialize it with `main`:

    git init -b main

If the repository has a remote `master` but no remote `main`, migrate to `main`
before continuing.

Recommended sequence:

    git fetch origin
    git switch master
    git pull origin master
    git branch -m master main
    git push -u origin main

Then the repository default branch must be changed to `main` on GitHub. If the
GitHub CLI is available:

    gh repo edit --default-branch main

After the default branch is changed and verified, the old remote branch may be
deleted:

    git push origin --delete master

If the repository has no remote `main` yet but has the current local bootstrap
branch, creating the first remote `main` is allowed only as repository
initialization.

Required precondition:

    git ls-remote --heads origin

Only if remote `main` is absent, create the initial remote `main`:

    git push -u origin HEAD:main

If continued work should remain on the current feature branch, push it
separately:

    git push -u origin HEAD

After remote `main` exists, never push implementation work directly to `main`.

All future implementation work must use:

    git switch -c <feature-branch>
    git push -u origin <feature-branch>
    gh pr create --base main --head <feature-branch>

## Branch Rules

Use short, descriptive branch names.

Allowed branch prefixes:

- `chore/`
- `docs/`
- `feat/`
- `fix/`
- `refactor/`
- `ops/`

Bootstrap work should normally use:

    chore/bootstrap-repo

or:

    docs/bootstrap-repo

Do not work directly on `main` unless creating the initial repository baseline.

Do not force-push shared branches unless the user explicitly asks and the risk is
documented.

## Main Branch Protection Requirements

Record these requirements in `docs/ops.md`:

- `main` is the canonical default branch.
- Direct pushes to `main` are forbidden after initial bootstrap.
- Changes enter `main` through pull requests.
- CI must pass before merge.
- Deployment, if any, must run only from GitHub Actions on `main`.
- Secrets must be stored in GitHub Secrets or the hosting provider's secret
  manager.
- Secrets must never be committed.
- Local `.env` files must remain ignored.

If branch protection cannot be configured by the agent, write the required manual
steps into `TODO.md`.

## Repository Inspection

Before creating or overwriting files, inspect:

    find . -maxdepth 3 -type f | sort
    git status -sb

If the repository already has package metadata, inspect it:

    cat package.json

If the repository already has Go metadata, inspect it:

    cat go.mod

If files exist, merge carefully. Do not blindly overwrite meaningful existing
content.

Safe overwrite candidates:

- empty placeholder files
- generated files from a previous failed bootstrap
- files that clearly contradict the current approved scope

Unsafe overwrite candidates:

- user-authored specifications
- existing source code
- existing CI workflows
- existing deployment configuration
- existing security documentation
- existing license files

When in doubt, preserve content and append a clearly marked section.

## Required Output Files

Bootstrap must ensure these files exist.

### `README.md`

Must contain:

- project name
- short description
- current status
- intended users
- v0.1.0 scope summary
- install command
- development command
- check/test/build commands
- deployment status
- repository workflow summary

The README must not claim features are implemented unless they exist.

### `SPEC.md`

Must contain:

- project goal
- target users
- problem statement
- v0.1.0 scope
- explicit non-goals
- functional requirements
- non-functional requirements
- security requirements
- data model or state assumptions, if applicable
- error handling expectations
- acceptance criteria

The spec must be narrow enough that implementation can finish without guessing.

### `TODO.md`

Must contain task sections:

- Bootstrap
- Design
- Implementation
- Tests
- Documentation
- Operations
- Release readiness
- Out of scope

Each task must be written as a concrete checkbox.

Do not create vague tasks like "improve app" or "make better".

### `AGENT.md`

Must contain operating rules for future agents:

- read `SPEC.md`, `TODO.md`, `README.md`, and `docs/ops.md` first
- do not push to `main`
- do not deploy manually
- do not commit secrets
- do not expand v0.1.0 scope
- run required checks before reporting completion
- leave a clean Git state or explicitly report remaining changes
- prefer small, reviewable commits
- preserve user-authored decisions

### `.gitignore`

Must ignore common local and secret files.

Minimum entries:

    node_modules/
    dist/
    build/
    coverage/
    .env
    .env.*
    !.env.example
    .DS_Store
    *.log

For Go projects, include:

    bin/
    *.test
    *.out

For editor and OS noise, include:

    .vscode/
    .idea/
    Thumbs.db

Do not ignore lockfiles.

### `.env.example`

Must list required environment variable names with safe placeholder values.

Do not include real secrets.

If no environment variables are required yet, write:

    # No environment variables are required for local development yet.

### `docs/ops.md`

Must contain:

- branch policy
- commit policy
- PR policy
- CI policy
- secret policy
- local development policy
- verification commands
- deployment boundary
- manual setup checklist
- known operational risks

### `docs/deploy.md`

Must contain:

- current deployment status
- deployment target, if known
- deployment trigger
- required secrets
- forbidden deployment paths
- rollback notes, if applicable
- manual setup steps, if applicable

If deployment is not configured, say so explicitly.

Do not invent a deployment target.

## Package Manager Policy

Prefer existing repository choices.

For Node.js projects:

- if `pnpm-lock.yaml` exists, use `pnpm`
- if `package-lock.json` exists, use `npm`
- if `yarn.lock` exists, use `yarn`
- if no lockfile exists, prefer `pnpm`

Do not create multiple lockfiles.

For Node.js projects, if creating `package.json`, include scripts only when they
are meaningful for the stack.

Recommended baseline scripts:

    "dev": "echo \"No dev command configured yet\"",
    "check": "echo \"No check command configured yet\"",
    "test": "echo \"No tests configured yet\"",
    "build": "echo \"No build command configured yet\""

Replace placeholders with real commands when the stack template provides them.

Do not claim placeholder scripts validate correctness.

## Secret Handling

Secrets must be handled through:

- GitHub Secrets
- hosting provider secret stores
- local ignored `.env` files

Never commit:

- API keys
- tokens
- passwords
- private keys
- session secrets
- production credentials

If a secret is required, document:

- variable name
- purpose
- where it is set
- whether it is required locally
- whether it is required in CI
- whether it is required for deployment

Do not generate real secret values unless the user explicitly requests local-only
development placeholders.

## Deployment Boundary

Bootstrap must not deploy.

Bootstrap may document deployment.

Bootstrap may create deployment documentation.

Bootstrap may create CI workflow documentation.

Bootstrap must not run:

    wrangler deploy
    vercel deploy --prod
    firebase deploy
    netlify deploy --prod
    fly deploy
    railway up

Bootstrap may run non-production validation commands, such as dry runs, only if
the repository already defines them and they do not mutate production state.

## GitHub Actions Policy

If creating or normalizing GitHub Actions documentation, enforce:

- checks run on pull requests
- deployment runs only on pushes to `main`
- deployment requires secrets from GitHub Secrets
- no deployment from feature branches
- no deployment from local machines as the normal path

If workflow files already exist, do not rewrite them unless they contradict the
branch and deployment policy.

If workflow files are missing, document the required workflow in `TODO.md`
instead of inventing stack-specific CI that may not work.

## Scope Control

Bootstrap must preserve a strict v0.1.0 boundary.

Every feature must be classified as one of:

- required for v0.1.0
- optional after v0.1.0
- explicitly out of scope

If user notes contain many ideas, move nonessential ideas into the "Out of
scope" section.

Do not let README, SPEC, and TODO disagree about scope.

## Normalization Rules

When normalizing an existing repository:

- keep existing working code
- keep existing lockfiles
- keep existing license
- keep existing package manager
- keep existing stack unless the user requested migration
- add missing docs
- repair contradictory docs
- remove references to nonexistent root templates
- document unresolved issues in TODO

Do not delete source files during bootstrap.

Do not perform large refactors during bootstrap.

Do not install heavy dependencies just to satisfy a template.

## Workflow

### 1. Preflight

Inspect repository state.

Required commands:

    pwd
    git status -sb
    git branch -a
    git remote -v
    git ls-remote --heads origin

If not a Git repository, initialize with:

    git init -b main

### 2. Normalize Git Baseline

Ensure the repository has or can create a canonical `main` branch.

Apply the Git Branch Baseline rules.

Document any manual GitHub settings in `TODO.md`.

### 3. Read Project Inputs

Read:

- user notes
- existing `README.md`
- existing `SPEC.md`
- existing `TODO.md`
- existing `AGENT.md`
- existing `docs/ops.md`
- existing `docs/deploy.md`
- stack metadata files such as `package.json`, `go.mod`, or `manifest.json`

Infer:

- project name
- project type
- target user
- v0.1.0 goal
- stack
- commands
- deployment assumptions
- required secrets

### 4. Resolve Templates

Use:

    skills/bootstrap-skill/templates/base/

Then optionally merge one supported stack template:

- `skills/bootstrap-skill/templates/nodejs-api/`
- `skills/bootstrap-skill/templates/nodejs-fullstack-webapp/`
- `skills/bootstrap-skill/templates/chrome-extension/`
- `skills/bootstrap-skill/templates/go-cli/`

Never read from:

    templates/

at repository root.

### 5. Create or Merge Documents

Create or normalize:

- `README.md`
- `SPEC.md`
- `TODO.md`
- `AGENT.md`
- `.gitignore`
- `.env.example`
- `docs/ops.md`
- `docs/deploy.md`

Keep the documents mutually consistent.

### 6. Normalize Commands

Document the actual commands available.

For Node.js repositories, inspect `package.json` scripts.

For Go repositories, inspect `go.mod` and expected `go test ./...`.

If commands are not yet available, write that explicitly.

Do not pretend checks exist.

### 7. Run Safe Verification

Run safe verification commands only.

Always run:

    git status -sb

If Node.js dependencies are already installed or install is reasonable, run the
existing check/test/build commands.

If Go is configured, run:

    go test ./...

Do not install large dependencies without need.

Do not deploy.

### 8. Completion Report

At completion, report:

- files created
- files modified
- project type detected
- branch state
- remote `main` state
- commands found
- commands run
- checks passed or skipped
- manual follow-up tasks
- known risks

The final report must be factual. Do not claim success for skipped checks.

## Completion Gate

Bootstrap is not complete until all are true:

- `README.md` exists
- `SPEC.md` exists
- `TODO.md` exists
- `AGENT.md` exists
- `.gitignore` exists
- `.env.example` exists
- `docs/ops.md` exists
- `docs/deploy.md` exists
- Git branch policy is documented
- secret policy is documented
- deployment boundary is documented
- v0.1.0 scope is documented
- non-goals are documented
- verification commands are documented
- `git status -sb` has been checked

If any item is missing, bootstrap must report it as incomplete.

## Failure Handling

If a command fails, do not continue blindly.

Record:

- command
- failure summary
- likely cause
- safe next action

If remote inspection fails because `origin` is missing, continue locally and add
a TODO item to configure `origin`.

If GitHub CLI is unavailable, write manual GitHub steps into `TODO.md`.

If templates are missing, stop and report which template path is missing.

If project type is unsupported, continue with the base template only.

## Required Manual Follow-Up Items

Add TODO items for anything the agent cannot safely complete.

Common manual items:

- create GitHub repository
- set default branch to `main`
- enable branch protection
- add GitHub Secrets
- create Cloudflare resources
- configure deployment target
- verify production domain
- review generated SPEC
- approve v0.1.0 scope

## Quality Bar

Generated documents must be:

- specific
- internally consistent
- narrow in scope
- safe for future agents
- honest about missing pieces
- free of fake implemented-feature claims
- free of real secrets
- free of root-template path assumptions

A future implementation agent should be able to proceed from the generated files
without asking what the repository is supposed to do.
EOF
