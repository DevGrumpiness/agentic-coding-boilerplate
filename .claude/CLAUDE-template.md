# CLAUDE.md Template

A flexible template for creating global rules. Adapt sections based on your project type.

---

# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

<!-- What is this project? One paragraph description -->

{Project description and purpose}

---

## Tech Stack

<!-- List technologies used. Add/remove rows as needed -->

| Technology | Purpose |
|------------|---------|
| {tech} | {why it's used} |

---

## Commands

<!-- Common commands for this project. Adjust based on your package manager and setup -->

```bash
# Development
{dev-command}

# Build
{build-command}

# Test
{test-command}

# Lint
{lint-command}
```

---

## Project Structure

<!-- Describe your folder organization. This varies greatly by project type -->

```
{root}/
├── {dir}/     # {description}
├── {dir}/     # {description}
└── {dir}/     # {description}
```

---

## Architecture

<!-- Describe how the code is organized. Examples:
- Layered (routes → services → data)
- Component-based (features as self-contained modules)
- MVC pattern
- Event-driven
- etc.
-->

{Describe the architectural approach and data flow}

---

## Code Patterns

<!-- Key patterns and conventions used in this codebase -->

### Naming Conventions
- {convention}

### File Organization
- {pattern}

### Error Handling
- {approach}

---

## Testing

<!-- How to test and what patterns to follow -->

- **Run tests**: `{test-command}`
- **Test location**: `{test-directory}`
- **Pattern**: {describe test approach}

---

## Validation

<!-- Commands to run before committing -->

```bash
{validation-commands}
```

---

## Key Files

<!-- Important files to know about -->

| File | Purpose |
|------|---------|
| `{path}` | {description} |

---

## On-Demand Context

<!-- Keep this file short. Put detailed, concern-specific rules in .agents/reference/
     and list them here. Load a reference doc only when working on that concern. -->

| Topic | File | Load when |
|-------|------|-----------|
| {e.g. Components} | `.agents/reference/{components.md}` | {working on frontend UI} |
| {e.g. API} | `.agents/reference/{api.md}` | {adding or changing endpoints} |
| {e.g. Database} | `.agents/reference/{database.md}` | {touching schema or queries} |

---

## Notes

<!-- Any special instructions, constraints, or gotchas -->

- {note}