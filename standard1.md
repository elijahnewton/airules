
# AI Coding Rules

Priority: correctness/security > project conventions > tests/CI > readability > style. Break a rule only with documented reason.

## Workflow
- No direct pushes to protected `main`/`develop`. Feature branch -> PR to `develop` if it exists, else `main`.
- Keep PRs small, focused, single-purpose. Never commit secrets, debug prints, dead code, commented-out code, or fabricated results.
- Run formatter, linter, typecheck, tests locally before PR. CI is final gate.
- Never merge, force-push shared branches, or bypass CI.

## Design
- Single responsibility. One abstraction level per function. Small functions, 0–3 params (else param object).
- Prefer guard clauses/early returns. Max 2–3 nesting levels. (if avoidable , avoid nesting if statements)
- DRY after rule of three. YAGNI. Simpler beats clever. Avoid god classes, deep inheritance, premature abstraction.

## Naming & Comments
- Names reveal intent. Rename instead of explaining via comment.
- Comments explain why, constraints, invariants, tradeoffs, links. Public APIs may document contracts/exceptions.
- Delete commented-out code.

## Errors & Data
- Handle errors explicitly. No silent swallowing, magic returns, sentinel errors.
- Avoid null as control flow. Prefer empty collections, Optional/Result, explicit error types where idiomatic.
- Validate at boundaries. Add context when propagating errors.

## Tests
- Behavior changes need tests; bug fixes need regression tests.
- Tests: fast, independent, repeatable, self-verifying, deterministic. No shared state/order dependence.
- Keep tests clean. Run locally before PR.

## AI-Specific
- Read before writing. Match existing style/patterns. Don't rewrite unrelated code.
- Don't invent APIs/libraries/configs/commands. Verify.
- Keep diffs small and single-concern.
- If ambiguous, state assumptions or ask.
- Prefer boring, maintainable solutions.
