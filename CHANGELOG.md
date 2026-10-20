# Changelog

Releases are numbered `vN.M`: `N` is the version of the standard
(the line `*Metaproject standard: vN.*` in a state's `setup.md`), and
`M` counts releases within it. A new `N` always comes with an
`upgrade(N-1)-N.md` step that brings existing states up to date.

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
