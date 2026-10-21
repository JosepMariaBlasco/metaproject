# upgrade4-5

Moves a state from Metaproject standard v4 to v5.

Assumes the state is at v4. Leaves the state at v5.

**States that adopted part of v5 by hand.** Some projects adopted
the v5 startup and closing rules before v5 existed; their marker
reads `v3 + reinjection-proof startup (…)`. The orchestrator treats
them as v3 and brings them to v4 with `upgrade3-4` first. When they
reach this hop, several pieces below are already in place. Every
step therefore starts by checking: if the piece already exists,
verify it matches what is described here, adjust what differs, and
do not duplicate it.

## What changes between v4 and v5

v5 makes the session startup and close robust against the ways in
which real sessions have lost work, and separates the framework's
own skills from domain skills.

### 1. Startup moves out of the project instructions

Until v4, `create` told the user to put the startup instruction (an
`unzip` of the zips, then "read setup") in the Claude project
instructions. Some orchestrators reinject the project instructions
on **every turn**, not just the first. An `unzip -o` reinjected
mid-session overwrites the live, edited state on disk with the
incoming, older zip, and the session's work is lost silently.

v5 moves the startup instruction to a file in state,
`closing/readme.md`, which the user hands over loose, together with
the zips, at the start of each session. Its decompression sits
behind a guard anchored to `setup.md`
(`if [ ! -f /home/claude/state/setup.md ]`), which makes it
idempotent. The project instructions are left without any startup
instruction.

### 2. Uploads are looked for in two places

In some environments uploaded files do not arrive in
`/mnt/user-data/uploads/` but in
`/root/.claude/uploads/<session>/<hash>-name`. The guard looks in
both, with a pattern that admits the hash prefix.

### 3. The conventions become a startup read

v4 read setup, the handoff and the router at startup, and left the
conventions on-demand. That is circular: to know whether the
conventions are needed, one has to have read them. They apply to any
work, so v5 reads them at startup.

### 4. The handoff is archived by copy, and continuity is checked

v4 archived the handoff by *moving* it, which leaves an instant
with no `handoff.md`; if anything fails there, state is left
without a handoff. v5 archives by copy and overwrites only after
the copy is verified. Before archiving, the session checks that no
handoff number is missing; after packaging, it checks that the
outgoing zip keeps every archived handoff the incoming one had.
Handoffs have been lost both ways: by a single failure, and by
erosion across repackagings.

The closing protocol also says, from v5.2 on, what has always been
the practice: a session closes only when the user asks, and a pause
is not a close. And the project's `handoffs/readme.md`, which
describes the same mechanics and had never been refreshed after
`create`, is brought in line with Metaproject's.

### 5. Intermediate packaging, and the interim inbox file

Long sessions can die before closing (context full, environment
reclaimed). v5 adds an intermediate packaging, proposed by Claude
and decided by the user, with an `interim-vN.md` note. The note
joins the v4 family of inbox files: if a session starts from an
interim packaging, it is the rescue of session vN and closes with
vN's number.

### 6. Starting conventions

`templates/conventions.md` now ships three standard conventions:
numeric dumps always as files, intermediate packaging, and git
attribution without links to sessions.

### 7. Domain skills leave the framework

Until v4, Metaproject shipped example domain skills at the root of
`skills/` (ooRexx, Paged.js, RexxPub and others), copied into every
project and synchronised by `update`. Those skills are not the
framework's: they have their own upstreams and are published in
their own collections. v5 ships only `skills/core/`. A project's
skills now have three origins — **core** (from Metaproject),
**imported** (from an external collection, recorded in the router's
new **Source** column) and **personal** — and `update` synchronises
each against its own upstream.

## Procedure

Throughout, use the templates of the Metaproject version you are
upgrading to as the canonical wording: `templates/closing/readme.md`,
`templates/setup-standalone.md` or `templates/setup-external.md`
(whichever matches the project's hosting), `templates/conventions.md`
and `templates/router.md`. Write in the project's working language
if it is not English, keeping the substance.

### 1. Create `closing/readme.md`

Create `/home/claude/state/closing/readme.md` from
`templates/closing/readme.md`, substituting `{{PROJECT_NAME}}`,
`{{PROJECT_SLUG}}` (the prefix of the project's zips — look it up in
`setup.md`) and `{{USER_NAME}}`. For an external project, remove
everything about the project zip (the sentence about it, and every line of
the guard that handles `$P`).

If the file already exists (adopted by hand), bring it in line with
the template: the guard must use exact name patterns, take the
newest zip when there is more than one (and say so), and stop when
there is none; the startup reads must be four — setup, handoff,
conventions and router. Replacing the hand-adopted file with the
template is usually simplest.

Run the `find` of the guard, with the slug substituted, and check
that it finds this session's zips.

### 2. Update `setup.md`

Copy from the setup template that matches the project's hosting,
adjusting names and wording to the project:

- **Session startup — reinjection-proof**: the whole section, right
  after *Canonical paths*.
- **Inbox files**: add the `interim-vN.md` subsection after the
  existing ones.
- **State files**: add the `closing/readme.md` and `interim-vN.md`
  entries.
- **What to read on startup**: four startup reads — setup, handoff,
  conventions and router — with the reasons given in the template.
  `conventions.md` leaves the on-demand list.
- **Intermediate packaging**: the whole section, before *At the end
  of each session*.
- **At the end of each session**: add the template's opening
  paragraph (a session closes only when the user asks; a pause is
  not a close), and replace the handoff and packaging bullets with
  the template's — the absolute counter, the continuity check,
  archiving by copy, the `mkdir -p`, the check of handoffs in the
  outgoing zip, and delivering `closing/readme.md` loose. Remove any
  text saying that archiving means moving. In an external project
  kept in a repository, add the template's bullet asking for every
  change to be committed and pushed. Keep any project-specific
  closing steps the setup already had.

If the setup carries a hand-adopted *Session startup* section or
closing rules, replace them with the template's wording rather than
keeping two versions.

### 3. Replace `handoffs/readme.md`

Replace `/home/claude/state/handoffs/readme.md` with Metaproject's
`handoffs/readme.md` (translated, if the project's state is in
another language, keeping the substance). The project's copy was
made when the project was created, and nothing has refreshed it
since: depending on its age it may still say that archiving means
moving, or describe a per-day counter. Left as it is, it contradicts
the closing protocol just written into `setup.md`.

### 4. Update `conventions.md`

Add the three standard conventions from `templates/conventions.md`
(section *Standard conventions*) that the project does not already
have. Drop the git one if the project has no repository. If the
project has a repository, the git attribution rule also goes into
its `CLAUDE.md`.

### 5. Reclassify the skills at the root of `skills/`

Every skill at the root of `skills/`, or in a subdirectory other
than `core/` and `personal/`, becomes either an **imported** skill
or a **personal** one.

1. In `router.md`, rename the *Project-specific* section to
   *Imported* and add a **Source** column, as in
   `templates/router.md`.
2. For each such skill, find its upstream collection and record it
   in **Source**. For the skills Metaproject itself used to ship:

   | Skill (v4 name) | Where it lives now |
   |---|---|
   | `oorexx`, `oorexx-run`, `ebnf2svg` | Rexx Skills, `https://rexx.epbcn.com/skills/` |
   | `apache-cgi-oorexx` | Rexx Skills (same name) |
   | `apache-cgi-rexx` | Rexx Skills, renamed to `apache-cgi-oorexx` |
   | `pagedjs-cli`, `rexxpub` (and `rexxpub/`) | No published collection at the time of writing |

   For any other skill, ask the user where it comes from.
3. Skills without a collection are local to the project. Propose
   to the user, for each one, moving it to `skills/personal/` (with
   its row to the router's *Personal* section) or removing it. Do
   nothing without the user's decision.
4. Inactive skills (present in `skills/` but with no router row)
   that came from Metaproject — those named in the table above —
   are no longer distributed. Propose removing them; the user
   decides. For any other inactive skill, ask the user.

The comparison of each imported skill with its collection is done
by `update`, which the orchestrator proposes after the last hop.
For a skill renamed upstream (such as `apache-cgi-rexx`), `update`
will report the old name as *Local* and propose the new one.

### 6. If the state has `escalate-to-meta.md`

Some projects kept an `escalate-to-meta.md` for remarks addressed
to the framework. v4's `framework-feedback.md` is the standard
mailbox for exactly that. Propose to the user moving its entries
into `framework-feedback.md` (creating it if needed) and then
removing `escalate-to-meta.md` and its entry in *State files*.
Entries that record rules now standardised by v5 can be marked as
such rather than carried over as open. Do nothing without the
user's decision.

### 7. Update the version marker

Change the version marker near the top of `setup.md` from
`*Metaproject standard: v4.*` to:

```
*Metaproject standard: v5.*
```

### 8. Tell the user what changes for them

1. **Remove every startup instruction from the Claude project
   instructions** (unzipping, reading setup). They may stay empty,
   or hold only things that do no harm if repeated on every turn.
   Recommend this line, which acts only before state exists and
   lets a fresh session find the startup file by itself:

   ```
   If `/home/claude/state/setup.md` does not exist yet, look for an
   uploaded `readme.md` and follow it.
   ```
2. **From now on, at the start of each session, upload the zip(s)
   and `closing/readme.md`, loose.** Claude delivers an updated copy
   of it at the end of every session.

### 9. Verify

- `closing/readme.md` exists, with the right slug, its `find`
  finds this session's zips, and its startup reads match
  `setup.md`.
- `setup.md` has the *Session startup* section, the
  `interim-vN.md` inbox file, the new *State files* entries, four
  startup reads, the *Intermediate packaging* section, and the v5
  closing protocol, opening with the close-only-when-asked rule (no
  "archiving means moving" left anywhere).
- `handoffs/readme.md` matches Metaproject's.
- `conventions.md` has the standard conventions that apply.
- Every skill outside `core/` and `personal/` has a router row in
  *Imported* with a **Source**, or the user has decided what to do
  with it.
- The version marker says v5.

### 10. Gaps in `handoffs/`

The v5 closing protocol stops at any gap in the handoff sequence.
States that lost handoffs before v5 will hit it at the close of this
very session. Run the continuity check now, show the user any gap,
and, for each one the user confirms as lost for good, write the
tombstone described in the closing protocol. Then the close can go
ahead.

### 11. Done

The state is now at v5. Control returns to the orchestrator. This
session already closes with the v5 protocol; it is the first time
the project does, so if anything in it does not fit the project's
`setup.md`, say so in the handoff.
