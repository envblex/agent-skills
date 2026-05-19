# SPEC

## Project Type

Chrome extension.

## Purpose

Build a small, auditable Chrome extension with minimal permissions.

## Current Scope

The initial scope should include only:

- manifest setup
- minimal extension entry points required by the approved behavior
- implemented UI or content-script behavior explicitly listed in this specification
- build and verification commands
- local loading instructions
- documented permissions

## Out of Scope

Do not add unless explicitly requested:

- Chrome Web Store publishing
- Firefox support
- account systems
- remote backend services
- analytics
- telemetry
- broad host permissions
- persistent tracking
- payment features
- obfuscated code
- remote code loading
- automatic updates outside browser mechanisms

## Expected Layout

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

Avoid unnecessary structure.

## Manifest Requirements

The manifest must include only required fields.

Document:

- extension name
- extension purpose
- permissions
- host permissions
- background script, if used
- content scripts, if used
- action/popup, if used
- options page, if used

## Permission Rules

The extension must use the narrowest permissions possible.

Do not add:

- `<all_urls>`
- broad host permissions
- browsing history access
- tab access
- scripting access
- storage access
- clipboard access

unless the approved scope requires them and the reason is documented.

## Security Rules

The extension must not:

- collect unnecessary user data
- transmit page content unless explicitly approved
- inject remote scripts
- load remote executable code
- hide network behavior
- store secrets in the extension bundle
- request permissions for future unused features
- include tracking by default

## Package Manager

Use pnpm only.

Forbidden:

- `npm install`
- `yarn`
- `bun install`
- `package-lock.json`
- `yarn.lock`
- `bun.lockb`

## Verification

Expected commands:

```bash
pnpm run format
pnpm run lint
pnpm run typecheck
pnpm run build
pnpm test
```

Manual verification should include loading the built extension in a Chromium-based browser.

## Completion Criteria

A change is complete only when:

- implemented behavior matches this specification
- permissions are minimal and documented
- the extension builds successfully
- no secrets are bundled
- no unapproved data collection is added
- no remote executable code is loaded
- documentation matches actual behavior
- the diff is scoped and reviewable
