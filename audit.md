# audit

Checks whether a project's state structure conforms to the
Metaproject standard. It observes first: it lists what conforms
and what deviates, and changes nothing while doing so. Then, only
if the user wants, it fixes the deviations the user approves, one
by one (see *Fixing*, at the end).

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
  delivering with the environment's file-sending tool when there is
  one, and delivering `closing/readme.md` loose? Does it
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

### Skills against their upstreams

Obtain exact copies of the upstreams, as the list in step 1 of
`update.md` says (never a rendered web page), and compare byte for
byte:

- each file in `skills/core/` with Metaproject's `skills/core/`;
- each imported skill with the collection named in the **Source** of
  its router row. An imported skill without a **Source** is not
  compared; *Router* reports it.

Report, among the deviations, every skill that differs from its
upstream, every one present here but not upstream, and every core
skill that is new upstream (what `update` would call *Modified*,
*Local* and *New*); a modification the conventions record is named
as known. If an upstream cannot be downloaded, ask the user to
upload it; if that is not possible either, its skills are reported
as not checked, not as conforming. `skills/personal/` is not
compared.

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

### Comparison with the templates

The checks above name the elements the standard requires. A
maintenance release may also change wording that none of them names
(a flag in a command, a sentence in the closing protocol), so compare
the state with the current templates as well, section by section.
Skip this comparison when the state is on an older version than the
current standard: `upgrade` brings the wording in, and the report
would only fill up with what the upgrade will change anyway.

- `setup.md` with the setup template it follows:
  `templates/setup-external.md` if the project's material lives
  outside the state (a repository, a server), otherwise
  `templates/setup-standalone.md`.
- `closing/readme.md` with `templates/closing/readme.md`. For an
  external project, compare with the template **without its
  project-zip lines**, as `create.md` says to remove them: their
  absence is not a deviation, and their presence is (see *Startup
  file* above).
- The *Standard conventions* of `conventions.md` with those of
  `templates/conventions.md`.

Sections the template leaves as placeholders (*Project setup* in
the setup, *Project-specific conventions*) are not compared, and
neither are template options that do not apply to the project (the
git convention, in a project with no repository).

The state may be in another language, name the user and the
project, and carry lines of its own. One section may hold
differences of several kinds; each difference is one of three:

- **A deviation:** a rule or a step the template has is missing, or
  something contradicts it. Say exactly what (for instance, "the
  `find` of the closing check has no `-H`").
- **Wording only:** a sentence that says the same as the template's
  sentence, in other words. In a translated file, only a sentence
  that is not a translation of the template's current one — reworded,
  merged, split or reordered; a different but correct choice of words
  in the translation is not a difference. Not a deviation; list it
  apart (see *Report format*), so that it can be aligned. Once
  aligned, a later audit is quiet about it, and a new difference
  stands out.
- **The project's own:** what the state adds and the template does
  not have (a section of its own, project-specific lines, a clause
  added to a template sentence). Not a deviation: mention it briefly
  among what conforms, so the user sees it.

What the project's conventions record — a deviation kept on
purpose, wording kept on purpose, an addition of its own — is named
as known, not reported as news: a known addition among what
conforms, a known deviation among the deviations, marked as known.

### Project files

- Are there files in `/mnt/project/` (the Files/Knowledge
  section of the Claude project)? If so, list them — they would
  normally belong in state or project, not there.

## Report format

Present the results to the user grouped in three blocks:

1. **Conforming** — what meets the standard (briefly), including
   the project's own additions.
2. **Deviations** — what doesn't match, with enough detail for
   the user to understand what's going on, but without proposing
   fixes. A deviation may be an error or a conscious decision —
   only the user knows.
3. **Wording only** — the sentences that say the same as the
   template in other words (from *Comparison with the templates*),
   each with its section.

After presenting the report, ask the user whether they want to
address any of the deviations, or align the wording. If they do, go
on to *Fixing*.

## Fixing

**An older version comes first.** If the state is on an older
version than the current standard, do not patch it piece by piece
and fix nothing else: offer to run `upgrade.md` instead (it ends by
proposing `update`). The rest of the report will change after the
upgrade.

Otherwise, go through the deviations one at a time, in the order of
the report. For each:

1. Say what the fix would change — which file, which section, and
   what it would say afterwards — and ask the user: fix it now,
   leave it as it is, or leave it for later.
2. Apply only what the user approves. The user may approve several
   at once ("all the ones in `setup.md`"); even then, apply them one
   by one and show each.
3. A fix brings in the current wording of the template the
   comparison used (`templates/setup-*.md`,
   `templates/closing/readme.md`, `templates/conventions.md`), in
   the language of the file being fixed, with the user's name and
   the project's slug. Change only what the deviation names — add
   the missing rule or step, replace the sentence that contradicts
   the template — and do not rewrite the rest of the section.
   Cross-references use the section names the file already uses.
4. If the user leaves a fix for later, add an entry for it to
   `/home/claude/state/pending-tasks.md` (create the file if it does
   not exist).

After the deviations, offer to align the *Wording only* sentences,
all in one question (the user may pick some). Each alignment
replaces only a sentence listed as wording only with the template's
current one (translated, in a translated file), with the user's name
and the project's slug; apply them one by one, showing each before
and after. A sentence that carries anything of the project's own (a
name, a path, a clause the template does not have) is not replaced
without showing it to the user first. Nothing else in the section
changes. Aligning is optional: wording left as it is still conforms.

Some deviations are not fixed by editing the state here:

- **A skill or a copy that differs from its upstream.** A core
  skill or an imported skill that is not the same as its upstream,
  or a `handoffs/readme.md` that differs from Metaproject's (in
  substance, if translated): the fix is `update.md`, which compares them
  and asks about each difference. Offer to run it.
- **A gap in the handoff sequence.** Never filled. If the user
  confirms that the handoff is lost for good, write a tombstone, as
  the closing protocol of the setup template describes (a file
  `handoffs/handoff-YYYYMMDD-vK.md`, with today's date, titled
  `# Handoff vK — lost`); otherwise leave the gap as it is.
- **A rule the template says to copy into the project's
  repository** (the git attribution, into its `CLAUDE.md`): fix it
  there, in the repository, and commit and push it as the closing
  protocol says.
- **What only the user can change:** the Claude project
  instructions and the files in `/mnt/project/`. Tell the user what
  to change there, and why.

When the user leaves a deviation or some wording as it is on
purpose, propose recording the decision in the *Project-specific
conventions* of `conventions.md`; propose the same for the
project's own additions the audit found. Name the section and
describe each in a line; do not copy the text there. The next audit
then names them as known, instead of judging them again. When fixing is over, write in the new handoff, at close,
what was fixed and what was left.
