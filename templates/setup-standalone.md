# Setup — {{PROJECT_NAME}}

*Metaproject standard: v3.*

This project works with two independent zips. The **state zip**
contains cross-session continuity: handoffs, conventions, pending
tasks, project map and reference skills. You receive it at the
start of every session. The **project zip** contains the project's
own material — whatever the project is made of. You will unpack it
at the start of each session alongside the state zip.

## Canonical paths

| What | Fixed path |
|---|---|
| State zip | → `/home/claude/state/` |
| Project zip | → `/home/claude/project/` |

## Pending tasks

Before anything else in this session, check whether
`/home/claude/state/pending-tasks.md` exists. If it does, read it
and propose each entry to the user before proceeding with the
regular startup flow. Each entry is something a previous session
decided needed to happen but could not or preferred not to do at
the time.

For each entry, ask the user whether to execute it now. If they
say yes, do it conversationally and remove the entry from the
file — and if that was the last entry, delete the file itself.
If they say no, leave the entry where it is; it will come up
again next session.

This is the only imperative read in the startup flow. Everything
else is on-demand.

## State files

- `setup.md` — this file.
- `conventions.md` — working conventions, terminology.
- `todo.md` — to-do list ordered by priority.
- `agents.md` — map of the project's material.
- `handoffs/` — handoff history. No handoff is ever deleted —
  they constitute the project's memory.
- `skills/` — skill collection. See the router for the structure
  and what is active.
- `router.md` — skill router pointing to the correct documents.
- `pending-tasks.md` — present only when a previous session left
  something to be picked up. See the section above.

## What to read on startup

After this file and the handoff, you have enough context to get
started. Do not read anything else from state preventively — the
rest is for on-demand reference:

- `/home/claude/state/conventions.md`
- `/home/claude/state/agents.md`
- `/home/claude/state/todo.md`
- `/home/claude/state/router.md`
- `/home/claude/state/skills/`
- `/home/claude/state/handoffs/`

Look things up when you need them: conventions and project map
when you're about to modify the project, the router when you need
a skill, the to-do list if the session has no clear goal from the
start.

## Project setup

*Pending configuration. Document here any operations the project
needs to be set up or exercised — running tests, building outputs,
or whatever applies to this project.*

## At the end of each session

* Update `/home/claude/state/agents.md` if the project's structure has changed.

* The handoff is a single piece with two states: live (at the
  state root, as `handoff.md`) or archived (in `handoffs/`, named
  `handoff-YYYYMMDD-vN.md`). At any given moment it exists in
  exactly one of the two places, never both. Archiving means
  moving, not copying.

* Before packaging: archive the live handoff by moving
  `/home/claude/state/handoff.md` to
  `/home/claude/state/handoffs/handoff-YYYYMMDD-vN.md`, where
  `vN` is the session number that *wrote* it — a monotonic
  counter that increments once per session across the whole
  project history, not a per-day counter. The date is whatever
  day the handoff is being written. Then write a new
  `/home/claude/state/handoff.md` reflecting what was done in
  this session.

* Package the complete state zip (all files in
  `/home/claude/state/`, not just the ones that changed). Name:
  `{{PROJECT_SLUG}}-state-YYYYMMDD-vN.zip`, using the same
  session number as the handoff. Always
  package from `/home/claude/` so that the zip contains `state/`
  as its root directory:

  ```bash
  cd /home/claude && zip -r /mnt/user-data/outputs/{{PROJECT_SLUG}}-state-YYYYMMDD-vN.zip state/
  ```

* Also package the project zip with all files in
  `/home/claude/project/`. Name:
  `{{PROJECT_SLUG}}-project-YYYYMMDD-vN.zip`.

  ```bash
  cd /home/claude && zip -r /mnt/user-data/outputs/{{PROJECT_SLUG}}-project-YYYYMMDD-vN.zip project/
  ```
