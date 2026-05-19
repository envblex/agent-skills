# AGENT

This file defines additional rules for Chrome extension projects.

It extends the base `AGENT.md` template. These rules must not weaken the base rules.

## Project Type

This project is a Chrome extension.

## Required Reading

Before working, read:

- `SPEC.md`
- `TODO.md`
- `README.md`
- `AGENT.md`

If `PATCH_REQUEST.md` exists, read it first and treat it as the current patch scope.

## Language

Use TypeScript when JavaScript is required.

Rules:

- Prefer `.ts` and `.tsx`.
- Do not introduce `.js` unless required by the extension framework or build system.
- Keep type checking enabled.
- Do not weaken TypeScript strictness without explicit approval.

## Package Manager

Use pnpm only.

Forbidden:

- `npm install`
- `yarn`
- `bun install`
- `package-lock.json`
- `yarn.lock`
- `bun.lockb`

Allowed:

- `pnpm install`
- `pnpm add <package>`
- `pnpm add -D <package>`
- `pnpm run <script>`

## Extension Rules

The extension must use the narrowest permissions possible.

Do not add broad permissions unless explicitly required by the approved scope.

Avoid:

- `<all_urls>`
- broad host permissions
- browsing history access
- tab access
- scripting access
- storage access
- clipboard access

unless the feature requires them and the reason is documented.

## Security Rules

The extension must not:

- collect unnecessary user data
- transmit page content unless explicitly approved
- inject remote scripts
- load remote executable code
- hide network behavior
- store secrets in the extension bundle
- request permissions for future unused features
- obfuscate code to hide behavior

## Source Layout

Prefer a small layout.

Example:

```txt
src/
  ...
public/
  manifest.json
package.json
README.md
SPEC.md
TODO.md
AGENT.md
```

The exact layout may differ if the selected extension framework requires it.

Do not add unnecessary architecture.

## UI Rules

Keep extension UI minimal.

Do not add:

- account systems
- payment flows
- analytics UI
- settings pages
- onboarding flows

unless explicitly required by the approved scope.

## Verification

Before claiming completion, run available commands.

Expected commands:

```bash
pnpm run format
pnpm run lint
pnpm run typecheck
pnpm run build
pnpm test
```

Manual verification should include loading the built extension in a Chromium-based browser.

## Git Rules

Follow the base Git authority model.

Do not:

- commit on `main`
- push to `main`
- force-push
- merge pull requests
- create or edit secrets
- deploy locally
- publish to the Chrome Web Store unless explicitly requested by the user
