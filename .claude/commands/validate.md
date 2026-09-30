---
description: Run the full validation pyramid (lint, types, tests, build, acceptance criteria) and report
argument-hint: [path-to-plan]
---

# Validate: Run the Validation Pyramid

Run every automated check the project has and verify the implementation against its plan. Report clearly, fix what is fixable, and never claim a pass you did not observe.

## Plan Reference

Plan file (optional): `$ARGUMENTS`

If a plan is given, read it fully. Its "Validation Commands" and "Acceptance Criteria" sections are the contract for this run.

## 1. Discover the Validation Commands

Do not guess. Find the real commands, in this order:

1. The plan's validation section, if provided
2. `CLAUDE.md` → "Validation" section
3. `package.json` scripts, `pyproject.toml`, `Makefile`, `justfile`, or equivalent
4. `README.md`

Typical commands to look for: lint, format check, type check, unit tests, integration tests, build.

## 2. Run Each Level in Order

Run every level that applies. Stop only if a level fails in a way that makes later levels meaningless (for example, the build is broken).

### Level 1: Syntax, Lint, Format
```bash
# examples: npm run lint / uv run ruff check . / bun lint
```

### Level 2: Type Checking
```bash
# examples: npx tsc --noEmit / uv run mypy . / uv run pyright
```

### Level 3: Unit and Integration Tests
```bash
# examples: npm run test:run / uv run pytest -v / bun test
```
Report pass and fail counts. If coverage is configured, report it.

### Level 4: Build
```bash
# examples: npm run build / uv run python -m build
```

### Level 5: Runtime Smoke Test (if applicable)
Start the app if it is not running, hit the main entry point and any health endpoints, then stop what you started. For UI-heavy features, use the e2e-test skill instead of doing this by hand.

## 3. Check Acceptance Criteria

If a plan was given, walk through each acceptance criterion and mark it:

- ✅ verified (say how: which test, command, or observation)
- ❌ not met (say what is missing)
- ⚠️ not verifiable automatically (needs manual check by the user)

## 4. Fix Loop

If a level fails:

1. Read the full error output
2. Fix the root cause, not the symptom
3. Re-run that level
4. Continue to the next level

Do not disable tests, lower coverage thresholds, or add ignore comments to make a check pass. If a fix would need one of those, stop and report it instead.

## 5. Report

```markdown
## Validation Report

| Level | Command | Result |
|-------|---------|--------|
| Lint | `...` | ✅ pass / ❌ fail |
| Types | `...` | ✅ / ❌ / ⏭ n/a |
| Tests | `...` | ✅ 42 passed / ❌ 2 failed |
| Build | `...` | ✅ / ❌ |
| Smoke | `...` | ✅ / ❌ / ⏭ n/a |

### Acceptance Criteria
- ✅ ...
- ❌ ...
- ⚠️ ...

### Fixes Applied
- file: what changed and why

### Open Issues
- anything still failing or needing manual verification

**Overall: PASS / FAIL**
```

Include the actual output of any failing command. A report that says "fail" with the error is more useful than one that hides it.
