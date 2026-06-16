# Testing

## Test-Driven Development

- **Write tests first.** Before writing implementation code, write failing tests that describe the expected behavior. Red-green-refactor: fail, pass, clean up.
- **Start with the unhappy path.** Write tests for edge cases, invalid inputs, error states, and boundary conditions *before* the happy path. The happy path is the easiest to get right and the least likely to be forgotten — the edges are where bugs hide.
- **Think from the user's perspective.** Tests should describe what a user (or caller) expects to happen, not how the internals work. Test behavior and outcomes, not implementation details. If you can refactor the internals without breaking tests, your tests are good.

## Coverage Philosophy

- **Think holistically about use cases.** Before writing any tests, enumerate the meaningful scenarios: What are the valid inputs? Invalid inputs? Empty states? Concurrent access? Permission failures? Timeouts? Don't just test the one case that prompted the feature — think about the full surface area.
- **Prioritize test cases by risk.** Not all paths need equal coverage. Focus on: things that would be embarrassing or costly to break, boundary conditions where off-by-one or type errors lurk, and integration points where two systems meet.
- **Each test should justify its existence.** If a test doesn't catch a class of bug that another test already covers, it's redundant. If it tests an internal detail that could change without affecting behavior, it's brittle. Write fewer, better tests.

## Workflow

- **Automated tests are required.** Write tests for new functionality before committing. Run the full test suite and confirm it passes before every commit.
- **Manual testing is required.** Before committing, verify the change actually works — build the project, run it, and confirm the feature/fix behaves correctly. Do not commit untested code.
- **Test environments:** Prefer real DOM when possible. If a simulated DOM is needed, use happy-dom over jsdom.
- **Colocate by feature, not by file type.** Tests should live alongside the code they test (e.g., `useGamepad.test.ts` next to `useGamepad.ts`) when it makes logical sense, rather than in a separate top-level `__tests__` directory.
