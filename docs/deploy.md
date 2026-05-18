# Deployment

## Policy

Production deploys are only performed by GitHub Actions from `main`.

Direct push to `main` is forbidden.

## Deploy flow

```text
feature branch
-> pull request
-> review
-> merge to main
-> GitHub Actions deploy
```

## Required GitHub Secrets

| Name | Purpose |
| ---- | ------- |
| CLOUDFLARE_API_TOKEN | Required only when a future Cloudflare Worker deployment workflow is added |
| CLOUDFLARE_ACCOUNT_ID | Required only when a future Cloudflare Worker deployment workflow is added |

## Required Cloudflare resources

| Resource | Binding | Environment | Created |
| -------- | ------- | ----------- | ------- |

No Cloudflare resources are required yet.

## GitHub Actions workflow

No deployment workflow is configured yet because the repository does not currently contain a deployable application, Wrangler configuration, or package manager file.

When a deployable Worker is added, the production workflow must be triggered only by pushes to `main` and must read deployment credentials from GitHub Secrets.

## Local validation

There are no local validation commands yet. Add validation commands to `package.json` before referencing them in GitHub Actions.

## Production deploy

Production deployment is automatic after merge to `main` once a deployment workflow exists.

Do not run local production deploy.

## Rollback

Revert the merged pull request and allow GitHub Actions to redeploy from `main`.

## Troubleshooting

* If deployment credentials are missing, add or rotate GitHub Secrets manually in repository settings.
* If Wrangler configuration is missing, add the Worker config and document all bindings in `docs/ops.md`.
* If a workflow references a missing script, either add the script or remove the workflow step before merging.
