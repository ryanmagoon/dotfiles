# Code Style

- Prefer descriptive names over abbreviations (e.g., `audioContext` not `ctx`,
  `sampleRate` not `sr`).
- Document functions whose purpose or parameters aren't obvious from the name and
  signature alone, using the language's documentation convention (TSDoc,
  docstrings, doc comments, …). Skip docs for trivial or self-explanatory
  functions.
- **Avoid unsafe escapes from the type system.** Don't reach for non-null
  assertions, force-casts, or `any`-equivalents to silence the type checker —
  use proper checks, guard clauses, narrowing, or precise types instead. In
  tests, write typed helpers that throw descriptive errors rather than asserting
  your way past the types.
- **Avoid suppressing the linter or type checker.** Suppressions
  (`eslint-disable`, `@ts-ignore`/`@ts-expect-error`, `# noqa`, `//nolint`, …)
  should almost never be used — fix the underlying issue. When a suppression is
  truly unavoidable, include a comment explaining *why*, with a hyperlink (issue,
  docs) if the justification is a third-party limitation, so future readers can
  verify it's still needed.
- **Deferred work must be tracked.** If you ship a suppression, TODO, or any
  known shortcut to fix later, create a tracking issue and reference its URL in
  the comment (e.g., `// TODO: fix XYZ — https://github.com/org/repo/issues/123`).
