# upgrade2-3

Moves a state from Metaproject standard v2 to v3.

Assumes the state is at v2. Leaves the state at v3.

## What changes between v2 and v3

The v3 standard introduces `pending-tasks.md`, a lightweight
imperative buffer that lets a session hand off deferred work to
the next one in a way that cannot be silently skipped.

The mechanism has three parts:

- A new section in `setup.md`, **Pending tasks**, placed right
  after **Canonical paths**. It instructs the next session to
  check whether `/home/claude/state/pending-tasks.md` exists
  and, if so, to propose each entry to the user before doing
  anything else. The user may execute or postpone each entry
  individually. When the file becomes empty it is deleted.
- The `pending-tasks.md` file itself. **Not created ex-ante** —
  it exists only when something is actually pending. Plain
  markdown, one bullet per entry, no prescribed format beyond
  that.
- A new final step in `upgrade.md` that proposes running
  `update` after the last hop, and, if the user postpones it,
  writes an entry in `pending-tasks.md`. This is what motivates
  the mechanism: the structural upgrade leaves distributed
  content potentially out of sync, and nothing used to guarantee
  the follow-up.

v3 does not touch the structure of `skills/` or any other file.

## Procedure

### 1. Add the Pending tasks section to setup.md

Open `/home/claude/state/setup.md` and insert a new section
**Pending tasks** immediately after the **Canonical paths**
section and before **State files**. Use this text:

```
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
```

### 2. Add `pending-tasks.md` to the State files list

In the same `setup.md`, add one more bullet at the end of the
**State files** list:

```
- `pending-tasks.md` — present only when a previous session left
  something to be picked up. See the section above.
```

### 3. Update the version marker

Change the version marker near the top of `setup.md` from

```
*Metaproject standard: v2.*
```

to

```
*Metaproject standard: v3.*
```

### 4. Do not create `pending-tasks.md`

The file is created on demand by the operations that use it (for
now, the final step of `upgrade`). Do not create it during the
hop.

### 5. Verify

- `setup.md` has the **Pending tasks** section after
  **Canonical paths** and before **State files**.
- The **State files** list mentions `pending-tasks.md`.
- The version marker says v3.
- No `pending-tasks.md` file has been created.

### 6. Done

The state is now at v3. Control returns to the orchestrator.
