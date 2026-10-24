# Changelog

Releases are numbered `vN.M`: `N` is the version of the standard
(the line `*Metaproject standard: vN.*` in a state's `setup.md`), and
`M` counts releases within it. A new `N` always comes with an
`upgrade(N-1)-N.md` step that brings existing states up to date.

## v5.6 — 24/10/2026

A maintenance release of standard **v5**, with no new upgrade step.
It changes how the next maintenance releases reach states that are
already at v5.

**`audit` can fix what it finds.** Until now `audit` only reported;
after v5.2, v5.4 and v5.5, a state already at v5 had to copy each
change from the templates by hand, following a paragraph in these
notes. Now `audit` also compares `setup.md`, `closing/readme.md` and
the standard conventions with the current templates, section by
section and in substance (language, names and the project's own
lines are not deviations), and ends with an optional *Fixing* step:
deviation by deviation, it says what it would change and applies only
what the user approves. An older version is sent to `upgrade`,
differing core skills or `handoffs/readme.md` to `update`, a gap in
the handoffs is never filled (a tombstone only if the user confirms
the loss), and what lives in the Claude project instructions is left
for the user to change.

**`upgrade` on a state already at the current version** no longer
just stops: it proposes `audit` with its *Fixing* step.

**If your state is already at v5**, there is nothing to copy by hand
this time: run `audit` and accept the fixes you want. From now on,
that is how a v5 state takes a maintenance release.

## v5.5 — 24/10/2026

A maintenance release of standard **v5**, with no new upgrade step.
It comes from the upgrade of eight real projects to v5 through
`upgrade.md`, all from hand-adopted v3 states; none failed, and what
each had to decide by hand is now written down.

**Delivering the closing files.** v5.4 asked Claude to judge whether
`/mnt/user-data/outputs` reaches the user; in an environment with both
that folder and a tool for sending files, that was a guess. Now the
rule is plain: when there is a tool for sending files, deliver every
file with it; package in `/mnt/user-data/outputs`, and use another
directory only if that one cannot be written. In the setup templates,
`create`, `audit`, `upgrade4-5` and `doc/troubleshooting.md`.

**`write-handoff`.** The core skill's pointer to *At the end of each
session* also covers a translated `setup.md`, so a translated state
need not edit it.

**The operations**, for states not yet at v5:

- `upgrade.md`: a state may name its version in a sentence rather
  than in the marker line; a plain `v3` marker on a state that already
  has the hand-adopted startup is the hybrid case too. A state whose
  files are in several languages keeps each file in its own.
- `upgrade4-5`: how to pick the template (a project in repositories
  that never shipped a project zip is external, whatever its old setup
  said); where project-specific startup lines go; `handoffs/readme.md`
  created when missing; an intermediate-packaging procedure moved out
  of the conventions; non-skill router rows moved to *State
  documents*; paths fixed when a skill moves to `personal/`; interim
  zips named in another language renamed, the old ones removed by hand
  at the first close.
- `update.md`: an imported skill the project extended locally: take
  the upstream copy, move the local additions to a personal skill, and
  first search the state for references into the old one.

**If your state is already at v5**, replace in `setup.md` the
paragraph that precedes the packaging commands with the template's
(or add it, if your state has not taken v5.4's), and the pointer to it
in step 3 of *Intermediate packaging*, adapting the user's name and
the language. Then run `update`: it reports `write-handoff.md` as
*Modified*; take Metaproject's copy (or ask Claude to do all of it).

## v5.4 — 23/10/2026

A maintenance release of standard **v5**, with no new upgrade step.
It comes from the first upgrade of a real project to v5 through
`upgrade.md` (from the hybrid v3 marker), which went through without
failures but left four things to be decided by hand.

- **The startup guard follows a symbolic-linked upload folder.** An upload folder may
  be a symbolic link, and `find` does not descend into a symbolic
  link given as a starting point: the guard would find no zip. The
  guard in `closing/readme.md` and the closing check in the setup
  templates now use `find -H`.
- **When `/mnt/user-data/outputs` does not reach the user.** Some
  Claude Code sessions have no such folder, or do not show its
  contents. The closing protocol in the setup templates now says to
  package in another writable directory (the scratchpad, say) and
  deliver with the environment's tool for sending files. `create`
  says the same for the first zips; `audit` checks it.
- **Rexx Skills' repository.** `update` needs an exact copy of a
  collection. `upgrade4-5` and `doc/skill-collections.md` now name
  the public repository `JosepMariaBlasco/rexx-skills` next to the
  site.
- **Repository plus backup zip.** A project kept in a repository that
  also delivers a backup zip of it at every close fitted neither
  setup template. It is external: `doc/concepts.md` explains how to
  add the backup, and `create` and `upgrade4-5` point there.

**If your state is already at v5**, change `find` to `find -H` in
your `closing/readme.md` and in the closing check of your
`setup.md`. Copy into `setup.md` the paragraph about the output
directory that now precedes the packaging commands in the setup
template, and the pointer to it in step 3 of *Intermediate
packaging*, adapting the user's name and the language to the
project (or ask Claude to do all of it).

## v5.3 — 23/10/2026

A fix to the startup of standard **v5**, with no new upgrade step.

- **Uploads in the Claude app's cloud sessions are found.** There,
  attached files arrive at
  `/mnt/user-data/attachments/<session>/<hash>-name`, a location the
  startup guard did not search: it found no state zip and stopped.
  The guard in `closing/readme.md`, the closing check in the setup
  templates and `upgrade4-5` now search three locations:
  `/mnt/user-data/uploads/`, `/mnt/user-data/attachments/` and
  `/root/.claude/uploads/`.

**If your state is already at v5**, add `/mnt/user-data/attachments`
to the `find` in your `closing/readme.md` and in the closing check of
your `setup.md` (or ask Claude to copy both from the templates). Until
then, in a session where the guard finds nothing, tell Claude where
the zip is.

## v5.2 — 21/10/2026

A maintenance release of standard **v5**, with no new upgrade step:
several files that restate the rules had drifted apart, and two
rules the documentation already assumed are now written into the
setup templates.

- **`handoffs/readme.md` is kept in step.** Projects copy it when
  they are created, and nothing refreshed it afterwards: a state
  upgraded to v5 could still say there that archiving means moving
  (or describe a per-day counter), against its own `setup.md`.
  `upgrade4-5` now replaces it, `update` offers to synchronise it
  with the core skills, and `audit` compares it.
- **A session closes only when you ask.** The documentation already
  promised it; now the closing protocol of the setup templates says
  so: a pause is not a close (an intermediate packaging before a long
  pause still is welcome). External projects kept in a repository
  also commit and push everything at close.
- Smaller fixes in `create`, `migrate`, `upgrade`, `write-handoff`
  and `doc/`.
- **For maintainers:** `CLAUDE.md` lists every rule stated in more
  than one file, with its canonical source.

**If your state is already at v5**, `upgrade` has nothing to do.
Run `update` to refresh `handoffs/readme.md`, and ask Claude to copy
the new opening paragraph of *At the end of each session* (and, in
an external project with a repository, the commit-and-push bullet)
from the setup template into your `setup.md`. `audit` reports both.

## v5.1 — 20/10/2026

First release of standard **v5**, and the first built from this
repository. It also contains standard v4, which was never released
on its own: coming from v3, `upgrade` applies `upgrade3-4.md` and
then `upgrade4-5.md`.

- **Startup that survives repeated instructions.** The startup no
  longer lives in the project instructions, which some versions of
  the Claude app re-send on every turn. It moves to a file,
  `closing/readme.md`, uploaded loose with the zips. It unpacks the
  zips only if the state is not there yet, looks for uploads in
  every known location, and uses the newest zip.
- **`conventions.md` is read at startup**, alongside setup, handoff
  and router.
- **Safer close.** The handoff is archived by copy, never by move;
  the handoff history is checked for gaps before archiving (with
  *tombstones* for handoffs confirmed lost) and in the outgoing zip.
- **Long sessions.** Intermediate packaging, with an
  `interim-vN.md` note; a session started from it is the rescue of
  the interrupted one.
- **Skills from collections.** The framework ships only its core
  skills. Domain skills come from skill collections; the router
  records the source of each, and `update` syncs every skill against
  its own source.
- **Starting conventions** in `templates/conventions.md`: big
  outputs as files, intermediate packaging, git attribution without
  links to private sessions.
- Apache License 2.0; new documentation in `doc/`.

## v4.0 — 21/06/2026 (not released separately)

Standard **v4**, released as part of v5.1.

- **The router is read at startup.** It is the index of everything
  else, so deferring it was circular.
- **Inbox files.** The startup check for `pending-tasks.md`
  generalises to a family of files whose presence means something
  is waiting.
- **`framework-feedback.md`**, a mailbox in which a project collects,
  with the user's approval, remarks about Metaproject itself.

## v3.3 — 26/04/2026

- Archiving a handoff described unambiguously. (Replaced in v5 by
  archiving by copy.)

## v3.2 — 24/04/2026

- Handoff numbers defined as a counter across the whole project
  history, not per day.

## v3.1 — 23/04/2026

First release of standard **v3**.

- **`pending-tasks.md`**: work one session leaves for the next, read
  at every startup.
- `upgrade` offers to run `update` afterwards, so that a state is up
  to date in content as well as in structure.
- Absolute paths in the router, and a *Personal* section for the
  project's own skills.

## v2.1 — 22/04/2026

First public release, standard **v2**: language-neutral templates,
skills with YAML headers, and the three layers of `skills/`.
