# Metaproject

A framework for cross-session continuity with Claude.

Claude starts every session from zero — no memory, no context.
For one-off conversations this is fine, but for ongoing projects
it means losing everything between sessions: what was done, what
was decided, how the project is organised, what's left to do.

Metaproject solves that. It defines a portable state structure
that travels with your project as a zip file, uploaded at the
start of each session and updated at the end. Claude reads it,
picks up where the last session left off, and keeps working.

Metaproject includes protocols for creating this structure
from scratch, migrating existing projects, auditing conformity,
upgrading between versions of the standard, and keeping skills in
sync with their upstreams.

## Operations

| You want to... | Follow... |
|---|---|
| Create state for a new project | `create.md` |
| Migrate an existing project to the standard | `migrate.md` |
| Audit a project's conformity | `audit.md` |
| Synchronise a project's skills with their upstreams | `update.md` |
| Upgrade a project to a newer version of the standard | `upgrade.md` |

Each document is self-contained — read it and follow it step
by step.

## How to invoke an operation

Metaproject operations run inside a Claude session. The mechanics
are always the same: give Claude the framework at the start of the
session (upload its zip, or let Claude clone this repository) and
tell Claude to follow the relevant document
(`create.md`, `migrate.md`, `audit.md`, `update.md`, or
`upgrade.md`). Each document is written as an instruction set for
Claude — not a manual for the user — and drives the session end
to end.

There is no separate "start-of-session" protocol for Metaproject
itself: the choice of document is the protocol.

## What else is here

- `templates/` — templates used by `create.md` and `migrate.md`
  to generate the state files.
- `skills/` — the framework's own skills, copied to each project:
  - `skills/core/` — skills about the metaproject's own processes
    (how to maintain the todo, how to write handoffs, etc.). These
    are distributed with Metaproject and synchronised by `update`.
  - `skills/personal/` — a placeholder for the project's own
    skills, which never travel from Metaproject and are never
    synchronised back.

  Metaproject does not ship domain skills (a language reference, a
  deployment recipe, a publishing pipeline). Those are published
  and maintained in their own **skill collections**, and a project
  imports the ones it uses: they live at the root of the project's
  `skills/`, and the router records the collection each one comes
  from, so that `update` can keep them in sync. An example of a
  collection that works this way is
  [Rexx Skills](https://rexx.epbcn.com/skills/), skills for working
  with ooRexx and Regina.

  Skills meant to be imported usually carry a YAML frontmatter with
  `name` and `description` fields, following the Claude Code
  convention; the `description` helps decide whether a skill is
  worth importing. Core skills omit the frontmatter — they are
  always active and do not need to be advertised.
- `handoffs/` — template for the handoffs directory that gets
  copied to each new project.
- `doc/` — the article describing Metaproject's design (see
  *Further reading*).

## Working on Metaproject itself

If you are working on the framework itself (not on a project
derived from it), Metaproject is set up as a Metaproject project
of its own, in external mode: the project is this repository, and
its state travels separately, in a private state zip that is not
part of the repository. The session starts the same way as in any
derived project — the framework eats its own dogfood.

## Further reading

The design of Metaproject, and the reasons behind it, are described
in the article *Metaproject — On keeping state between sessions*,
presented at the 37th International Rexx Language Symposium
(Barcelona, May 2026). The Markdown source is in
[`doc/article.md`](doc/article.md); it is also available as
[HTML](https://rexx.epbcn.com/metaproject/) and as a
[PDF](https://www.epbcn.com/pdf/josep-maria-blasco/2026-05-04-Metaproject.pdf).

## License

Copyright 2026 Josep Maria Blasco.

Licensed under the Apache License, Version 2.0. See
[`LICENSE`](LICENSE) for the full text.
