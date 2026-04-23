# dotfiles

My dotfiles, managed with [chezmoi](https://chezmoi.io). Starts with Claude Code
config; more to come.

## Bootstrap

**Mac / Linux:**

```bash
sh -c "$(curl -fsLS get.chezmoi.io)" -- init --apply ryanmagoon
```

**Windows:**

```powershell
winget install twpayne.chezmoi
chezmoi init --apply ryanmagoon
```

**Preview before applying** on a new box:

```bash
chezmoi diff                 # what would change in $HOME
chezmoi apply --dry-run -v   # simulate
chezmoi apply                # go
```

See [`AGENTS.md`](./AGENTS.md) for repo structure and conventions.
