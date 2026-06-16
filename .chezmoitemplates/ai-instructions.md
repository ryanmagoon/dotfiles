{{/*
  ai-instructions.md — the shared "core" body of AI coding instructions.

  Single source of truth, fanned out to every AI tool (Claude, Codex, Cursor,
  Grok, opencode, ...). Authored once here, included by each tool's Delivery
  Target. See AGENTS.md → "AI instruction fan-out" for the architecture and
  docs/adr/0001 for why.

  Composition: an explicit, ordered list of topic partials from
  .chezmoitemplates/ai/. Order is deliberate — edit it here, not by renaming
  files. There are NO tool-conditionals in this body; rules that look
  Claude-flavored have been generalized to read correctly for any tool.
*/ -}}
# Global AI coding instructions

These conventions apply to every project unless a project's own instructions say
otherwise. Keep them load-bearing — they are loaded into context on every run.

{{ includeTemplate "ai/coding-principles.md" . -}}

{{ includeTemplate "ai/commits.md" . -}}

{{ includeTemplate "ai/git-workflow.md" . -}}

{{ includeTemplate "ai/ci-checks.md" . -}}

{{ includeTemplate "ai/testing.md" . -}}

{{ includeTemplate "ai/code-style.md" . -}}

{{ includeTemplate "ai/error-handling.md" . -}}

{{ includeTemplate "ai/session-start.md" . -}}

{{ includeTemplate "ai/self-healing.md" . -}}

{{ includeTemplate "ai/context-management.md" . -}}
