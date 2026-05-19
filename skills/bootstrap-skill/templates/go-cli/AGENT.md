# AGENT

This file defines additional rules for Go CLI projects.

It extends the base `AGENT.md` template. These rules must not weaken the base rules.

## Project Type

This project is a Go command-line application.

## Required Reading

Before working, read:

- `SPEC.md`
- `TODO.md`
- `README.md`
- `AGENT.md`

If `PATCH_REQUEST.md` exists, read it first and treat it as the current patch scope.

## Language

Use Go.

Do not introduce another implementation language unless the current scope explicitly requires it.

## Package Manager

Use Go modules.

Allowed:

- `go mod init`
- `go mod tidy`
- `go mod download`
- `go run`
- `go build`
- `go test`
- `go vet`
- `gofmt`

Do not add vendored dependencies unless explicitly required.

## Source Layout

Prefer this layout:

```txt
cmd/
  <app-name>/
    main.go
internal/
  ...
go.mod
README.md
SPEC.md
TODO.md
AGENT.md
```

Rules:

- Put the CLI entry point under `cmd/<app-name>/`.
- Use `internal/` only when separation is useful.
- Do not create empty architecture.
- Do not add broad framework structure before it is needed.

## Dependency Rules

Prefer the Go standard library.

Add third-party dependencies only when:

- the standard library would make the implementation significantly more error-prone
- the dependency is small and maintained
- the dependency is directly needed by the approved scope

Do not add dependencies for hypothetical future features.

## CLI Behavior

The CLI must:

- provide useful `--help` output when implemented
- return non-zero exit codes on failure
- write normal output to stdout
- write errors to stderr
- avoid hidden network access
- avoid writing persistent files unless explicitly specified
- avoid reading secrets unless explicitly specified

## Error Handling

Use explicit error handling.

Do not:

- ignore returned errors
- panic for normal user-facing errors
- print misleading success messages
- hide partial failures

## Verification

Before claiming completion, run:

```bash
gofmt -w .
go test ./...
go vet ./...
```

If any command fails, report:

- the command
- the failure summary
- the likely cause
- whether the failure is caused by the current change

## Git Rules

Follow the base Git authority model.

Do not:

- commit on `main`
- push to `main`
- force-push
- merge pull requests
- create or edit secrets
- deploy locally
