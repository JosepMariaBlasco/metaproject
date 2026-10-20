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
and keeping shared skills in sync across projects.

## Operations

| You want to... | Follow... |
|---|---|
| Create state for a new project | `create.md` |
| Migrate an existing project to the standard | `migrate.md` |
| Audit a project's conformity | `audit.md` |
| Synchronise distributed material with a new version | `update.md` |
| Upgrade a project to a newer version of the standard | `upgrade.md` |

Each document is self-contained — read it and follow it step
by step.

## How to invoke an operation

Metaproject operations run inside a Claude session. The mechanics
are always the same: upload the Metaproject zip at the start of
the session and tell Claude to follow the relevant document
(`create.md`, `migrate.md`, `audit.md`, `update.md`, or
`upgrade.md`). Each document is written as an instruction set for
Claude — not a manual for the user — and drives the session end
to end.

There is no separate "start-of-session" protocol for Metaproject
itself: the choice of document is the protocol.

## What else is here

- `templates/` — templates used by `create.md` and `migrate.md`
  to generate the state files.
- `skills/` — skill collection, copied to each project. The three
  layers have different origins and lifecycles:
  - `skills/core/` — skills about the metaproject's own processes
    (how to maintain the todo, how to write handoffs, etc.). These
    are distributed with Metaproject and synchronised by `update`.
  - `skills/*.md` and subdirectories at the root — example skills
    shipped with Metaproject. Also distributed and synchronised.
    Activate the ones that matter in this project; leave the rest
    inactive or remove them if they are clearly irrelevant.
  - `skills/personal/` — the project's own skills, never travelled
    from Metaproject and never synchronised back. This is the space
    for anything project-local.

  Distributed skills at the root and in subdirectories (not
  `core/`) carry a YAML frontmatter with `name` and `description`
  fields, following the Claude Code convention. The `description`
  helps decide whether the skill is worth activating in a given
  project. Core skills omit the frontmatter — they are always
  active and do not need to be advertised.
- `handoffs/` — template for the handoffs directory that gets
  copied to each new project.

## Working on Metaproject itself

If you are working on the framework itself (not on a project
derived from it), Metaproject is set up as a Metaproject project
of its own: it has a state zip alongside the framework zip, and
the session starts by reading `setup.md` and `handoff.md` from
state. This is the same protocol as any derived project — the
framework eats its own dogfood.

## Further reading

The design of Metaproject, and the reasons behind it, are described
in the article *Metaproject — On keeping state between sessions*,
presented at the 37th International Rexx Language Symposium
(Barcelona, May 2026). The Markdown source is in
[`doc/article.md`](doc/article.md); it is also available as
[HTML](https://rexx.epbcn.com/metaproject/) and as a
[PDF](https://www.epbcn.com/pdf/josep-maria-blasco/2026-05-04-Metaproject.pdf).
