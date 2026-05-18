---
name: ops-skill
description: Prepare and review repository operations including GitHub Actions, GitHub Secrets, Cloudflare deployment, branch rules, environment variables, and release procedures.
---

# ops-skill

Use this skill for infrastructure, deployment configuration, Cloudflare resource planning, GitHub Actions deployment setup, and secret documentation.

This skill must not implement product features.

## Core policy

This project uses a protected production workflow.

Secrets are managed only through GitHub Secrets.

Production deployments are allowed only through GitHub Actions triggered from `main`.

Direct pushes to `main` are forbidden.

Agents prepare deployability. GitHub Actions performs deployment. Humans control secrets and external resource creation.

## Purpose

Safely prepare infrastructure without bypassing the repository's branch, secret, and deployment rules.

This skill exists because infrastructure changes are higher risk than normal code changes. Infrastructure mistakes can affect production data, billing, domains, deployment credentials, and user data.

## Scope

Allowed work:

* inspect infrastructure configuration
* inspect Cloudflare Workers/Wrangler configuration
* inspect GitHub Actions workflows
* document required Cloudflare resources
* document required GitHub Secrets
* create resource creation plans
* update `wrangler.toml`, `wrangler.json`, or `wrangler.jsonc`
* update `.env.example` with secret names only
* update `.github/workflows/*.yml`
* update `docs/deploy.md`
* update `docs/ops.md`
* update `TODO.md`
* prepare a PR-ready branch
* validate configuration locally when safe
* commit changes on a feature branch only

Forbidden work:

* do not implement product features
* do not deploy production from a local machine
* do not run `wrangler deploy` for production
* do not push directly to `main`
* do not commit directly on `main`
* do not force push `main`
* do not commit secrets
* do not print secret values
* do not read or request secret values
* do not use `wrangler secret put` for production setup
* do not store production secrets in `.env`
* do not store production secrets in `.dev.vars`
* do not create Cloudflare resources without explicit approval
* do not delete Cloudflare resources without explicit approval
* do not bypass commit signing

## Required approval phrases

The following exact phrases are required before high-risk operations.

APPROVE_CLOUDFLARE_RESOURCE_CREATE
APPROVE_CLOUDFLARE_RESOURCE_DELETE
APPROVE_GITHUB_SECRET_CHANGE
APPROVE_GITHUB_ACTIONS_DEPLOY_SETUP
APPROVE_DOMAIN_CHANGE
APPROVE_FORCE_PUSH

Important:

Even with approval, production deployment must not be run locally. Production deployment must happen only through GitHub Actions after changes reach `main`.

## Branch policy

* never work directly on `main`
* never push directly to `main`
* always create or use a feature branch
* all production-bound changes must be merged into `main` through a pull request
* GitHub Actions may deploy only from `main`

Before editing, run:

```
git status --short
git branch --show-current
```

If the current branch is `main`, create a feature branch before changing files:

```
git switch -c ops/<short-task-name>
```

## Secret policy

All production and deployment secrets must be stored in GitHub Secrets.

Allowed:

* document required secret names
* update `.env.example` with secret names only
* update GitHub Actions workflows to read `${{ secrets.NAME }}`
* document manual GitHub Secret setup steps
* verify that workflows reference secret names correctly

Forbidden:

* do not print secret values
* do not commit secret values
* do not ask the user to paste secret values into chat
* do not store production secrets in `.env`
* do not store production secrets in `.dev.vars`
* do not run `wrangler secret put` for normal production setup
* do not place secrets in `wrangler.toml`, `wrangler.json`, or `wrangler.jsonc`

Required GitHub Secrets for Cloudflare Workers deployment usually include:

* `CLOUDFLARE_API_TOKEN`
* `CLOUDFLARE_ACCOUNT_ID`

Project-specific secrets may include:

* `SESSION_SECRET`
* `TURNSTILE_SECRET_KEY`
* `RESEND_API_KEY`
* `GITHUB_CLIENT_SECRET`
* `OAUTH_CLIENT_SECRET`

Only list names. Never expose values.

## Deployment policy

Production deployment is only allowed through GitHub Actions.

Allowed production path:

```
feature branch
-> pull request
-> review
-> merge into main
-> GitHub Actions deploy from main
```

Forbidden:

* local production deploy
* production deploy from feature branch
* manual `wrangler deploy` to production
* direct push to `main`
* force push to `main`

A deploy workflow must be triggered only by `push` to `main` unless the user explicitly requests a staging workflow.

Recommended trigger:

```
on:
  push:
    branches:
      - main
```

## Risk levels

### Low risk

Allowed without explicit approval if done on a feature branch:

* inspect files
* update documentation
* update `.env.example` with secret names only
* add resource planning tables
* add TODO items
* add placeholders for missing resource IDs
* create manual setup instructions

### Medium risk

Requires careful review before commit:

* edit Wrangler config
* edit GitHub Actions workflows
* add Cloudflare bindings
* add environment-specific config
* change deployment scripts
* change route/domain config in repo files

### High risk

Requires exact approval phrase and must be handled manually or through PR-reviewed config:

* create KV namespace
* create R2 bucket
* create D1 database
* add Durable Object migration
* delete any Cloudflare resource
* change production route/custom domain
* change GitHub Actions deployment permissions
* create or rotate GitHub Secrets

## Required inspection

Inspect these files when present:

* `README.md`
* `SPEC.md`
* `TODO.md`
* `AGENT.md`
* `docs/deploy.md`
* `docs/ops.md`
* `.env.example`
* `package.json`
* `pnpm-lock.yaml`
* `package-lock.json`
* `yarn.lock`
* `bun.lockb`
* `wrangler.toml`
* `wrangler.json`
* `wrangler.jsonc`
* `.github/workflows/`
* `src/`
* `app/`
* `worker/`
* `workers/`
* `migrations/`

Run:

```
git status --short
git branch --show-current
git log --show-signature -1
```

If Node.js is used, inspect:

```
cat package.json
```

If Wrangler is available, inspect:

```
pnpm wrangler --version
```

Use the package manager implied by the lockfile:

* `pnpm-lock.yaml` => `pnpm`
* `package-lock.json` => `npm`
* `yarn.lock` => `yarn`
* `bun.lockb` => `bun`

Prefer `pnpm` only when the repository uses pnpm.

## Required documents

Create or update:

* `docs/ops.md`
* `docs/deploy.md`
* `.env.example`
* `TODO.md`

Update when relevant:

* `README.md`
* `SPEC.md`
* `AGENT.md`
* `.github/workflows/deploy.yml`
* `wrangler.toml`
* `wrangler.json`
* `wrangler.jsonc`

## docs/ops.md structure

Use this structure:

# Operations

## Policy

* Secrets are managed through GitHub Secrets only.
* Production deployment is performed only by GitHub Actions from `main`.
* Direct push to `main` is forbidden.
* External resource creation requires human approval.

## Environments

| Environment | Branch                               | Deploy path             | Notes                 |
| ----------- | ------------------------------------ | ----------------------- | --------------------- |
| local       | any                                  | local dev only          | no production secrets |
| staging     | feature/staging branch if configured | optional GitHub Actions | optional              |
| production  | main                                 | GitHub Actions only     | protected             |

## GitHub Secrets

| Name | Environment | Purpose | Required | Set manually |
| ---- | ----------- | ------- | -------- | ------------ |

## Cloudflare resources

### KV namespaces

| Binding | Environment | Namespace name | Namespace ID | Purpose | Status |
| ------- | ----------- | -------------- | ------------ | ------- | ------ |

### R2 buckets

| Binding | Environment | Bucket name | Purpose | Public | Status |
| ------- | ----------- | ----------- | ------- | ------ | ------ |

### D1 databases

| Binding | Environment | Database name | Database ID | Migrations dir | Status |
| ------- | ----------- | ------------- | ----------- | -------------- | ------ |

### Durable Objects

| Binding | Class name | Migration tag | Storage backend | Status |
| ------- | ---------- | ------------- | --------------- | ------ |

## Manual setup steps

## Validation steps

## Rollback notes

## Known risks

## docs/deploy.md structure

Use this structure:

# Deployment

## Policy

Production deploys are only performed by GitHub Actions from `main`.

Direct push to `main` is forbidden.

## Deploy flow

```
feature branch
-> pull request
-> review
-> merge to main
-> GitHub Actions deploy
```

## Required GitHub Secrets

| Name | Purpose |
| ---- | ------- |

## Required Cloudflare resources

| Resource | Binding | Environment | Created |
| -------- | ------- | ----------- | ------- |

## GitHub Actions workflow

## Local validation

## Production deploy

Production deployment is automatic after merge to `main`.

Do not run local production deploy.

## Rollback

## Troubleshooting

## .env.example rules

`.env.example` may contain names only.

Example:

```
SESSION_SECRET=
TURNSTILE_SECRET_KEY=
RESEND_API_KEY=
```

Never commit real values.

## .gitignore rules

Ensure secret files are ignored:

```
.env
.env.*
!.env.example
.dev.vars
```

## Cloudflare resource policy

Cloudflare resources are external state.

Do not create, delete, or mutate them automatically.

Instead:

1. Detect required resources from code and config.
2. Write a resource plan in `docs/ops.md`.
3. Write exact manual commands.
4. Leave ID placeholders where needed.
5. Ask the human to run commands or create resources in the dashboard.
6. After the human provides non-secret IDs/names, update Wrangler config.
7. Commit config and docs on a feature branch.
8. Open or prepare a PR.
9. Production deploy happens only after merge to `main`.

## Manual resource workflow

Use this workflow for KV, R2, D1, and Durable Objects:

```
agent detects required resource
-> agent documents resource plan
-> human approves
-> human creates resource manually
-> human provides non-secret IDs/names
-> agent updates wrangler config
-> agent updates docs
-> agent commits signed patch on feature branch
-> human opens/merges PR
-> GitHub Actions deploys from main
```

Resource IDs are not secrets, but still avoid leaking unnecessary account details.

## Cloudflare KV

Use KV for simple key-value storage, metadata, cache-like state, and abuse reduction.

Do not describe KV as strict, atomic, or strongly consistent for rate limiting.

Before creating KV, document:

| Binding     | Environment | Namespace name               | Namespace ID | Purpose            | Status  |
| ----------- | ----------- | ---------------------------- | ------------ | ------------------ | ------- |
| MEMOS       | production  | `<project>-memos-prod`       | pending      | Store memo records | planned |
| RATE_LIMITS | production  | `<project>-rate-limits-prod` | pending      | Abuse reduction    | planned |

Manual creation command:

```
pnpm wrangler kv namespace create <NAMESPACE_NAME>
```

After creation, update Wrangler config.

Example `wrangler.jsonc`:

```
{
  "kv_namespaces": [
    {
      "binding": "MEMOS",
      "id": "replace_with_actual_namespace_id"
    }
  ]
}
```

If environments are used:

```
{
  "env": {
    "production": {
      "kv_namespaces": [
        {
          "binding": "MEMOS",
          "id": "replace_with_production_namespace_id"
        }
      ]
    },
    "staging": {
      "kv_namespaces": [
        {
          "binding": "MEMOS",
          "id": "replace_with_staging_namespace_id"
        }
      ]
    }
  }
}
```

Rules:

* use separate namespaces for production and staging
* do not point staging to production data
* do not invent namespace IDs
* do not create namespaces without approval
* do not delete namespaces without approval

## Cloudflare R2

Use R2 for file/object storage.

Before creating R2, document:

| Binding | Environment | Bucket name              | Purpose              | Public | Status  |
| ------- | ----------- | ------------------------ | -------------------- | ------ | ------- |
| BUCKET  | production  | `<project>-uploads-prod` | Store uploaded files | no     | planned |

Manual creation command:

```
pnpm wrangler r2 bucket create <BUCKET_NAME>
```

Example Wrangler config:

```
{
  "r2_buckets": [
    {
      "binding": "BUCKET",
      "bucket_name": "replace-with-bucket-name"
    }
  ]
}
```

Rules:

* bucket names must be stable
* use separate buckets for staging and production
* do not make a bucket public unless explicitly required
* document custom domains separately
* do not store secrets in R2

## Cloudflare D1

Use D1 for relational data.

Before creating D1, document:

| Binding | Environment | Database name    | Database ID | Migrations dir | Status  |
| ------- | ----------- | ---------------- | ----------- | -------------- | ------- |
| DB      | production  | `<project>-prod` | pending     | migrations     | planned |

Manual creation command:

```
pnpm wrangler d1 create <DATABASE_NAME>
```

Example Wrangler config:

```
{
  "d1_databases": [
    {
      "binding": "DB",
      "database_name": "replace-with-database-name",
      "database_id": "replace-with-database-id",
      "migrations_dir": "migrations"
    }
  ]
}
```

Migration rules:

* migrations must be committed to the repository
* migration files must be reviewed before production deploy
* destructive migrations require explicit approval
* production migration execution must happen only through the approved GitHub Actions deploy path or a separately approved manual maintenance process
* never run remote D1 commands casually from local development

If migrations are needed, create files with Wrangler or manually, then commit them:

```
pnpm wrangler d1 migrations create <DATABASE_NAME> <MIGRATION_NAME>
```

Do not apply production migrations locally unless the user explicitly approves a maintenance exception.

## Durable Objects

Durable Objects are configured through Worker code and Wrangler config.

Unlike KV, R2, and D1, a Durable Object class is not usually created by a separate create-namespace command. It is declared as a binding and initialized through Wrangler migrations during deployment.

Before adding a Durable Object, document:

| Binding      | Class name  | Migration tag | Storage backend | Status  |
| ------------ | ----------- | ------------- | --------------- | ------- |
| RATE_LIMITER | RateLimiter | v1            | SQLite          | planned |

Example `wrangler.jsonc`:

```
{
  "durable_objects": {
    "bindings": [
      {
        "name": "RATE_LIMITER",
        "class_name": "RateLimiter"
      }
    ]
  },
  "migrations": [
    {
      "tag": "v1",
      "new_sqlite_classes": ["RateLimiter"]
    }
  ]
}
```

Rules:

* Durable Object classes must be exported from Worker code
* migration tags must be unique and append-only
* do not rename or delete classes without an explicit migration plan
* deletion migrations are destructive and require explicit approval
* prefer SQLite-backed Durable Objects for new classes
* production Durable Object migrations happen through GitHub Actions deploy from `main`

## GitHub Actions deployment

Deployment workflow must run only from `main`.

Example workflow:

```
name: Deploy

on:
  push:
    branches:
      - main

permissions:
  contents: read

jobs:
  deploy:
    runs-on: ubuntu-latest
    timeout-minutes: 20

    steps:
      - uses: actions/checkout@v4

      - uses: pnpm/action-setup@v4
        with:
          version: 10

      - uses: actions/setup-node@v4
        with:
          node-version: 22
          cache: pnpm

      - run: pnpm install --frozen-lockfile

      - run: pnpm test

      - run: pnpm build

      - name: Deploy Worker
        run: pnpm wrangler deploy
        env:
          CLOUDFLARE_API_TOKEN: ${{ secrets.CLOUDFLARE_API_TOKEN }}
          CLOUDFLARE_ACCOUNT_ID: ${{ secrets.CLOUDFLARE_ACCOUNT_ID }}
```

Only include commands that exist in `package.json`.

If `lint` or `typecheck` exists, include them.

If they do not exist, do not invent them.

## GitHub Secrets setup

Document secret names in `docs/ops.md`.

Manual setup through GitHub UI:

* repository Settings
* Secrets and variables
* Actions
* New repository secret
* add the required name and value

Manual setup through GitHub CLI:

```
gh secret set CLOUDFLARE_API_TOKEN
gh secret set CLOUDFLARE_ACCOUNT_ID
```

The agent may document these commands, but must not receive or print secret values.

## GitHub Actions resource permissions

Use least privilege.

For Cloudflare deployment:

* create a scoped Cloudflare API token
* restrict it to the account/zone needed for this Worker
* store it as `CLOUDFLARE_API_TOKEN`
* store account ID as `CLOUDFLARE_ACCOUNT_ID`

Do not commit tokens.

## Wrangler config rules

Allowed config files:

* `wrangler.toml`
* `wrangler.json`
* `wrangler.jsonc`

Do not maintain multiple Wrangler config files unless the repository already intentionally does so.

Prefer the existing config format.

When adding bindings:

* keep binding names stable
* separate production and staging resources
* avoid sharing production resources with staging
* do not invent IDs
* use placeholders only when docs clearly mark them as pending
* update docs when IDs are filled

## Local development

Local development must not require production secrets.

Allowed:

* local mock storage
* local Wrangler dev storage
* `.env.example`
* documented local placeholder values

Forbidden:

* production secrets in `.env`
* production secrets in `.dev.vars`
* committing local secret files
* requiring production Cloudflare resources to run unit tests

## Validation

Run safe checks only.

Common commands:

```
pnpm test
pnpm lint
pnpm typecheck
pnpm build
pnpm wrangler deploy --dry-run
```

Only run commands that exist or are safe.

Do not run production deploy as validation.

If using GitHub Actions, validate workflow syntax by inspection unless a safe local checker exists.

## Commit rules

Before committing:

```
git status --short
git branch --show-current
git config --get commit.gpgsign
git config --get user.signingkey
git log --show-signature -1
```

If current branch is `main`, stop and create a feature branch.

Commit message examples:

* `ops: document cloudflare resources`
* `ops: configure worker bindings`
* `ops: add github actions deploy workflow`
* `ops: add deployment docs`
* `ops: prepare d1 configuration`
* `ops: prepare durable object migration`

If commit signing fails:

* stop
* report the exact cause
* do not commit unsigned

## Output requirements

At the end, report:

* branch name
* files inspected
* files created
* files updated
* resource plan
* manual steps required
* GitHub Secrets required
* commands run
* checks passed
* checks failed
* commit hash if committed
* PR/deploy next step
* remaining risks

Never include secret values.

## Completion criteria

This skill is complete when:

* no direct work was done on `main`
* no direct production deploy was performed
* required Cloudflare resources are documented
* manual resource creation steps are documented
* required GitHub Secrets are documented by name only
* Wrangler config is prepared or placeholders are clearly marked
* GitHub Actions deployment is configured to run only from `main`
* `docs/deploy.md` is accurate
* `docs/ops.md` is accurate
* `.env.example` contains names only
* no secrets are committed
* commit signing was not bypassed
* future production deploy can happen only through PR merge into `main`
