---
name: skill-router
description: >
  Skill router — central index pointing to the skills and documents
  relevant to the project. ALWAYS use it at the start of a task to
  know what to read. Contains no technical information — only points
  to the correct documents for the task at hand.

  IMPORTANT: Read only what you need, when you need it.
---

# Skill Router — {{PROJECT_NAME}}

## What is a router

The router is the single entry point to the project's operational
documentation. Instead of remembering which file contains what,
you check this table and read only what you need for the current
task.

Only reference the skills that the project actually uses — not
every skill available.

## Active skills in this project

### Core (metaproject conventions)

*These rows apply to every project — keep them as is.*

| You need to... | Read... |
|---|---|
| Edit the todo list | `/home/claude/state/skills/core/edit-todo.md` |
| Write the session's handoff | `/home/claude/state/skills/core/write-handoff.md` |

### Imported

*Skills imported from an external collection — a set of skills
published and maintained elsewhere, with their own upstream (a
website, a repository). They live at the root of `skills/` (or in a
subdirectory, for a skill with a subrouter). The **Source** column
records where each one comes from: `update` uses it to synchronise
the skill with its upstream. List only the skills the project
actually uses. Remove this placeholder once real rows are added.*

| You need to... | Read... | Source |
|---|---|---|
| *(example row, replace with something real)* | `/home/claude/state/skills/some-skill.md` | *(collection URL)* |

### Personal

*Skills created by this project for itself. They live in
`skills/personal/` and do not travel from Metaproject. In
principle list them all here — they exist because the project
uses them. Remove a row only if a skill is inactive on purpose
(superseded, draft, kept for reference).*

| You need to... | Read... |
|---|---|
| *(example row, replace with something real)* | `/home/claude/state/skills/personal/some-skill.md` |

## State documents

| You need to... | Read... |
|---|---|
| Find your way around the project | `/home/claude/state/agents.md` |
| Project conventions | `/home/claude/state/conventions.md` |
| To-do list | `/home/claude/state/todo.md` |

## How to adapt this router

1. Keep the **Core** section as is — those skills apply to every
   project and should always be active.
2. Add a row to **Imported** for each skill imported from an
   external collection (any skill at the root or in a subdirectory
   other than `core/` and `personal/`), with its **Source**.
3. The **Personal** section lists, by default, every skill in
   `/home/claude/state/skills/personal/` — these are the
   project's own skills and exist because the project uses them.
   Omit a row only when a skill is intentionally inactive.
4. Do not duplicate content here — the router only points, never
   explains.
5. If you need a new project-local skill, create it in
   `/home/claude/state/skills/personal/` and add a row to
   **Personal**.
