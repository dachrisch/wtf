# AGENTS.md — modules/

Each subdirectory here is a self-contained dashboard widget package. Some are grouped by provider (for example `cryptocurrency/`, `stocks/`, `weatherservices/`, `ipaddresses/`).

## Module conventions

- A minimal module has `settings.go` (a `Settings` struct with a `*cfg.Common` field, constructed by `NewSettingsFromYAML`) and `widget.go` (a `Widget` embedding a `view.*Widget` type plus its settings). Larger modules split out `display.go` and `keyboard.go`.
- Every widget must satisfy the `wtf.Wtfable` interface defined in [`wtf/wtfable.go`](../wtf/wtfable.go): it embeds `Enablable`, `Schedulable`, and `Stoppable`, and adds title, focus, help, config-text, and `*tview.TextView` methods.
- New text widgets can be scaffolded with the generator in [`generator/textwidget.go`](../generator/textwidget.go) (it carries a `//go:build ignore` tag). From the repo root, set `WTF_WIDGET_NAME=MyWidget` and run that file; it writes `modules/<lowercase-name>/widget.go` and `settings.go` from the templates in `generator/`.
- A module is not active until it is registered: add an import and a `case` to `MakeWidget`'s switch in [`app/widget_maker.go`](../app/widget_maker.go). That switch must stay in alphabetical order, and each case builds settings with `NewSettingsFromYAML` and then the widget with `NewWidget` (constructor signatures vary by module).
- Module configuration lives under `wtf.mods.<name>` in YAML; `cfg.Common` provides the shared enabled/position/refresh settings and validations. Keep provider credentials out of source (see the root [AGENTS.md](../AGENTS.md)).
