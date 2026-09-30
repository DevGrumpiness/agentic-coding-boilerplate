Create a new commit for the current logical unit of work.

1. Inspect the working tree:
   - run `git status`
   - run `git diff HEAD`
   - run `git status --porcelain`

2. Determine which changed and untracked files belong to the current logical unit of work.

3. Before staging:
   - do not include unrelated changes
   - do not commit secrets, credentials, `.env`, generated artifacts, or other files that should be ignored

4. Stage all files that belong to this logical unit of work.

5. Review the staged commit:
   - run `git diff --cached`
   - run `git status`

6. Create one atomic commit.

Use a concise Conventional Commit style message such as:

- `feat: ...`
- `fix: ...`
- `docs: ...`
- `refactor: ...`
- `test: ...`
- `chore: ...`

Choose the tag that best reflects the actual change.

Do not push.