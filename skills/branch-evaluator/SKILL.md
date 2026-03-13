---
name: branch-evaluator
description: Evaluate, score, and compare multiple git branches against a reference implementation plan. Recommends a winner and provides integration guidance from other branches. Use when the user asks to compare branches, evaluate implementations, pick the best branch, or review competing implementations.
metadata:
  author: branch-evaluator
  version: "1.0.0"
---

# Branch Evaluator

Evaluate multiple git branches implementing the same feature against a reference plan. Score each on Correctness, Test Quality, and Code Quality, then recommend a winner with integration guidance.

## Inputs

Collect the following from the user before starting:

1. **Reference implementation plan** -- inline text, a file path, or a URL describing the intended feature/workload.
2. **Branch list** -- two or more branch names to evaluate (e.g. `feature/auth-alice`, `feature/auth-bob`).
3. **Base branch** (optional) -- the branch all candidates diverged from. Defaults to `main`.

If any input is missing, ask the user before proceeding.

## Evaluation Workflow

### Phase 1: Setup

1. Confirm the repository is a git repo and the working tree is clean (stash or warn if dirty).
2. Verify the base branch exists locally; fetch if needed:
   ```bash
   git fetch origin
   git branch -a
   ```
3. Verify every candidate branch exists (local or remote). Abort with a clear message if any are missing.
4. Capture the merge-base for each candidate:
   ```bash
   git merge-base <base-branch> <candidate-branch>
   ```

### Phase 2: Plan Analysis

Parse the reference implementation plan into a checklist of discrete **requirements**. Each requirement should be a single testable statement. Present the checklist to the user in the report and use it as the evaluation backbone.

Example decomposition:
- R1: "User can sign up with email and password"
- R2: "Passwords are hashed with bcrypt before storage"
- R3: "Duplicate email returns 409 Conflict"

### Phase 3: Branch Review

For **each** candidate branch, perform the following:

#### 3a. Diff Analysis

```bash
git diff <base-branch>...<candidate-branch> --stat
git diff <base-branch>...<candidate-branch>
```

Read the full diff carefully. Also check out the branch and read key files when the diff alone is insufficient:

```bash
git show <candidate-branch>:<path/to/file>
```

#### 3b. Test Inspection

Identify all test files added or modified. Look for:
- Test runner configuration (jest, pytest, vitest, go test, etc.)
- Number and scope of test cases
- Whether tests actually run and pass (run the test suite if feasible):
  ```bash
  git stash && git checkout <candidate-branch>
  # run the project's test command
  git checkout - && git stash pop
  ```

#### 3c. Scoring

Score each branch on three dimensions (0--10 each). Consult the detailed rubric in [references/scoring-rubric.md](references/scoring-rubric.md) before assigning scores.

| Dimension | Weight | What to evaluate |
|-----------|--------|-----------------|
| **Correctness** | 45% | Implements all plan requirements, handles edge cases, no obvious bugs |
| **Test Quality** | 30% | Coverage breadth, edge-case tests, assertion quality, test reliability |
| **Code Quality** | 25% | Readability, maintainability, idiomatic patterns, minimal duplication |

**Weighted total** = `(Correctness * 0.45) + (Test Quality * 0.30) + (Code Quality * 0.25)`

Provide a brief justification (2--3 sentences) for each dimension score.

### Phase 4: Comparison

Build a side-by-side comparison matrix. Note each branch's relative strengths and weaknesses. Identify areas where a losing branch outperforms the winner.

### Phase 5: Recommendation

1. **Declare a winner** -- the branch with the highest weighted total. If scores are within 0.5 points, declare a tie and recommend the branch with higher Correctness.
2. **Integration suggestions** -- for each non-winning branch, list specific improvements worth cherry-picking into the winner:
   - Name the file(s) and describe the change.
   - Explain *why* it is worth integrating.
   - Suggest *how* to integrate (cherry-pick commit, manual merge of specific functions, copy test cases, etc.).
3. If no non-winning branch has anything worth integrating, state that explicitly.

## Output Format

Structure the final report exactly as follows:

```markdown
# Branch Evaluation Report

## Executive Summary

**Winner: `<branch-name>`** with a weighted score of **X.XX / 10**.

<1--2 sentence justification>

## Requirements Checklist

| # | Requirement | branch-A | branch-B | ... |
|---|------------|----------|----------|-----|
| R1 | description | PASS/FAIL | PASS/FAIL | ... |

## Branch Scorecards

### `<branch-name>`

| Dimension | Score | Justification |
|-----------|-------|--------------|
| Correctness | X/10 | ... |
| Test Quality | X/10 | ... |
| Code Quality | X/10 | ... |
| **Weighted Total** | **X.XX/10** | |

(repeat for each branch)

## Comparison Matrix

| Dimension | branch-A | branch-B | ... |
|-----------|----------|----------|-----|
| Correctness | X | X | ... |
| Test Quality | X | X | ... |
| Code Quality | X | X | ... |
| **Weighted Total** | **X.XX** | **X.XX** | ... |

## Integration Recommendations

### From `<losing-branch>` into `<winner>`

- **<file or change>**: <what and why to integrate>
  - How: <cherry-pick / manual merge / copy>

(repeat for each losing branch with worthwhile changes, or state "No additional integrations recommended.")
```

## Edge Cases

- **Single branch**: Skip comparison/integration phases; just produce a scorecard.
- **All branches fail most requirements**: Still pick the best and note that substantial work remains.
- **Tie**: Prefer the branch with higher Correctness. If still tied, prefer higher Test Quality.
- **Cannot run tests**: Score Test Quality based on static analysis of test code and note that tests were not executed.


