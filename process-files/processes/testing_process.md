# Testing and Development Process

STARTER_CHARACTER = 🚀

This project uses a test-first, incremental workflow. Use this process for all new functionality and refactors.

## Principles

1. **Write tests before production code**
   - Define or extend tests that express the desired behaviour and edge cases.
   - Only then implement or change production code.

2. **Align on test cases first**
   - Discuss and agree test scenarios (including edge cases) before implementation.
   - Treat tests as the primary executable specification.

3. **Keep the project spec up to date**
   - Whenever we clarify behaviour or edge cases, update `docs/project_spec.md`.
   - The spec and tests should tell the same story.

4. **Make tests fail meaningfully first**
   - Prefer failures where the code is implemented but *wrong* (e.g. incorrect return values), so the failing assertions describe behavioural gaps rather than "not implemented" errors.
   - Avoid using `throw new Error('Not implemented yet')` in normal workflow; favour a simple, plausible implementation that does *something* and then refine it until tests pass.

5. **Work in small, focused steps**
   - Change one small, coherent piece of behaviour at a time.
   - Keep each test or code change narrowly scoped and immediately verifiable.

6. **Back-end logic before UI wiring**
   - Implement and test TypeScript domain / processing logic first (pure functions, processors, etc.).
   - Only then connect that logic to the UI (DOM wiring, event handlers, Cypress tests).

7. **Avoid magic literals in tests**
   - When a value is used repeatedly in a test suite (e.g. a date string, label, or message fragment), define it once as a `const` and reuse it.
   - This reduces duplication and keeps tests readable and maintainable.

8. **Avoid brittle implementation-detail expectations**
   - Prefer assertions about *observable behaviour* and final outputs (returned content, DOM text, visible state).
   - Only assert on implementation details (like mock call counts) when the *behaviour* depends on them.
   - **Example**: When processing timestamp lines, it is appropriate to assert how `selectDateCallback` is called (with default dates) because the defaulting behaviour is part of the feature. By contrast, asserting that a helper function `formatLine()` was called exactly once would be brittle and unnecessary if you can already assert the final formatted content.

9. **Split tests by concern**
   - Place each logical group of tests in a separate file, even if the source logic is in a single file.
   - Typical pattern:
     - One test file per top-level `describe` block or major method group.
     - Example: `reply_thread_processor.findNextReplyThread.test.ts`, `reply_thread_processor.processAllReplyThreads.test.ts`, etc.

## Recommended Workflow for a New Feature

1. **Update the spec**
   - Describe the new behaviour in `docs/project_spec.md`.
   - Include edge cases and any user-visible messages.

2. **Design and agree tests**
   - Add or extend Jest tests for the core processing logic.
   - Add or extend Cypress tests for UI behaviour and full flows.

3. **Introduce a failing test**
   - Run the tests to confirm the new ones fail in the expected way.
   - Prefer test failures caused by incorrect behaviour (e.g. wrong return values or DOM state) rather than by deliberate `not implemented` exceptions.

4. **Implement core logic**
   - Implement the back-end / processing TypeScript to satisfy the Jest tests.
   - Iterate until all relevant Jest suites pass.

5. **Wire up the UI**
   - Connect the new logic to DOM elements and events.
   - Adjust Cypress tests as needed to assert on the user-visible behaviour.

6. **Refine and tidy**
   - Extract repeated literals in tests into constants.
   - Split or reorganize tests into focused files if they become large.
   - Re-run all tests (Jest and Cypress) to confirm the full suite is green.
