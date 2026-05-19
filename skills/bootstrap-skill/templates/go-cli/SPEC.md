# SPEC

## Project Type

Go CLI application.

## Purpose

Build a small, auditable command-line application in Go.

## Current Scope

The initial scope should include only:

- a minimal Go module
- a CLI entry point under `cmd/<app-name>/`
- implemented commands explicitly listed in this specification
- basic input validation
- explicit error handling
- tests for core behavior

## Out of Scope

Do not add unless explicitly requested:

- daemon mode
- background services
- network APIs
- GUI
- TUI
- plugin systems
- auto-update logic
- telemetry
- analytics
- shell completion
- package publishing
- cross-compilation release automation
- installer generation

## Expected Layout

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

Use `internal/` only when the implementation benefits from separation.

Do not create empty packages for architecture that does not exist yet.

## CLI Requirements

The CLI should:

- provide clear help text
- validate user input
- return non-zero exit codes on failure
- write normal output to stdout
- write errors to stderr
- avoid hidden network access
- avoid persistent file writes unless explicitly specified
- avoid reading secrets unless explicitly specified

## Dependency Rules

Prefer the Go standard library.

Add dependencies only when they clearly reduce complexity or improve correctness.

Do not add heavy frameworks for a small CLI.

## Error Handling

The implementation must not ignore errors.

Normal user-facing errors should be returned and printed clearly, not handled with `panic`.

## Security Rules

The CLI must not:

- log secrets
- write secrets into files
- transmit data over the network unless explicitly approved
- collect telemetry unless explicitly approved
- modify files outside documented paths

## Verification

Required commands:

```bash
gofmt -w .
go test ./...
go vet ./...
```

## Completion Criteria

A change is complete only when:

- implemented behavior matches this specification
- unsupported commands are not documented
- tests pass
- no secrets are introduced
- the diff is scoped and reviewable
- verification commands have been run and reported
