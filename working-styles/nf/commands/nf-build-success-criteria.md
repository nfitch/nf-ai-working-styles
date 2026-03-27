You are about to check in code. STOP. Verify every criterion below before committing.

This is not a suggestion. This is a gate. Do not commit until all four criteria are met.

---

## Criterion 1: Documentation is up to date and consistent

- Every file you touched or created: does the relevant documentation reflect it?
- Design docs, READMEs, component indexes, field playbooks -- all consistent with code changes.
- If you added a reusable component, hook, or utility: is it in the reusable-components-index?
- If you changed a schema or field: does the field-change-playbook trail match?
- No stale references to removed code, renamed fields, or deleted files.

## Criterion 2: ALL tests pass

- Run the targeted test suites for every package you touched.
- ALL tests must pass. Not just yours. ALL of them.
- If you find a pre-existing failure that "isn't yours" -- fix it anyway. You own the green bar. There is no "someone else's problem" for test failures. FIX ALL THE TESTS ALWAYS.
- If a test is failing because of a real bug, fix the bug. If the test is wrong, fix the test. Either way, green.
- Capture test output to `./tmp/` and verify. Do not eyeball scrolling terminal output.

## Criterion 3: All imports and paths follow project conventions

- Read `design/technical/path-conventions.md` in the project root. That is the spec.
- Shared library imports use `@lib` alias. NEVER relative paths like `../../lib/` or `../../../lib/`.
- Frontend API fetches from `/app/` context use `../api/` (relative, never absolute `/api/`).
- No absolute paths in frontend code.
- Every new package: tsconfig `paths` configured, vitest `resolve.alias` configured if applicable.
- Grep your changes for violations before committing.

## Criterion 4: Schema, API, and UI changes reviewed by human

- If you changed database schema, API endpoints, or user-facing UI behavior: the human must review and approve before commit.
- Do not silently commit schema migrations, new API routes, or UX changes.
- Show the human what changed and get explicit approval.
- Code-only refactors and test additions do not require this gate.

---

OUTPUT FORMAT: One table, then one line.

TABLE: Build Success Criteria
- Columns: # | Criterion | Status (PASS/FAIL) | Evidence
- Evidence = specific commands run, files checked, test output location

Final line: "All criteria met -- ready to commit" or "BLOCKED: [list failed criteria]"

Do NOT write prose. Do NOT summarize changes. Just the table and the verdict.

---

## Final Step: Close-out

After all four criteria pass:

1. Run `nf-check-work` to confirm the user is good to close things out.
2. Meticulously review every checklist item from the implementation plan or task list. Do not skim. Read each item and verify it was actually completed -- not "probably done" or "I think I did that." Actually check.
3. Go back and check off every box. If a box cannot be checked, explain why and resolve it before closing.
