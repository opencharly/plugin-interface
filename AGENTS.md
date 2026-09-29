# AGENTS.md — plugin-interface

Standalone plugin repo for the host-coupled, compiled-in `interface` check verb
(`verb:interface`). The plugin is a Go module at `candy/plugin-interface/`
(module path `github.com/opencharly/plugin-interface/candy/plugin-interface`);
the root `charly.yml` only declares `discover: candy` so the repo is a project
and its candy is scanned.

Canonical files:

- `candy/plugin-interface/charly.yml` — the `plugin-interface:` candy entity
  (`plugin:` block, `plan:` check).
- `candy/plugin-interface/` — the Go source: `plugin.go`,
  `schema/interface.cue` (the self-contained `#InterfaceInput`),
  `params/cue_types_gen.go`, `cmd/serve/main.go`.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-internals:plugin` — the plugin authoring reference: the `plugin:`
  block, the unified Provider model, the per-plugin CUE-schema contract,
  placement. Load before touching the provider or schema.
- `/charly-check:check` — the declarative check-step surface the `interface:`
  verb is authored through (the check verb catalog).
- `/charly-internals:git-workflow` — before any git/PR action.

## Build / validate / test

- `go build ./...` in `candy/plugin-interface/` — compile the plugin module.
- `go test ./...` in `candy/plugin-interface/` — the plugin's Go tests.
- `charly box validate` at the repo root — the structural check (the candy +
  `plugin:` block, CUE schema).
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no**
  per-repo candy gate.
- The changed path is exercised by any check bed composing the `interface:`
  verb.

## Modify this repo

- Edit the `plugin-interface:` candy entity, the Go source, and
  `schema/interface.cue` **together** — the schema is the single source for the
  verb's `params/` struct.
- The package is `iface` (not `interface`) — `interface` is a Go keyword; the
  reserved verb word stays `interface`.

## Landing

- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Load
  `/charly-internals:git-workflow` before any git/PR action; history lives in
  `CHANGELOG/`. Do not restate its rules here.
