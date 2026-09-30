---
description: Create global rules (CLAUDE.md) from codebase analysis
---

# Create Global Rules

Generate a CLAUDE.md file by analyzing the codebase and extracting patterns.

---

## Objective

Create project-specific global rules that give Claude context about:
- What this project is
- Technologies used
- How the code is organized
- Patterns and conventions to follow
- How to build, test, and validate

---

## Phase 1: DISCOVER

### Identify Project Type

First, determine what kind of project this is:

| Type | Indicators |
|------|------------|
| Web App (Full-stack) | Separate client/server dirs, API routes |
| Web App (Frontend) | React/Vue/Svelte, no server code |
| API/Backend | Express/Fastify/etc, no frontend |
| Library/Package | `main`/`exports` in package.json, publishable |
| CLI Tool | `bin` in package.json, command-line interface |
| Monorepo | Multiple packages, workspaces config |
| Script/Automation | Standalone scripts, task-focused |

### Analyze Configuration

Look at root configuration files:

```
package.json       → dependencies, scripts, type
tsconfig.json      → TypeScript settings
vite.config.*      → Build tool
*.config.js/ts     → Various tool configs
```

### Map Directory Structure

Explore the codebase to understand organization:
- Where does source code live?
- Where are tests?
- Any shared code?
- Configuration locations?

---

## Phase 2: ANALYZE

### Extract Tech Stack

From package.json and config files, identify:
- Runtime/Language (Node, Bun, Deno, browser)
- Framework(s)
- Database (if any)
- Testing tools
- Build tools
- Linting/formatting

### Identify Patterns

Study existing code for:
- **Naming**: How are files, functions, classes named?
- **Structure**: How is code organized within files?
- **Errors**: How are errors created and handled?
- **Types**: How are types/interfaces defined?
- **Tests**: How are tests structured?

### Find Key Files

Identify files that are important to understand:
- Entry points
- Configuration
- Core business logic
- Shared utilities
- Type definitions

---

## Phase 3: GENERATE

### Create CLAUDE.md

Use the template at `.claude/CLAUDE-template.md` as a starting point.

**Output path**: `CLAUDE.md` (project root)

**Adapt to the project:**
- Remove sections that don't apply
- Add sections specific to this project type
- Keep it concise - focus on what's useful

**Key sections to include:**

1. **Project Overview** - What is this and what does it do?
2. **Tech Stack** - What technologies are used?
3. **Commands** - How to dev, build, test, lint?
4. **Structure** - How is the code organized?
5. **Patterns** - What conventions should be followed?
6. **Key Files** - What files are important to know?

**Optional sections (add if relevant):**
- Architecture (for complex apps)
- API endpoints (for backends)
- Component patterns (for frontends)
- Database patterns (if using a DB)

### Create On-Demand Reference Docs

Keep `CLAUDE.md` short. Detailed, concern-specific rules go into `.agents/reference/` so the agent loads only what the current task needs.

**Identify concerns.** From the analysis, pick the 2-4 areas of the codebase that have enough distinct conventions to deserve their own doc. Typical splits:

| Concern | Example file | Covers |
|---------|--------------|--------|
| Frontend | `components.md` | component structure, styling, state management, UI libraries |
| Backend / API | `api.md` | routes or server actions, validation, auth, error responses |
| Data | `database.md` | schema conventions, migrations, query patterns, ORM usage |
| Testing | `testing.md` | test layout, fixtures, mocking, what must be covered |
| Tooling / agents | `tools.md` | tool definitions, prompts, agent patterns (for AI projects) |

Only create a doc for a concern that actually exists in the codebase. A backend-only project gets no `components.md`.

**Write each doc** to `.agents/reference/{concern}.md`, based on what the code actually does, not on generic best practices. Each doc should contain:

- Where this concern lives (directories, key files)
- The patterns to follow, each with a short real example from the codebase
- Conventions for naming, errors, logging, and types specific to this area
- Common mistakes to avoid (from inconsistencies you spotted during analysis)
- How to test code in this area

Keep each doc focused and scannable. If a doc already exists, update it rather than overwriting it.

**Link them from `CLAUDE.md`** in the "On-Demand Context" section, with a description of when to load each one:

```markdown
## On-Demand Context

Read these reference docs when working in the relevant area:

| When working on... | Read |
|--------------------|------|
| Frontend components, UI, styling, state management | `.agents/reference/components.md` |
| API routes, database queries, validation, auth | `.agents/reference/api.md` |
| Product requirements, feature specs, schema design | `PRD.md` |
```

Include the PRD in this table if one exists (check `PRD.md` and `.claude/PRD.md`).

---

## Phase 4: OUTPUT

```markdown
## Global Rules Created

**File**: `CLAUDE.md`

### Reference Docs Created

- `.agents/reference/{file}.md` - {what it covers}

### Project Type

{Detected project type}

### Tech Stack Summary

{Key technologies detected}

### Structure

{Brief structure overview}

### Next Steps

1. Review the generated `CLAUDE.md` and the reference docs
2. Add any project-specific notes
3. Remove any sections that don't apply
4. Re-run `/create-rules` after major changes, or update the docs by hand whenever the agent makes a mistake a clearer rule would have prevented
```

---

## Tips

- Keep CLAUDE.md focused and scannable
- Don't duplicate information that's in other docs (link instead)
- Focus on patterns and conventions, not exhaustive documentation
- Update it as the project evolves