# dotfiles

Personal dotfiles managed by chezmoi. The repo root is the chezmoi source
directory; `dot_<name>/` entries map to `~/.<name>/`. Cross-platform (macOS +
Windows) and cross-tool by design.

## Language

**AI Instructions**:
The tool-agnostic body of coding conventions, rules, and preferences that should
govern every AI coding assistant. Authored once as a shared template partial,
then fanned out to each tool's expected location.
_Avoid_: "CLAUDE.md content" (that's one delivery target, not the source)

**AI Tool**:
An AI coding assistant that reads instructions from a known path — Claude Code,
Codex, Cursor, Grok, opencode. Each is a distinct delivery target for the same
AI Instructions.
_Avoid_: agent (overloaded — chezmoi/Claude both use it differently)

**Delivery Target**:
The tool-specific file + location that an AI Tool reads its instructions from
(e.g. `~/.claude/CLAUDE.md`, `~/.codex/AGENTS.md`). A thin chezmoi template that
wraps the shared AI Instructions with any tool-specific preamble.
_Avoid_: "the config file"

**Shared Template Partial**:
A chezmoi `.chezmoitemplates/` file holding reusable content included by multiple
Delivery Targets via `{{ includeTemplate }}`. The single source for AI
Instructions.

**Core**:
The complete body of AI Instructions, authored once and shared by *every* tool
including Claude. There is no tool-specific instruction layer — rules that look
Claude-flavored (worktree paths, `~/.claude/rules/` examples) are incidental
references to be generalized, not a separate Claude-only partial. The only thing
unique to Claude is its larger *config* surface (settings, keybindings, agents,
commands, skills), which is not instructions and is not part of the fan-out.
_Avoid_: "claude-mechanics" / "claude-specific rules" — that boundary does not
exist.
