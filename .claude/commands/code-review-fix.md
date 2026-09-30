---
description: Fix the issues found in a code review, then re-validate
argument-hint: [path-to-review-file or description of issues]
---

# Code Review Fix

Work through the findings of a code review and fix them one by one.

## Input

Review (file path or description): `$ARGUMENTS`

If this is a file path, read the entire file first so you understand all findings and how they relate before touching anything. If no argument is given, use the most recent file in `.agents/code-reviews/`.

## Process

Fix findings in severity order: critical, high, medium, low.

For each finding:

1. **Read the surrounding code** in full, not just the flagged line
2. **Explain what was wrong** in one or two sentences
3. **Fix the root cause.** If the finding was a symptom of a deeper problem, fix the deeper problem and say so
4. **Add or update a test** that would have caught it, where a test makes sense
5. **Run that test** and confirm it passes

If you disagree with a finding, do not silently skip it. Say why and leave it for the user to decide.

## Rule Evolution

If a finding was caused by an unclear, missing, or wrong rule in `CLAUDE.md` or `.agents/reference/`, update the rule as part of the fix. Every bug the agent produces is a chance to fix the system that produced it.

## Finish

After all fixes:

1. Run `/validate` (lint, types, tests, build)
2. Report:

```markdown
## Fix Report

| # | Finding | Status |
|---|---------|--------|
| 1 | ... | ✅ fixed / ⏭ skipped (reason) |

### Rules updated
- file: what changed

### Validation
PASS / FAIL with details
```
