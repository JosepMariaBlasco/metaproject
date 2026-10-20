# Metaproject

**Keep a long-running project going with Claude, session after
session, without starting from zero each time.**

Claude does not remember anything between sessions. For a quick
question that is fine. For a project that lasts weeks — a book, a
research line, a piece of software, a course — it means explaining
everything again every time: what was done, what was decided, what
comes next.

Metaproject fixes that with a simple habit. Your project's memory
lives in a small zip file, the **state**. You give it to Claude at the
start of each session; Claude reads it, picks up where the last
session left off, and at the end hands you back an updated zip. That
is all you need to do.

```
  start of session                         end of session
  ────────────────                         ──────────────
  you upload:                              Claude hands you back:
    state zip  ─────►  Claude works  ─────►  updated state zip
    readme.md          with you              readme.md
```

Nothing to install. It works in the Claude app, with any kind of
project, in any language.

## Get started

1. Download [`metaproject-latest.zip`](https://rexx.epbcn.com/metaproject/metaproject-latest.zip)
   (or clone this repository).
2. In the Claude app, open a new project and a new conversation.
   Upload the zip and say: *"Unzip this and follow `create.md`."*
3. Answer a few questions. Claude gives you your first state zip and
   a `readme.md`.
4. From then on, start every session by uploading both.

The [quickstart](doc/quickstart.md) walks you through it, step by
step.

## Learn more

| | |
|---|---|
| [Quickstart](doc/quickstart.md) | Your first project, in five minutes. |
| [Concepts](doc/concepts.md) | State, handoffs, the router, skills — one paragraph each. |
| [How a session works](doc/sessions.md) | Start, work, close — and what to do when a session runs long. |
| [Operations](doc/operations.md) | Create, migrate, audit, update, upgrade: which one you need. |
| [Troubleshooting](doc/troubleshooting.md) | Real problems that have happened, and their fixes. |
| [Skill collections](doc/skill-collections.md) | Reusable know-how for your projects, and how to publish your own. |
| [Changelog](CHANGELOG.md) | What changed in each version. |

## What is in this repository

The documents at the root (`create.md`, `migrate.md`, `audit.md`,
`update.md`, `upgrade.md` and the `upgradeN-M.md` steps) are
**instructions for Claude**, not manuals for you: you ask Claude to
follow one, and it drives the session. `templates/` holds the files
a new state is made from, `skills/core/` the framework's own skills,
and `doc/` the documentation you are reading.

Metaproject maintains itself with Metaproject: the framework is this
repository, and its own state travels separately, in a private zip.

## History

Metaproject was presented at the 37th International Rexx Language
Symposium (Barcelona, May 2026), in the article *Metaproject — On
keeping state between sessions*:
[source](doc/article.md), [HTML](https://rexx.epbcn.com/metaproject/article.md),
[PDF](https://www.epbcn.com/pdf/josep-maria-blasco/2026-05-04-Metaproject.pdf).
The article describes version 3 of the standard and is kept as it
was presented, for its historical value; the documentation above
describes the current version.

## License

Copyright 2026 Josep Maria Blasco.

Licensed under the Apache License, Version 2.0. See
[`LICENSE`](LICENSE) for the full text.
