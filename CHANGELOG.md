# Changelog

Releases are numbered `vN.M`: `N` is the version of the standard
(the line `*Metaproject standard: vN.*` in a state's `setup.md`), and
`M` counts releases within it. A new `N` always comes with an
`upgrade(N-1)-N.md` step that brings existing states up to date.

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
