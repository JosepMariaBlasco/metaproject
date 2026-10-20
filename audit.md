# audit

Checks whether a project's state structure conforms to the
Metaproject standard. Does not fix anything — lists deviations
and lets the user decide what to do.

## What is checked

### File structure

Verifies that all expected files exist:

```
state/
├── handoff.md
├── setup.md
├── router.md
├── agents.md
├── conventions.md
├── todo.md
├── handoffs/
│   ├── readme.md
│   └── handoff-*.md
└── skills/
    ├── core/               (distributed, metaproject's own)
    │   └── *.md
    ├── personal/           (not distributed, optional)
    │   └── *.md
    ├── *.md                (distributed examples)
    └── subdir/             (optional: subrouters with their skills)
        └── *.md
```

Reports missing files and unexpected files (those not part of
the standard).

### Metaproject coherence (prerequisite)

Before auditing the project, verify that the Metaproject zip is
internally coherent about its own version. Three sources must
agree:

- `max(M)` among the `upgradeN-M.md` files.
- The version declared in `templates/setup-standalone.md`.
- The version declared in `templates/setup-external.md`.

If any of the three diverges, **stop and report** — the
Metaproject itself is broken and there is no well-defined current
standard to audit against. Fix Metaproject before running audit.

When the three agree, that number is the **current standard** and
audit proceeds.

### Contents of setup.md

- Is the Metaproject standard version declared (line
  `*Metaproject standard: vN.*` near the top)? If absent, the
  state predates versioning and is treated as **v1**.
- Does the declared version match the current standard
  established above? If the project is on an older version,
  report it as a deviation and mention that `upgrade.md` is the
  protocol to bring it current.
- Does it have the introductory paragraph (two-zip description)?
- Does it have the canonical paths table (state + project)?
- Does it have the state files section?
- Does it have the what-to-read-on-startup section?
- Does it have the project setup section?
- Does it have the session closing protocol?
- Are the paths mentioned absolute and correct?

### Handoffs

- Does the current `handoff.md` have the expected sections
  (what's been done, what's left)?
- Do the archived handoffs follow the naming convention
  (`handoff-YYYYMMDD-vN.md`)?

### Router

- Does every row in the router point to a skill or subrouter
  that exists in `skills/`?
- If a row points to a subrouter (an `.md` with an associated
  subdirectory), does the subdirectory exist and does it contain
  the skills the subrouter references?
- Are there skills in `skills/` (root or subdirectories other
  than `core/` and `personal/`) that have no row in the router
  (neither directly nor through a subrouter)?
  (Not an error — these are inactive skills — but reported for
  visibility.)
- For `skills/core/`: report which core skills are not in the
  router. Core skills are always available but, like any other
  skill, activated by being listed in the router.
- For `skills/personal/`: report which personal skills are not
  in the **Personal** section of the router. The default is that
  all personal skills are listed — they exist because the project
  uses them — so omissions are worth the user's visibility. Not
  an error: a personal skill may be intentionally unrouted (kept
  for reference, draft, superseded), but the user should confirm
  it is on purpose.
- Are the paths in the router absolute? The standard requires
  absolute paths in router tables (e.g. `/home/claude/state/skills/core/edit-todo.md`)
  so that Claude does not have to resolve anything at read time.
  Report any relative paths as deviations.

### Conventions

- Does it define the working language?
- Is the *Project-specific conventions* section present? Report
  whether it has been filled in or still holds the placeholder
  text — the section is open by design, so an unfilled placeholder
  is not an error, but it is worth the user's visibility in case
  conventions have emerged that are not yet written down.

### Project files

- Are there files in `/mnt/project/` (the Files/Knowledge
  section of the Claude project)? If so, list them — they would
  normally belong in state or project, not there.

## Report format

Present the results to the user grouped in two blocks:

1. **Conforming** — what meets the standard (briefly).
2. **Deviations** — what doesn't match, with enough detail for
   the user to understand what's going on, but without proposing
   fixes. A deviation may be an error or a conscious decision —
   only the user knows.

After presenting the report, ask the user whether they want to
address any of the deviations.
