# TODO

This file tracks deferred work. Items in this file are not current implementation scope unless `SPEC.md` or `PATCH_REQUEST.md` explicitly promotes them.

## Deferred

### Skill packaging

- Define a stable install layout for different agent runtimes.
- Add examples for agent-specific installation paths when they are known.
- Decide whether to add an install script.

### Template coverage

- Review all base templates for consistency.
- Review stack-specific templates:
  - `chrome-extension`
  - `go-cli`
  - `nodejs-api`
  - `nodejs-fullstack-webapp`
- Ensure stack-specific templates do not weaken base Git, security, or deployment rules.

### Verification

- Add repository-level verification scripts if the repository starts containing generated or compiled assets.
- Add markdown linting if documentation style becomes inconsistent.

### Documentation

- Add examples of complete workflows:
  - new project bootstrap
  - patch request refinement
  - implementation review
  - operations review
- Add a compatibility table for supported agent runtimes if needed.

### Security and operations

- Document recommended GitHub token scopes.
- Document recommended GitHub Actions permissions.
- Document recommended repository rulesets for public and private repositories.
- Document how to handle repositories where GitHub branch protection is unavailable.

## Not Planned

The following are intentionally not planned at this stage:

- hosted skill registry
- package publishing
- remote installer using `curl | bash`
- automatic secret management
- automatic deployment management
- granting agents write access to `main`
