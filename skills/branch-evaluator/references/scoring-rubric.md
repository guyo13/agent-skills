# Scoring Rubric

Detailed rubric for evaluating branches. Each dimension is scored 0--10.

## Default Weights

| Dimension | Weight | Rationale |
|-----------|--------|-----------|
| Correctness | 45% | Working software is the primary measure of progress |
| Test Quality | 30% | Tests protect correctness over time |
| Code Quality | 25% | Maintainability matters but is secondary to function |

Users may override weights. If they do, recompute all weighted totals accordingly.

---

## Correctness (0--10)

Does the branch implement what the plan requires, and does it work?

| Score | Band | Criteria |
|-------|------|----------|
| 9--10 | Excellent | All plan requirements implemented. Edge cases handled. No observable bugs. Error paths covered gracefully. |
| 7--8 | Good | Most requirements implemented (>80%). Minor edge cases missed. No critical bugs. |
| 5--6 | Average | Core happy path works (~60% of requirements). Some edge cases or error paths missing. Minor bugs present. |
| 3--4 | Below Average | Partial implementation (<50% of requirements). Several bugs. Missing error handling. |
| 0--2 | Poor | Barely started, fundamentally broken, or implements the wrong thing. |

### What to look for

- **Requirement coverage**: Check each plan requirement against the diff. Mark PASS/FAIL.
- **Bug indicators**: Off-by-one errors, null/undefined access, race conditions, missing input validation.
- **Error handling**: Graceful failures, meaningful error messages, no swallowed exceptions.
- **Data integrity**: Correct data transformations, consistent state management, proper transaction handling.

---

## Test Quality (0--10)

How well do the tests validate the implementation?

| Score | Band | Criteria |
|-------|------|----------|
| 9--10 | Excellent | Comprehensive coverage of happy paths, edge cases, and error paths. Assertions are specific and meaningful. Tests are deterministic and isolated. Both unit and integration tests present where appropriate. |
| 7--8 | Good | Good coverage of happy paths and most edge cases. Assertions verify behavior, not just absence of errors. Tests are reliable. |
| 5--6 | Average | Happy path covered. Some edge cases tested. Assertions exist but may be shallow (e.g. only checking status codes, not response bodies). |
| 3--4 | Below Average | Minimal tests. Only sunny-day scenarios. Vague assertions. Tests may be flaky or coupled to implementation details. |
| 0--2 | Poor | No tests, or tests that don't actually verify anything (e.g. `expect(true).toBe(true)`). |

### What to look for

- **Coverage breadth**: Are all public interfaces tested? All significant branches (if/else)?
- **Edge cases**: Empty inputs, boundary values, concurrent access, large payloads, malformed data.
- **Assertion quality**: Do assertions check the right thing? Specific field values vs. just "no error thrown."
- **Test isolation**: No shared mutable state. No order dependency. Proper setup/teardown.
- **Test naming**: Descriptive names that document expected behavior.

---

## Code Quality (0--10)

Is the code readable, maintainable, and idiomatic?

| Score | Band | Criteria |
|-------|------|----------|
| 9--10 | Excellent | Clean, idiomatic code. Well-structured modules with clear responsibilities. Minimal duplication. Appropriate abstractions. Consistent style. |
| 7--8 | Good | Readable and well-organized. Minor style inconsistencies. Abstractions are reasonable. Low duplication. |
| 5--6 | Average | Generally understandable but has some issues: inconsistent naming, moderate duplication, overly long functions, or unclear module boundaries. |
| 3--4 | Below Average | Hard to follow. Significant duplication. Poor naming. God functions/classes. Mixed concerns. |
| 0--2 | Poor | Unreadable, deeply nested, copy-pasted blocks, no discernible structure. |

### What to look for

- **Naming**: Variables, functions, and files have clear, consistent names.
- **Structure**: Logical file/module organization. Single responsibility. Appropriate layering.
- **Duplication**: DRY without over-abstracting. Repeated blocks of 5+ lines are a red flag.
- **Idioms**: Uses language/framework conventions (e.g. Go error handling, React hooks, Python context managers).
- **Complexity**: Cyclomatic complexity is manageable. No deeply nested conditionals (>3 levels).
- **Documentation**: Non-obvious decisions are commented. Public APIs have docstrings where expected by the ecosystem.

---

## Scoring Guidelines

- **Be consistent across branches.** The same gap should produce the same deduction regardless of which branch it appears in.
- **Anchor to the plan.** Scores are relative to the reference implementation plan, not to an idealized solution.
- **Justify every score.** Each score must have a 2--3 sentence justification citing specific evidence from the diff.
- **Half points are acceptable.** Use X.5 scores when a branch sits clearly between two bands.
- **Tiebreaking order:** Correctness > Test Quality > Code Quality.


