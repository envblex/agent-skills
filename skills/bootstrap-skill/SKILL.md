---
name: bootstrap-skill
description: Initialize or normalize a repository by creating project documents, task structure, scripts, and development conventions before implementation.
---

# Bootstrap Skill

Initialize or normalize a repository by copying project documents and
project-type guidance from this skill's own templates.

## Template Location

All templates are stored under this skill directory:

```txt
skills/bootstrap-skill/templates/
```

When this skill refers to `templates/...`, resolve that path relative to
`skills/bootstrap-skill/`, not relative to the repository root. Do not depend on
or recreate a repository-root `templates/` directory.

Supported template directories are:

- `templates/base/`
- `templates/nodejs-api/`
- `templates/nodejs-fullstack-webapp/`
- `templates/chrome-extension/`
- `templates/go-cli/`

## Workflow

1. Read the user's project notes and identify the target project type.
2. Copy the baseline project documents from `templates/base/`.
3. If the project type matches a supported template, copy or merge the matching
   template guidance from the skill-relative template directory.
4. Keep generated project documents focused on the approved initial scope.
5. Do not introduce unsupported template types or repository-root template
   paths.
