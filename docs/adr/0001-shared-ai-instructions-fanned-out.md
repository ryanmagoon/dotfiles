# Shared AI instructions, fanned out to every tool on every machine

The same coding conventions should govern every AI coding assistant (Claude
Code, Codex, Cursor, Grok, opencode) and stay trivially in sync. We author the
instructions **once** as a chezmoi shared template partial
(`.chezmoitemplates/ai-instructions.md`) and each tool gets a thin `.tmpl`
Delivery Target that `includeTemplate`s the partial plus any tool-specific
wrapper. All tools are co-equal — none is primary.

We apply **all** tool targets on **every** machine (blanket), with no
per-machine tool gating. Rationale: a tool's instruction file is just a few KB
of markdown that nothing reads until that tool is installed, so a "phantom"
file is harmless, and blanket apply means installing a tool on a new box finds
the instructions already in place. Considered machine-gating via chezmoi data
(`{{ if has "codex" .aiTools }}`) and rejected it as machinery without payoff
for plain markdown — to be revisited only if a target becomes behavior-changing
config (e.g. a settings.json) rather than instructions.
