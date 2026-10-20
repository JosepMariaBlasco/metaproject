# upgrade1-2

Moves a state from Metaproject standard v1 to v2.

Assumes the state is at v1 (either explicitly or, more commonly,
by absence of the version marker). Leaves the state at v2.

## What changes between v1 and v2

The v2 standard introduces a three-layer organisation inside
`skills/`, where the three layers have different origins and
lifecycles:

- `skills/core/` — skills about the metaproject's own processes
  (how to maintain the todo, how to write handoffs, etc.). These
  are distributed with Metaproject and synchronised by `update`.
  **New in v2.**
- `skills/*.md` and subdirectories at the root — example skills
  shipped with Metaproject. Also distributed and synchronised.
  Activate the ones that matter in this project; leave the rest
  inactive or remove them if they are clearly irrelevant.
  Unchanged from v1 in terms of location, though some skills may
  have moved to `core/` or `personal/` at the metaproject level.
- `skills/personal/` — the project's own skills, never travelled
  from Metaproject and never synchronised back. This is the space
  for anything project-local. **New in v2.**

The router template is also updated to activate the core skills
by default.

Additionally, v2 introduces the **version marker** in `setup.md`.

## Procedure

### 1. Prepare Metaproject in context

The user should have uploaded the current Metaproject zip. Verify
that the following exists in the Metaproject copy:

- `skills/core/edit-todo.md`
- `skills/core/write-handoff.md`

If these are missing, Metaproject is itself outdated — stop and
report.

### 2. Create the new skills directories in the project

```bash
mkdir -p /home/claude/state/skills/core
mkdir -p /home/claude/state/skills/personal
```

### 3. Copy core skills into the project

Copy the full content of Metaproject's `skills/core/` into the
project's `skills/core/`:

```bash
cp -r /path/to/metaproject/skills/core/* /home/claude/state/skills/core/
```

(Adapt the source path to wherever the user placed the
Metaproject copy.)

### 4. Review skills at the project's `skills/` root

Look at the files currently sitting in `skills/` (root) in the
project. Some of them may be project-local material that should
live in `skills/personal/` rather than the root. Examples of
what *probably* belongs in `personal/`:

- Directories of people specific to the user or project.
- Biographies or author information.
- Style guides specific to the user.
- Anything clearly not reusable across projects.

Present the list to the user and ask for each ambiguous case
whether to move it. Do not move anything without confirmation.

For each file the user confirms as personal:

```bash
mv /home/claude/state/skills/<file>.md /home/claude/state/skills/personal/
```

If `router.md` referenced that skill, its path in the router
table must be updated accordingly.

### 5. Update the router

Open `/home/claude/state/router.md` and:

- Add a **Core** subsection under "Active skills in this project"
  with rows for the core skills just copied:

  ```
  ### Core (metaproject conventions)

  | You need to... | Read... |
  |---|---|
  | Edit the todo list | `skills/core/edit-todo.md` |
  | Write the session's handoff | `skills/core/write-handoff.md` |
  ```

- Rename the existing table (if any) to **Project-specific** for
  clarity.

- Update any router rows whose skills were moved to `personal/`
  in step 4.

The template in Metaproject's `templates/router.md` shows the
canonical v2 layout.

### 6. Add the version marker to `setup.md`

Insert the following line immediately below the title of
`/home/claude/state/setup.md`:

```
*Metaproject standard: v2.*
```

(If the state comes from an even older variant where the title
is different, the marker still goes right after the title.)

### 7. Verify

- `skills/core/` exists and contains the core skills.
- `skills/personal/` exists (may be empty).
- Router has a Core subsection with the new rows.
- `setup.md` has the version marker.
- Any skills that moved are still correctly referenced.

### 8. Done

The state is now at v2. The orchestrator (`upgrade.md`) updates
the version marker and moves on to the next hop if there is one.
For v2 → current, there is no next hop yet.
