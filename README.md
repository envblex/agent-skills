# Agent Skills

This repository stores reusable agent skills and supporting project templates.

## Repository Layout

```txt
.
├── docs/
├── skills/
│   ├── bootstrap-skill/
│   │   ├── SKILL.md
│   │   ├── agents/
│   │   │   └── openai.yaml
│   │   └── templates/
│   │       ├── base/
│   │       ├── chrome-extension/
│   │       ├── go-cli/
│   │       ├── nodejs-api/
│   │       └── nodejs-fullstack-webapp/
│   ├── explain-skill/
│   ├── implement-skill/
│   ├── ops-skill/
│   ├── refine-skill/
│   ├── review-skill/
│   └── spec-skill/
├── PATCH_REQUEST.md
└── TODO.md
```

The bootstrap skill lives at `skills/bootstrap-skill/`. Its templates are kept
under `skills/bootstrap-skill/templates/`; there is no repository-root
`bootstrap-skills/` directory.
