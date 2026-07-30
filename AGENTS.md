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
| Edit the shared AI instructions | `.chezmoitemplates/ai/<topic>.md`             |
| Reorder / add a topic           | `.chezmoitemplates/ai-instructions.md`        |
| Onboard a new AI tool           | New `dot_<tool>/<file>.md.tmpl` (see below)   |
| Codex config / prompts / rules  | See "Codex (first-class target)" below        |
| Edit Claude settings            | `dot_claude/settings.json`                    |
| Edit Claude keybindings         | `dot_claude/keybindings.json`                 |
| Add a Claude agent              | `dot_claude/agents/<name>.md`                 |
| Add a slash command             | `dot_claude/commands/<name>.md`               |
| Add a skill                     | `dot_claude/skills/<name>/`                   |
| Exclude a file from sync        | `.chezmoiignore` (target paths, not source)   |
| Document the repo for humans    | `README.md`                                   |

## AI instruction fan-out

The same coding conventions govern **every** AI coding tool (Claude Code, Codex,
opencode, …). They're authored **once** and fanned out. See
[`docs/adr/0001`](./docs/adr/0001-shared-ai-instructions-fanned-out.md) for why.

**How it fits together:**

- **Topic partials** live in `.chezmoitemplates/ai/` — one file per concern
  (`commits.md`, `testing.md`, …), mirroring the topical split. Edit these to
  change a rule.
- **The shared core** is `.chezmoitemplates/ai-instructions.md` — an explicit,
  ordered list of `includeTemplate` calls that assembles the partials. Edit this
  to reorder topics or add/remove one.
- **Delivery Targets** are thin per-tool wrappers, each a single line:
  `{{ includeTemplate "ai-instructions.md" . }}`. One per tool, materializing at
  that tool's expected path:
  - `dot_claude/CLAUDE.md.tmpl` → `~/.claude/CLAUDE.md`
  - `dot_codex/AGENTS.md.tmpl` → `~/.codex/AGENTS.md`
  - `dot_config/opencode/AGENTS.md.tmpl` → `~/.config/opencode/AGENTS.md`

The core is **tool-neutral**: it contains no `{{ if eq tool ... }}` branching.
Rules that read Claude-flavored (worktree detection, `~/.claude/rules/` paths)
have been generalized so they're true for any tool. Don't add tool-conditionals
to the core — if something is truly one-tool-only, it doesn't belong in the
shared body. Claude is the only tool with a larger config surface (settings,
keybindings, agents, commands, skills); that lives under `dot_claude/` and is
**not** part of the fan-out — only the instructions are shared.

**To onboard a new AI tool:** create one source file at the tool's instruction
path with the standard `dot_` mapping, containing the single include line. E.g.
for a tool that reads `~/.foo/AGENTS.md`:

```
mkdir -p dot_foo
printf '{{ "{{" }} includeTemplate "ai-instructions.md" . {{ "}}" }}\n' > dot_foo/AGENTS.md.tmpl
chezmoi diff   # preview, then: chezmoi apply
```

That's the whole job — the shared core comes along for free.

## Codex (first-class target)

Codex is the primary daily harness on the work machine, so treat
`~/.codex/AGENTS.md` as a first-class Delivery Target, not an afterthought.
Codex merges the global `~/.codex/AGENTS.md` with any repo-root `AGENTS.md`
(like this file) — both layers matter.

Beyond instructions, Codex has its own config surface, analogous to Claude's:

| Codex file                | Purpose                            | Tracked?          |
|---------------------------|------------------------------------|-------------------|
| `~/.codex/AGENTS.md`      | Global instructions (the fan-out)  | Yes — rendered    |
| `~/.codex/config.toml`    | Model, approval policy, MCP servers| Not yet — see below |
| `~/.codex/rules/*.rules`  | Command-permission prefix rules    | Not yet — see below |
| `~/.codex/prompts/*.md`   | Custom prompts (slash commands)    | Not yet — see below |
| `~/.codex/auth.json`      | OAuth tokens                       | NEVER (secret)    |

**Onboarding a machine that already has Codex config** (e.g. the work
machine): backport first, apply second. Copy its `config.toml`, `rules/`, and
`prompts/` into `dot_codex/` on a branch, strip anything machine-specific or
work-confidential, merge, and only then run `chezmoi apply` there. Applying
first would do nothing destructive (these files are unmanaged until tracked),
but backporting first is what makes the repo canonical. The richest Codex
config currently lives on the work machine, which is why these files aren't
tracked yet — don't seed them from a machine with placeholder config.

**Deferred tools (paths unconfirmed):** Cursor and Grok were intentionally left
out because their *global* instruction path varies by version and wasn't
verifiable without the tool installed. To add either, confirm the real path on a
machine that has it, then follow the recipe above:

- **Cursor** — global rules path has shifted across versions
  (`~/.cursor/rules/`, `AGENTS.md` support, …). Confirm before adding.
- **Grok** — confirm the Grok CLI's global-instructions path before adding.

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
  credentials. `~/.codex/auth.json` is an OAuth token blob sitting right next
  to files we do manage — never `chezmoi add` anything under `~/.codex/`
  without checking what it is. Scan before staging.
- **Committing Codex runtime state** (`~/.codex/sessions/`, `sqlite/`, `tmp/`,
  `models_cache.json`, `vendor_imports/`, `skills/.system/`). Same deal as the
  Claude runtime dirs: state, not config. `.chezmoiignore` blocks them at
  apply-time, but be careful with manual `chezmoi add`.
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
