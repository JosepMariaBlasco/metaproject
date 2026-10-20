# Setup — {{PROJECT_NAME}}

*Metaproject standard: v4.*

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

## Inbox files

Some files in state exist only when there is something pending;
their mere presence is the signal. Before anything else in this
session, check whether any of them exist, and for each one that
does, follow its protocol below. These are the only imperative
reads in the startup flow — everything else is on-demand.

### `pending-tasks.md`

If this file exists, read it and propose each entry to the user
before proceeding with the regular startup flow. Each entry is
something a previous session decided needed to happen but could
not or preferred not to do at the time.

For each entry, ask the user whether to execute it now. If they
say yes, do it conversationally and remove the entry from the
file — and if that was the last entry, delete the file itself.
If they say no, leave the entry where it is; it will come up
again next session.

### `framework-feedback.md`

If this file exists, it holds feedback this project has recorded
*about the framework itself* (a friction in the protocol, a
suggestion, a correction), waiting to be collected. Mention to the
user that there is uncollected framework feedback and show the
entries. Do not act on them as if they were tasks for this project,
and do not empty the file on your own — it is cleared only when the
user confirms the feedback has been carried back to the framework.

Entries get *into* this file during the session: when something
comes up that is feedback for the framework rather than work for
this project, propose recording it ("this looks like a suggestion
for the framework — shall I note it in the feedback mailbox?") and,
only with the user's approval, append it here. Never record it
unprompted.

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
- `framework-feedback.md` — present only when this project has
  recorded feedback about the framework, waiting to be collected
  by the maintainer. See the Inbox files section above.

## What to read on startup

After this file, the handoff, and the router you have enough
context to get started. Read the router (`router.md`) at startup
too: it is not content but the index of content — the map of which
skills and documents exist and when to read them — so deferring it
would mean not knowing what you might need. Do not read anything
else from state preventively — the rest is for on-demand
reference:

- `/home/claude/state/conventions.md`
- `/home/claude/state/agents.md`
- `/home/claude/state/todo.md`
- `/home/claude/state/skills/`
- `/home/claude/state/handoffs/`

Look things up when you need them: conventions and project map
when you're about to modify the project, the to-do list if the
session has no clear goal from the start.

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
