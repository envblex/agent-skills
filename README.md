# Envblex Skills

Envblex Skills is a local collection of AI-agent workflow skills maintained for envblex projects.

These skills define repeatable workflows for:

- repository bootstrap
- specification writing
- implementation
- review
- refinement
- explanation
- operations

Envblex Skills is not:

- a general-purpose skill registry
- an official OpenAI, Anthropic, Google, GitHub, or Cloudflare product
- a package manager
- a deployment system
- a replacement for Git permissions, branch protection, repository rules, or token restrictions

The skills are plain repository assets. They are intended to be copied, referenced, or installed into an agent runtime that understands skill-style instruction files.

## install

Use skills.

[![skills.sh](https://skills.sh/b/envblex/agent-skills)](https://skills.sh/envblex/agent-skills)

```bash
pnpm dlx skills add envblex/agent-skills
```

## Repository Layout

```txt
skills/
  bootstrap-skill/
  explain-skill/
  implement-skill/
  ops-skill/
  refine-skill/
  review-skill/
  spec-skill/
templates/
  base/
  chrome-extension/
  go-cli/
  nodejs-api/
  nodejs-fullstack-webapp/
```

Each skill contains a canonical instruction file:

```txt
skills/<skill-name>/SKILL.md
```

Some skills may also contain agent-specific adapter files:

```txt
skills/<skill-name>/agents/
```

`SKILL.md` is the source of truth. Agent-specific adapter files must not weaken the rules defined in `SKILL.md`.

## Skills

### Bootstrap Skill

Initializes or normalizes a repository by creating project documents, task structure, scripts, and development conventions before implementation.

Typical output:

- `SPEC.md`
- `TODO.md`
- `README.md`
- `AGENT.md`

### Spec Skill

Turns rough project notes into a structured implementation specification.

Typical output:

- `SPEC.md`
- `TODO.md`
- `README.md`
- `AGENT.md`

### Implement Skill

Implements only the approved current scope defined by the project documents.

The implementation workflow must respect:

- `PATCH_REQUEST.md`, if present
- `SPEC.md`
- `AGENT.md`
- `README.md`
- `TODO.md`

### Review Skill

Reviews repository changes against the approved scope and project rules.

Priority order:

1. scope violations
2. security issues
3. correctness issues
4. verification gaps
5. maintainability issues
6. documentation drift

### Refine Skill

Converts rough change notes into a focused patch request.

Typical output:

- `PATCH_REQUEST.md`

### Explain Skill

Explains code, specs, errors, repository state, or verification failures without changing files.

### Ops Skill

Defines operational rules for Git, verification, deployment boundaries, and completion gates.

## Installation

Clone this repository locally:

```bash
git clone https://github.com/envblex/agent-skills.git
cd agent-skills
```

Install the skills by copying or symlinking the `skills/` directory into the target agent workspace.

### Symlink install

Use this when local changes in this repository should be reflected immediately.

```bash
mkdir -p ~/.local/share/envblex-skills
ln -s "$(pwd)/skills" ~/.local/share/envblex-skills/skills
```

### Copy install

Use this when a stable snapshot is preferable.

```bash
mkdir -p ~/.local/share/envblex-skills
cp -R skills ~/.local/share/envblex-skills/
```

### Manual per-skill install

Use this when the target agent expects one skill directory at a time.

```bash
mkdir -p ~/.local/share/envblex-skills/skills
cp -R skills/spec-skill ~/.local/share/envblex-skills/skills/
cp -R skills/implement-skill ~/.local/share/envblex-skills/skills/
cp -R skills/review-skill ~/.local/share/envblex-skills/skills/
```

## Usage

Open the relevant `SKILL.md` file and provide it to the target agent as the active workflow instruction.

Examples:

```txt
skills/spec-skill/SKILL.md
skills/implement-skill/SKILL.md
skills/review-skill/SKILL.md
skills/ops-skill/SKILL.md
```

For repository bootstrapping, use:

```txt
skills/bootstrap-skill/SKILL.md
```

For patch refinement, use:

```txt
skills/refine-skill/SKILL.md
```

For explanation-only work, use:

```txt
skills/explain-skill/SKILL.md
```

## Recommended Workflow

Use the skills in this order for a new project:

1. `bootstrap-skill`
2. `spec-skill`
3. `implement-skill`
4. `review-skill`
5. `ops-skill`

For an existing project change, use:

1. `refine-skill`
2. `implement-skill`
3. `review-skill`
4. `ops-skill`

For explanation-only work, use:

1. `explain-skill`

## Git and Deployment Policy

Envblex Skills assumes strict separation between human authority and agent authority.

Agents may work on non-main branches.

Agents must not:

- push to `main`
- commit directly on `main`
- merge pull requests
- edit secrets
- create or rotate deploy tokens
- change deployment settings
- change repository permissions
- change branch protection or repository rules
- bypass checks
- deploy from a local machine

Production deployment should be controlled by GitHub Actions from `main`, not by local agent commands.

The documentation in this repository defines workflow rules. It does not replace actual GitHub permissions, token restrictions, branch protection, or repository rules.

## Security Model

This repository can define agent behavior, but it cannot enforce repository permissions by itself.

Actual enforcement must happen through:

- GitHub token scopes
- GitHub Secrets
- branch protection rules
- repository rulesets
- GitHub Actions permissions
- human review before merge

Do not give an agent a token that can perform actions the agent is not allowed to perform.

In particular, do not give an agent a token that can push to `main` if the intended policy is that agents cannot push to `main`.

## Generated Project Documents

The base project template expects these files:

```txt
SPEC.md
TODO.md
README.md
AGENT.md
```

### `SPEC.md`

Defines the current approved implementation scope.

### `TODO.md`

Tracks deferred work. It is not automatically current scope.

### `README.md`

Explains how to set up and use the generated project.

### `AGENT.md`

Defines repository-specific rules for future agents.

## Verification

Before claiming a change is complete, run the relevant checks for the repository being modified.

For this repository, at minimum run:

```bash
git status -sb
git diff --check
```

If project-specific scripts are added later, document them here.

## Status

This repository is maintained as a local workflow toolkit for envblex projects.

It should remain small, explicit, and easy to audit.
