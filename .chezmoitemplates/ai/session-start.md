# Session Start

At the start of every new conversation, before doing anything else:

1. **Sync with latest main:**
   - **Normal checkout:** `git fetch origin && git checkout main && git pull origin main`
   - **Linked worktree** (the working dir is a linked git worktree, not the main
     checkout — `git rev-parse --git-common-dir` differs from `.git`): you cannot
     check out `main` because the primary checkout already holds it. Instead:
     `git fetch origin && git rebase origin/main`.
2. If there are stashed or uncommitted changes from a previous session, surface them to the user and ask what to do (keep, drop, or stash).

Never give advice about what to work on next, or start new work, from a stale branch.
