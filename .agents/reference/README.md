# On-Demand Reference Docs

Modular rules split by concern. `CLAUDE.md` stays short and points here; the agent loads only the file relevant to the current task.

Examples of files that belong here once your project has conventions worth capturing:

- `components.md` – frontend component patterns, styling rules, state management
- `api.md` – endpoint conventions, request/response shapes, error handling
- `database.md` – schema conventions, migration workflow, query patterns
- `testing.md` – test structure, fixtures, what to mock, coverage expectations
- `deployment.md` – build, environments, release process

How it gets used:

- `/create-rules` adds an "On-Demand Context" table to `CLAUDE.md` linking these files.
- `/plan-feature` reads the relevant ones while researching and lists them under "Mandatory Reading" in the plan.
- `/prime` reads them to build a mental model of the codebase.

Keep each file focused on one concern and update it whenever the agent makes a mistake that a better rule would have prevented.
