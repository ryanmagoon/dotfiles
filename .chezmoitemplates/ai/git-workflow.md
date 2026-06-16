# Git Workflow

## Branch Hygiene

- **Start new branches from latest `main`.** Before creating a new branch or starting work on a fresh task, fetch and rebase onto `origin/main` to ensure you're building on the latest code. The only exception is when you're intentionally branching off an older commit or a non-main branch.
- **Use descriptive branch names.** Branch names should describe the change (e.g., `fix-savestate-dualcore-crash`, `feat/multi-disc-swap`). Never use auto-generated or random names like `relaxed-mcclintock` or `kind-sutherland`. When pushing to a remote or creating a PR, always rename to something meaningful first.
- **Check branch divergence from main periodically.** At natural landmarks (e.g., after completing a feature, before starting a new task), check how far ahead the working branch is from `main`. If it's significantly ahead, flag it so we can merge or rebase before the delta gets unwieldy.

## Pull Requests

- **Always squash merge PRs.** Use `gh pr merge --squash` (or the GitHub UI squash-and-merge option) so each PR lands on `main` as a single, clean commit. This keeps `main` history readable regardless of how many granular commits the working branch had.
