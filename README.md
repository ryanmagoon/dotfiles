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

## Private overlay

The zsh config (`dot_zshrc`, `dot_zshenv`) sources an optional overlay from
`~/.config/zsh/overlay/{zshenv,zshrc}.zsh` for machine-specific setup that does
not belong in a public repo. To have chezmoi clone and keep it updated, set the
repo URL in the machine's local chezmoi config (never committed here):

```toml
# ~/.config/chezmoi/chezmoi.toml
[data]
zshOverlayRepo = "https://github.com/<owner>/<private-repo>.git"
```

`chezmoi update` then pulls both repos (the overlay at most daily; add
`--refresh-externals` to force it). Edit overlay files in place and commit from
`~/.config/zsh/overlay`.
