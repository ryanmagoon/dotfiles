# chezmoi over bare-git, symlinks, or a build script

**Status:** accepted

We manage these dotfiles with chezmoi even though the repo currently uses only a
small slice of it — the `dot_` → `~/.` rename and `includeTemplate` fan-out (no
OS conditionals in source, no run scripts, no encryption, no externals).

The deciding requirement is **managed single-source fan-out**: one authored body
of AI instructions that lands at several tool-specific paths, kept auditable.

Considered and rejected:

- **Bare git repo (`--git-dir` trick) or GNU Stow / symlinks** — lighter, very
  common, but cannot fan out. One source can't become several distinct target
  files without duplication, and duplication drifts. These only "win" if we
  abandon single-source, which is the whole point of the repo.
- **A small build/concatenation script** — does fan-out and is lighter than
  chezmoi, but produces files nothing tracks: no `diff`, no `--dry-run`, no
  preview, no cleanup when a tool is removed. We actually built this first
  (dual sh/ps1 run scripts) and dropped it when verification showed it writing
  untracked files outside chezmoi's destination model.

chezmoi is the lightest option that does single-source fan-out **and** keeps it
managed (previewable, dry-runnable, self-cleaning). The "heaviness" is a one-time
binary install; the per-file mechanism we rely on is small and stable. Revisit
only if the fan-out requirement itself goes away (e.g. all tools converge on a
single shared `AGENTS.md` path), at which point a bare git repo would suffice.
