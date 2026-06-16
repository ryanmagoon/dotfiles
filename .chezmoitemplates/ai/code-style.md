# Code Style

- Prefer descriptive variable names over abbreviations (e.g., `audioContext` not `ctx`, `sampleRate` not `sr`).
- Add TSDoc comments for functions whose purpose or parameters aren't immediately obvious from the name and types alone. Skip TSDoc for trivial or self-explanatory functions.
- **No `!` non-null assertions.** Use proper null checks, guard clauses, or restructure the code so TypeScript can narrow the type. In tests, write helper functions that throw descriptive errors instead of asserting with `!`.
- **No `as any` casts.** Use proper types, `as unknown as SpecificType` only when truly necessary (e.g., mocking), or restructure to avoid the cast. If you need to satisfy a complex type in a test mock, create a properly-typed helper.
- **`eslint-disable` and `@ts-ignore`/`@ts-expect-error` should almost never be used.** Fix the underlying issue instead. When a suppression is truly unavoidable, always include an explanation comment describing *why* it's necessary. If the justification involves a third-party limitation or known issue, include a hyperlink (e.g., GitHub issue, docs page) so future readers can verify it's still needed.
- **Deferred work must be tracked.** If you push code with a lint suppression, TODO comment, or any known shortcut that should be fixed later, create a GitHub issue for it and reference the issue URL in the code comment (e.g., `// TODO: fix XYZ — https://github.com/org/repo/issues/123`).
