# Agents guide

A map of this repo for AI agents (Claude, Codex, etc.). Humans should read
[`README.md`](./README.md) first.

## What this repo is

Personal dotfiles, managed by [chezmoi](https://chezmoi.io). The repo root IS
the chezmoi source directory (`~/.local/share/chezmoi`). Files at `dot_<name>/`
map to `~/.<name>/` on every machine chezmoi applies to.

Starts with Claude Code config; more tools will follow.

## Where to look

| Task                            | Location                                      |
|---------------------------------|-----------------------------------------------|
| Edit global Claude preamble     | `dot_claude/CLAUDE.md`                        |
| Edit Claude settings            | `dot_claude/settings.json`                    |
| Edit Claude keybindings         | `dot_claude/keybindings.json`                 |
| Add a Claude agent              | `dot_claude/agents/<name>.md`                 |
| Add a slash command             | `dot_claude/commands/<name>.md`               |
| Add a skill                     | `dot_claude/skills/<name>/`                   |
| Exclude a file from sync        | `.chezmoiignore` (target paths, not source)   |
| Document the repo for humans    | `README.md`                                   |

## Conventions

- **Source → target mapping**: chezmoi rewrites `dot_` → `.` at apply time.
  `dot_claude/CLAUDE.md` in the repo becomes `~/.claude/CLAUDE.md` on the
  machine. Don't create literal `.claude/` dirs in the repo — chezmoi won't
  pick them up the same way.
- **OS-conditional content**: rename `foo` to `foo.tmpl` and wrap with
  `{{ if eq .chezmoi.os "darwin" }}...{{ end }}`. Mac-only files are fine.
- **Host-conditional content**: same idea with
  `{{ if ne .chezmoi.hostname "..." }}`. Not used yet.
- **`.chezmoiignore` uses TARGET paths**, not source paths. So
  `.claude/sessions/**` — never `dot_claude/sessions/**`.
- **Dotfiles inside a `dot_` dir**: use `dot_<name>` again, e.g.
  `dot_claude/dot_hiddenfile`. Plain `.gitkeep` in a source dir is ignored by
  chezmoi (treated as chezmoi metadata prefix) — which is actually handy for
  keeping empty dirs tracked in git without materializing them in `$HOME`.

## Anti-patterns

- **Editing `~/.claude/*` directly on a chezmoi-managed machine** — changes
  are lost the next time `chezmoi apply` runs. Edit the source, then apply.
- **Committing runtime dirs** (`sessions/`, `backups/`, `projects/`,
  `shell-snapshots/`, `session-env/`). These are state, not config, and can
  contain conversation history / transient data. `.chezmoiignore` blocks them
  at apply-time, but `chezmoi add ~/.claude/sessions/...` will still write
  them into the source if you're not careful. Don't.
- **Committing secrets**. This repo is PUBLIC. No API keys, no session
  tokens, no `.env` contents, no `~/.claude.json` auth blob, no OAuth
  credentials. Scan before staging.
- **Committing `settings.local.json`** — it's per-machine state.
  `.chezmoiignore` already excludes it. Don't fight that.
- **Hardcoding absolute paths** that only work on one OS. Use
  `{{ .chezmoi.homeDir }}` in templates, or just trust chezmoi's `dot_`
  convention to resolve `$HOME` correctly per-platform.
- **Adding Mac-only tools without templating** (Homebrew bundles, Fish
  config, macOS defaults). These will fail on Windows. Gate behind
  `{{ if eq .chezmoi.os "darwin" }}` so Windows silently skips.

## Safety checklist before any `chezmoi apply`

1. `chezmoi diff` — read the diff, confirm nothing unexpected would change.
2. `chezmoi apply --dry-run -v` — simulate with verbose logging.
3. Only then: `chezmoi apply`.
