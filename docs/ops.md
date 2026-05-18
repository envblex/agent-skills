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
| CLOUDFLARE_API_TOKEN | production | Cloudflare deployment token for a future Worker deployment workflow | no, not until a deployable Worker exists | yes |
| CLOUDFLARE_ACCOUNT_ID | production | Cloudflare account identifier for a future Worker deployment workflow | no, not until a deployable Worker exists | yes |

## Cloudflare resources

No Cloudflare resources are currently required by this repository. Add rows only after code or configuration introduces a real binding.

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

No manual Cloudflare setup is required yet.

When a deployable Cloudflare Worker is added:

1. Create the required Cloudflare resources manually after explicit approval.
2. Add non-secret resource names and IDs to Wrangler configuration.
3. Add required GitHub Secrets in repository settings.
4. Open a pull request and merge to `main` before production deployment.

## Validation steps

Current repository validation is by inspection only because there is no package manager file, Wrangler config, deploy workflow, or automated test command.

When a Node.js project is added, use the package manager implied by the lockfile and run only scripts that exist in `package.json`.

Before reporting ops work complete, run the `ops-skill` Git completion gate and
report branch, worktree, push, and pull request state. Production deployment
must still be left to GitHub Actions after merge to `main`.

## Rollback notes

Rollback depends on the future deployment target. For GitHub Actions based production deployments, prefer reverting the merged pull request and letting the `main` deployment workflow redeploy the reverted state.

## Known risks

* No GitHub Actions deployment workflow exists because there is no deployable application or Wrangler configuration yet.
* The repository currently has no initial commit, so commit-signature history cannot be verified.
* Future Cloudflare resources must not share production data with staging or local development.
