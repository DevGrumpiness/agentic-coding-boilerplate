---
description: Prime agent with codebase understanding
---

# Prime: Load Project Context

## Objective

Bootstrap context at session start. Build a mental model of the project before doing any work: what it is, how it is organized, what conventions apply, and what changed recently.

Optional focus: `$ARGUMENTS` (e.g. "frontend", "api", "auth"). If given, go deeper on that area and load only the reference docs relevant to it.

## Process

### 1. Analyze Project Structure

List all tracked files:
!`git ls-files`

Show directory structure:
On Linux/macOS, run: `tree -L 3 -I 'node_modules|__pycache__|.git|dist|build'`

### 2. Read Core Documentation

- Read `PRD.md` (or `.claude/PRD.md`) if it exists
- Read `CLAUDE.md` or similar global rules file
- Read README files at project root and major directories
- Read any architecture documentation
- Read database schema/migration files if applicable

### 3. Load On-Demand Reference Docs

List what exists in `.agents/reference/`. Then:

- If a focus area was given, read only the docs relevant to it
- If no focus was given, read the docs' headings and first paragraph to know what each covers, and read fully only the ones central to the project

Do not dump every reference doc into context. The point of splitting them is that they are loaded when needed.

Also check `.agents/plans/` for the most recent plan and skim it. It tells you what is currently being worked on.

### 4. Identify Key Files

Based on the structure, identify and read:
- Main entry points (main.py, index.ts, app.py, etc.)
- Core configuration files (pyproject.toml, package.json, tsconfig.json)
- Key model/schema definitions
- Important service or controller files

For large codebases, delegate this exploration to subagents that return a summary. Keep the main context lean.

### 5. Understand Current State

Check recent activity:
!`git log -10 --oneline`

Check current branch and status:
!`git status`

## Output Report

Provide a concise summary covering:

### Project Overview
- Purpose and type of application
- Primary technologies and frameworks
- Current version/state

### Architecture
- Overall structure and organization
- Key architectural patterns identified
- Important directories and their purposes

### Tech Stack
- Languages and versions
- Frameworks and major libraries
- Build tools and package managers
- Testing frameworks

### Core Principles
- Code style and conventions observed
- Documentation standards
- Testing approach

### Current State
- Active branch
- Recent changes or development focus
- Most recent plan in `.agents/plans/`, if any
- Any immediate observations or concerns

### Reference Docs Available
- Which docs exist in `.agents/reference/` and when to load each

**Make this summary easy to scan - use bullet points and clear headers.** Then stop and wait for instructions. Priming is context loading, not a license to start changing things.
