---
description: Technical code review of uncommitted changes against project rules, saved as a report
argument-hint: [optional: branch, commit range, or path to review instead of the working tree]
---

# Code Review

Review recently changed code for real bugs, security issues, and violations of this project's documented conventions. The output is a written report the user can act on or hand to `/code-review-fix`.

## Scope

Target: `$ARGUMENTS` (default: all uncommitted changes in the working tree)

## Core Principles

- Simplicity is the ultimate sophistication. Every line should justify its existence.
- Code is read far more often than it is written. Optimize for readability.
- Focus on real bugs, not style. Linting handles style.
- Be specific: file and line, not vague complaints. Suggest fixes, don't just complain.

## 1. Load the Standards

The review is only as good as the standard it reviews against. Read, if they exist:

- `CLAUDE.md` (project rules)
- Every file in `.agents/reference/` relevant to the changed area
- The plan in `.agents/plans/` this change implements, if there is one
- `README.md`

These define what "correct" means for this project. Rules found here outrank general best practices.

## 2. Gather the Changes

```bash
git status
git diff --stat HEAD
git diff HEAD
git ls-files --others --exclude-standard
```

If a target was given, adapt: `git diff main...branch`, `git diff <range>`, or review the given path.

Read every changed and new file **in its entirety**, not just the diff. Bugs often live in how a change interacts with the unchanged code around it.

## 3. Analyze

For each changed or new file, look for:

1. **Logic errors**: off-by-one, wrong conditionals, missing error handling, race conditions, unhandled null/undefined
2. **Security**: injection, XSS, insecure data handling, exposed secrets, missing auth checks, unvalidated input at trust boundaries
3. **Performance**: N+1 queries, needless recomputation, unbounded growth, blocking calls in hot paths
4. **Correctness vs. the plan**: does the change actually do what the plan or acceptance criteria say? Anything skipped or silently narrowed?
5. **Adherence to project rules**: naming, file organization, error handling, logging and testing patterns as documented in CLAUDE.md and `.agents/reference/`
6. **Test quality**: are new tests real assertions or placeholders? Do they cover the edge cases the plan named?

## 4. Verify Before Reporting

Do not report guesses as findings.

- Run the specific test that would expose a suspected bug
- Confirm a type error by running the type checker
- Trace a suspected security issue to a concrete input that triggers it

Drop anything you could not confirm, or mark it clearly as "unverified".

## 5. Write the Report

Save to `.agents/code-reviews/{yyyy-mm-dd}-{short-description}.md`. Create the directory if needed.

```markdown
# Code Review: {short description}

**Date:** {date}
**Scope:** {what was reviewed}

## Stats
- Files modified: N
- Files added: N
- Files deleted: N
- Lines added / removed: +N / -N

## Findings

### 1. {one-line title}
- **Severity:** critical | high | medium | low
- **File:** path/to/file:line
- **Issue:** what is wrong
- **Why it matters:** consequence for a concrete input or situation
- **Suggestion:** how to fix
- **Verified:** yes (how) | unverified

...

## Rule Violations
Findings that are specifically breaches of CLAUDE.md or `.agents/reference/` rules. If a rule was broken because the rule is unclear or missing, say so. That is a signal to update the rule (system evolution).

## Summary
Overall: PASS | PASS WITH NOTES | NEEDS FIXES
```

If nothing was found: "Code review passed. No technical issues detected." Still write the report with stats.

## 6. Output to the User

Print the report path and a short summary ordered by severity. Security findings are always critical.
