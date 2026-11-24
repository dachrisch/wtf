# AGENTS.md

## What this repo is

WTF ("wtfutil") is a personal information dashboard for the terminal, written in Go. This is a fork of `wtfutil/wtf` that produces a patched binary with OSC 8 hyperlink support; the default branch is `trunk`. The Go module is `github.com/wtfutil/wtf` (see `go.mod`) and the built binary is named `wtfutil`.

## Layout

- `main.go` — entry point.
- `app/` — application manager, config validation, and the widget factory (`widget_maker.go`).
- `cfg/`, `wtf/` — YAML config parsing and the shared `Wtfable` widget interface.
- `view/`, `logger/`, `flags/`, `help/`, `checklist/`, `utils/` — shared UI and support packages.
- `modules/` — one package per dashboard widget. See [modules/AGENTS.md](modules/AGENTS.md).
- `generator/` — scaffolding templates for new text widgets.
- `_sample_configs/` — example YAML configuration files.
- Documentation lives in the separate [wtfdocs](https://github.com/wtfutil/wtfdocs) repo; do not edit it here.

## Build, test, verify

Run from the repo root. The Makefile sets `GO111MODULE=on` and `GOPROXY=https://proxy.golang.org,direct`.

- `make build` — build `bin/wtfutil` (`go build -o bin/wtfutil`). The release workflow instead builds with `GOTOOLCHAIN=auto go build -o wtf-binary .`.
- `make test` — build, then `go test ./...` (`go test ./...` also works on its own).
- `make coverage` — `go test -coverprofile=coverage.out ./...`, then `go tool cover -html=coverage.out`.
- `make lint` — installs/runs `golangci-lint` (`golangci-lint cache clean` then `golangci-lint run`); CI uses golangci-lint v2.11 with `./... --timeout=10m`.
- `make run` — build and launch `bin/wtfutil` (requires a config file).
- `make install` / `make clean` / `make help` — install the binary, remove build output, list targets.

PR checks against `trunk` also run static analysis (imports, errcheck, shadow, staticcheck) and a GoReleaser snapshot build. Aim for `go test ./...` and `golangci-lint run ./...` to pass before opening a PR.

## Safety policies

- Never commit generated build or release artifacts: `bin/`, `wtf-binary`, `wtf-patched-*/`, `wtf-patched-*.tar.gz`, and `coverage.out`. The release workflow creates these on `trunk`.
- Never commit credentials or secrets. `gcal/client_secret.json` and `gspreadsheets/client_secret.json` are gitignored; keep them out of commits.
- Do not push directly to `trunk`; work on a branch and open a PR against `trunk`.
- Follow [CONTRIBUTING.md](CONTRIBUTING.md) and [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md). Report security issues via GitHub Issues as described in [SECURITY.md](SECURITY.md).
