# migrate

Metaproject protocol for migrating the state infrastructure of an
existing project to the Metaproject standard.

This is not a mechanical script — it is a protocol of analysis and
conversation. The migration may take several sessions.

## Key concept: state and project are separate things

The standard separates two spaces with fixed absolute paths:

| Space | Path | Contents |
|---|---|---|
| **State** | `/home/claude/state/` | Continuity infrastructure: handoffs, conventions, skills, project map, to-dos. Travels in a zip that the user uploads at the start of each session. |
| **Project** | `/home/claude/project/` | The project's own material — whatever the project is made of. Arrives as a zip too: either shipped alongside state (standalone) or built from a source hosted elsewhere and retrieved when the session needs it (external). |

This separation is key for two reasons:

1. **Continuity**: state travels between sessions independently
   of the project. Claude can resume work by reading the handoff
   without needing to re-examine the project from scratch.
2. **Efficiency**: fixed absolute paths eliminate ambiguity.
   No file requires interpretation to locate, which saves tokens
   and avoids errors.

In many existing projects, this separation does not exist — state
(if any) is mixed with the project, scattered across several
documents, or lives only in Claude's project Instructions. The
goal of the migration is to extract or create that state layer
as an independent space.

## Target structure

The standard defines this state structure:

```
state/
├── handoff.md          — what's been done, what's left
├── setup.md            — introduction, paths, state files,
│                         what to read on startup, project
│                         setup, session closing protocol
├── router.md           — skill router (only points, never explains)
├── agents.md           — map of the project's material
├── conventions.md      — working language and project conventions
├── todo.md             — to-dos ordered by priority
├── handoffs/
│   ├── readme.md       — handoff mechanics
│   └── handoff-*.md    — history (never deleted)
└── skills/
    ├── core/           — skills about the metaproject itself
    │                     (distributed, synchronised by `update`)
    ├── personal/       — the project's own skills
    │                     (never travelled, never synchronised)
    ├── *.md            — example skills (distributed)
    └── subdir/         — subrouter skills (distributed)
```

**Note on skills:** the `skills/` folder contains the full
collection of available skills for a project. What determines
which are *active* is `router.md`: a skill is active if it has
a row in the router table, inactive if it doesn't.

The three layers of `skills/` have different origins and
lifecycles:

- `skills/core/` — skills about the metaproject's own processes
  (how to maintain the todo, how to write handoffs, etc.). These
  are distributed with Metaproject and synchronised by
  `update`.
- `skills/*.md` and subdirectories at the root — example skills
  shipped with Metaproject. Also distributed and synchronised.
  Activate the ones that matter in this project; leave the rest
  inactive or remove them if they are clearly irrelevant.
- `skills/personal/` — the project's own skills, never travelled
  from Metaproject and never synchronised back. This is the space
  for anything project-local.

The reference templates are in `templates/` within
Metaproject.

## Phase 1 — Working language

Ask the user what language they want to work in (English,
Spanish, Catalan, etc.). Default is English — if the user
doesn't specify, use English. From this point on, conduct
the entire interaction in that language.

## Phase 2 — Audit

Look for the project's state transfer conventions. They may be
anywhere:

- Claude's project Instructions (not directly accessible — ask
  the user)
- Project files
- Inside the handoff or another state document
- Scattered across several documents

For each piece of information you find, note:

- **What it is** — handoff, conventions, router, project map,
  to-dos, setup, history, skills, or something else
- **Where it is** — in which file(s) and which section(s)
- **Current name** — the file's current name
- **Expected name** — according to the standard

Also identify the **project hosting**:

- **Standalone** — the project zip is shipped together with state
  and produced at session close. At session close, two zips must
  be packaged. The project Instructions must unzip both.
- **External** — the project zip is built from a source hosted
  elsewhere (a repository, a server, or similar) rather than
  shipped with state. The Instructions unzip state; setup
  documents how to obtain the project zip and unzip it at the
  canonical path. At session close, only the state zip is
  repackaged.

Establish the **project slug**: a short prefix (lowercase, no
spaces) that will be used in zip filenames (e.g.
`yaml-state-v3.zip`). Propose one derived from the project name
and let the user confirm or change it.

Also check whether there are **project files** (the
Files/Knowledge section of the Claude project). These files are
mounted at `/mnt/project/` — list their contents. If there are
files, present them to the user and propose for each one whether
it should go into state (skills, conventions, references),
project (the project's own material), or be discarded. The goal
is to leave the Files section empty, if at all possible:
everything that matters should travel in the zips, where it is
versioned and under control. If `/mnt/project/` is empty, no
need to mention it.

Also look for information that has no equivalent in the standard
(such as release notes, decision logs, or any project-specific
artefact). Do not discard it — it may make sense to keep it,
integrate it, or drop it, but that is the user's call.

## Phase 3 — Proposal

Present the user with a report containing:

1. **Inventory**: what you found, where, and what it maps to in
   the standard.
2. **Migration plan**: what to rename, what to merge, what to
   create from scratch, what to keep as is, what to discard.
   For each action, explain why.
3. **What's missing**: standard files that don't exist and need
   to be created (possibly from scattered content).
4. **What's extra**: files or sections that don't fit the
   standard. Propose what to do with them, but let the user
   decide.
5. **Estimate**: if the migration is complex, propose an order
   of execution and an estimate of how many sessions it may take.

## Phase 4 — Discussion

The user reviews the proposal. They may:

- Confirm the plan as is
- Modify specific decisions
- Postpone parts of the plan
- Add requirements you hadn't considered

Do not execute anything without the user's explicit agreement.

## Phase 5 — Execution

Apply the agreed changes. After each change:

- Update internal references (a file that mentions another by
  its old name).
- Verify that the resulting structure is coherent.

When done (or at the end of each session if the migration spans
multiple sessions), apply the session closing protocol described
in the `setup.md` that has been generated or migrated: move the
current `handoff.md` to `handoffs/` with the standard naming
convention, write a new `handoff.md` reflecting what was done,
and package the state zip.

## Phase 6 — Verification

When the migration is complete:

1. Present the final structure to the user.
2. Remind them to update the Claude project Instructions.
   The Instructions depend on project hosting:
   - **Standalone**: unzip two zips (state + project).
   - **External**: unzip state only. The project zip is retrieved
     during the session when the task requires it, following what
     `setup.md` documents under *Project setup*.
   Use the Instructions models from `create.md` (step 5) as
   reference.
3. Verify that `setup.md` contains the version marker for the
   current Metaproject standard (line `*Metaproject standard:
   vN.*` near the top). The templates in `templates/` already
   carry the current version, so if `setup.md` was generated
   from them this is automatic; if it was hand-assembled, add
   the marker.
4. Confirm that the standard is met.

## Principles

- **Do not destroy information.** Rename and reorganise, never
  delete. If something is to be removed, it must be an explicit
  decision by the user.
- **Respect the history.** Old handoffs are kept as they are,
  even if they use old names. They are the project's memory.
- **Go at the user's pace.** If something is unclear, ask.
  If the user wants to stop, stop. The migration is in no hurry.
- **The standard is a guide, not a law.** If a project has
  specific needs that don't fit, adapt the result — don't force
  conformity.
