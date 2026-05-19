# Project Name

Briefly describe the Chrome extension project and the current approved scope.

## Current Scope

The current approved scope is defined in `SPEC.md`.

Do not treat ideas, notes, or deferred tasks as current scope unless they are explicitly included in `SPEC.md` or `PATCH_REQUEST.md`.

## Requirements

- Node.js
- pnpm
- Chromium-based browser for local testing

Document the exact Node.js and pnpm versions after implementation.

Example:

```bash
node --version
pnpm --version
```

## Setup

Install dependencies:

```bash
pnpm install
```

## Development

Run the development command:

```bash
pnpm run dev
```

Document framework-specific behavior after implementation.

## Build

Build the extension:

```bash
pnpm run build
```

Document the output directory after implementation.

Example:

```txt
dist/
```

## Local Testing

After building, load the extension from the generated output directory using the browser's extension developer mode.

General flow:

1. Open the browser extension management page.
2. Enable developer mode.
3. Load the built extension directory.
4. Verify the implemented behavior.

Document exact steps after the build output path is known.

## Verification

Before claiming completion, run the implemented checks.

Expected commands:

```bash
pnpm run format
pnpm run lint
pnpm run typecheck
pnpm run build
pnpm test
```

If a command does not exist, either add it or document that no automated check exists yet.

## Permissions

Document every extension permission used by the project.

Example:

```txt
permissions:
- storage: used for ...
host_permissions:
- https://example.com/*: used for ...
```

Do not request broad permissions unless they are required by the approved scope.

## Usage

Document implemented user-visible behavior only.

Do not document unimplemented features as available behavior.

## Privacy

Document what data the extension reads, stores, or transmits.

If the extension does not collect or transmit user data, state that clearly after verification.

## Deployment

Do not document Chrome Web Store publishing until it is implemented.

Default policy:

- build locally only for development
- do not publish unless explicitly approved
- do not add store credentials to the repository
- do not store secrets in the extension bundle

## Limitations

Document known limitations here.

## Deferred Work

Deferred work belongs in `TODO.md`, not in this README unless it affects current usage.
