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
├── pending-tasks.md        (inbox file: present only when work is pending)
├── framework-feedback.md   (inbox file: present only when feedback is pending)
├── interim-vN.md           (inbox file: present only in an interim packaging)
├── closing/
│   └── readme.md           (startup file)
├── handoffs/
│   ├── readme.md
│   └── handoff-*.md
└── skills/
    ├── core/               (distributed, metaproject's own)
    │   └── *.md
    ├── personal/           (not distributed, optional)
    │   └── *.md
    ├── *.md                (optional: imported from external collections)
    └── subdir/             (optional: imported skills with a subrouter)
        └── *.md
```

Reports missing files and unexpected files (those not part of
the standard). Note the **inbox files** (`pending-tasks.md`,
`framework-feedback.md`, `interim-vN.md`): these exist only when something is
pending, so their *absence* is normal and never a deviation.
Their *presence* is expected and must not be reported as an
unexpected file.

### Metaproject coherence (prerequisite)

Before auditing the project, verify that Metaproject (its zip or a
clone of its repository) is internally coherent about its own
version. Three sources must
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
- Does it have the **Inbox files** section, covering the
  conditional startup reads? At v4 this section generalises the
  v3 *Pending tasks* check: it should describe a single imperative
  check over the family of inbox files, with a per-file protocol
  for at least `pending-tasks.md` (inward-facing: propose and
  execute here) and `framework-feedback.md` (outward-facing:
  report but do not empty). A setup still carrying only the v3
  *Pending tasks* section is on an older version — report it and
  point to `upgrade.md`.
  At v5 the family also includes `interim-vN.md`, with its rescue
  protocol.
- Does it have the **Session startup — reinjection-proof** section
  (v5), pointing to `closing/readme.md`?
- Does it have the state files section, and does it list
  `closing/readme.md` and the inbox files (`pending-tasks.md`,
  `framework-feedback.md`, `interim-vN.md`)?
- Does it have the what-to-read-on-startup section, and does it
  name four startup reads: setup, handoff, **conventions** and
  **router**? At v4 the router became a startup read; at v5 the
  conventions did too. A setup that still lists either among the
  on-demand reads is on an older version — report it and point to
  `upgrade.md`.
- Does it have the project setup section?
- Does it have the **Intermediate packaging** section (v5)?
- Does it have the session closing protocol, and does it include
  the v5 elements: continuity check before archiving, archiving
  **by copy** (not by move), `mkdir -p` of the output directory,
  the check that the outgoing zip keeps every archived handoff,
  another output directory when `/mnt/user-data/outputs` does not
  reach the user, and delivering `closing/readme.md` loose? Does it
  say that a session closes only when the user asks, and that a
  pause is not a close?
  In an external project kept in a repository, does it ask for
  every change to be committed and pushed at close?
  A protocol that still says "archiving means moving" is on an older
  version.
- Are the paths mentioned absolute and correct?

### Startup file

- Does `closing/readme.md` exist, with the project's slug in the
  `find` patterns of its guard, and does that `find` search the
  three upload folders with `find -H` (an upload folder may itself
  be a symbolic link)? Does its list of startup reads match the
  *What to read on startup* section of `setup.md`? In an external
  project, is it free of project-zip lines (a stray
  project zip must not be unpacked over the project)?
- Ask the user whether the Claude project instructions still hold
  any startup instruction (unzipping, reading setup). If they do,
  report it: it is the reinjection hazard the startup file exists
  to remove.

### Handoffs

- Does the current `handoff.md` have the expected sections
  (what's been done, what's left)?
- Do the archived handoffs follow the naming convention
  (`handoff-YYYYMMDD-vN.md`)?
- Is the sequence continuous? List the `vN` of the archived
  handoffs and report any number missing between v1 and the
  highest. A gap is a lost handoff; report it, do not try to fill
  it. (A *tombstone* — a `handoff-…-vK.md` titled `# Handoff vK —
  lost` — is a gap the user has already confirmed; it is not a
  deviation.)
- Is `handoffs/readme.md` the same as Metaproject's (or a
  translation with the same substance)? Projects keep it as a copy
  made when they were created, and an old one may describe rules the
  standard has dropped (a per-day counter, archiving by moving).
  Report any difference; `update` proposes replacing it.

### Router

- Does every row in the router point to a skill or subrouter
  that exists in `skills/`?
- If a row points to a subrouter (an `.md` with an associated
  subdirectory), does the subdirectory exist and does it contain
  the skills the subrouter references?
- Does every row in the *Imported* section have a **Source**?
  Report rows without one (they cannot be synchronised by
  `update`).
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
- Does it carry the standard conventions (v5): numeric dumps as
  files, intermediate packaging, and — if the project has a
  repository — git attribution without session links? Report
  missing ones for visibility; the project may have dropped one on
  purpose.
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
