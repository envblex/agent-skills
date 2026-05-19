# Project Name

Briefly describe the Go CLI project and the current approved scope.

## Current Scope

The current approved scope is defined in `SPEC.md`.

Do not treat ideas, notes, or deferred tasks as current scope unless they are explicitly included in `SPEC.md` or `PATCH_REQUEST.md`.

## Requirements

- Go
- Git

Document the exact Go version after the project is initialized.

Example:

```bash
go version
```

## Setup

Install dependencies:

```bash
go mod download
```

If the project has no external dependencies yet, this command may complete without downloading anything.

## Development

Run the CLI locally:

```bash
go run ./cmd/<app-name>
```

Replace `<app-name>` with the actual command directory.

## Build

Build the CLI:

```bash
go build ./cmd/<app-name>
```

For a named output binary:

```bash
go build -o ./bin/<app-name> ./cmd/<app-name>
```

## Test

Run tests:

```bash
go test ./...
```

## Verification

Before claiming completion, run:

```bash
gofmt -w .
go test ./...
go vet ./...
```

If automated verification does not exist yet, state that clearly.

## Configuration

Document CLI flags, environment variables, and config files here after implementation.

Do not store secrets in tracked files.

Use examples with placeholder values only.

## Usage

Document implemented commands only.

Example format:

```bash
<app-name> --help
<app-name> <subcommand>
```

Do not document commands that are not implemented.

## Exit Codes

Document non-zero exit codes after they are defined.

Example:

```txt
0  success
1  general error
2  invalid input
```

## Limitations

Document known limitations here.

## Deferred Work

Deferred work belongs in `TODO.md`, not in this README unless it affects current usage.
