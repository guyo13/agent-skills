# guyo13/agent-skills

My collection of Agent Skills.

# Branch Evaluator

An agent skill that evaluates, scores, and compares multiple git branches against a reference implementation plan. It recommends a winner and provides guidance on integrating the best parts of losing branches.

## Scoring Dimensions

| Dimension | Weight | Focus |
|-----------|--------|-------|
| Correctness | 45% | Requirement coverage, bug-free, edge cases |
| Test Quality | 30% | Coverage, assertion quality, test isolation |
| Code Quality | 25% | Readability, structure, idiomatic patterns |

## Install

```bash
npx skills add guyo13/agent-skills --skill branch-evaluator
```

Or install globally to specific agents:

```bash
npx skills add guyo13/agent-skills --skill branch-evaluator -g -a cursor antigravity claude-code gemini-cli
```

## Usage

Once installed, ask your agent to evaluate branches:

> Compare branches `feature/auth-alice` and `feature/auth-bob` against this implementation plan: [paste plan or provide file path]

The agent will:

1. Parse the plan into discrete requirements
2. Diff each branch against the base
3. Score each on Correctness, Test Quality, and Code Quality
4. Produce a structured report with a winner and integration recommendations

## License

BSD-3-Clause

