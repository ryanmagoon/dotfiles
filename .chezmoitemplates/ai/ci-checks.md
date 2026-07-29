# CI Checks

- **After every code edit, before committing**, run the project's lint,
  typecheck, and format commands. Fix issues inline as you write, not in a
  cleanup pass at the end — this catches lint errors, type mismatches, and
  formatting drift before they become separate commits. (Check the project's
  own instructions / package manifest for the exact commands.)
- **Before creating a non-draft PR**, run lint, typecheck, format-check, and the
  full test suite and confirm they all pass. Fix any failures before opening the
  PR.
- **After resolving merge conflicts**, re-run typecheck and tests to verify
  nothing broke.
- **Before merging a PR**, verify all CI checks are passing. Do not merge until
  every check is green.
- **No decorative checks.** Any CI check that looks like a test gate must be
  able to fail the PR. If a check is intentionally advisory (e.g. a
  visual-regression tool with exit-zero-on-changes), it must not be the *only*
  automated coverage for that surface — pair it with a blocking equivalent.
  When auditing coverage, ask of each green check: "can this ever turn red?"
