# CI Checks

- **After every code edit, before committing**, run `pnpm lint && pnpm typecheck && pnpm format`. Fix issues inline as you write, not in a cleanup pass at the end. This catches lint errors, type mismatches, and formatting drift before they become separate commits.
- **Before creating a non-draft PR**, run linting (`lint`), typechecking (`typecheck`), formatting (`format:check`), and the full test suite (`test`) and confirm they all pass. Fix any failures before opening the PR.
- **After resolving merge conflicts**, run typechecking and tests to verify nothing broke.
- **Before merging a PR**, verify all CI checks are passing. Do not merge until every check is green.
