# Concepts

The ideas behind Metaproject, one paragraph each. You do not need
them to use it — Claude follows the rules — but they help you
understand what Claude is doing and why.

## State and project

Every project has two parts. The **state** is the project's memory:
what was done, what was decided, what is left, how to work. The
**project** is the material itself: the chapters, the code, the
data. They travel separately because they change for different
reasons. The state is always a zip you upload. The project is
either a zip too (**standalone**) or lives somewhere else, such as
a GitHub repository, and is fetched only when a session needs it
(**external**). Inside a session, both sit at fixed paths
(`/home/claude/state/` and `/home/claude/project/`), so Claude never
has to search for anything.

Some external projects also hand you, at every close, a dated
**backup zip** of their source (a `git archive` of the repository,
say), and keep in the state the few files that are not in the
repository. They are still external: the session works on the
repository, and the backup is never unpacked (the startup file of an
external project has no project-zip lines). Their `setup.md` comes
from the external template, with the backup added by hand to
*Intermediate packaging* and *At the end of each session* as a
project-specific step. Name the backup so that it cannot be taken
for a state zip (`slug-backup-YYYYMMDD-vN.zip`, say, never
`slug-state-…`).

## What the state contains

| File | What it is for |
|---|---|
| `setup.md` | How to work in this project and how to close a session. Read first. |
| `handoff.md` | The note the previous session left for this one. |
| `conventions.md` | How things are done here: language, naming, style. |
| `router.md` | The index of skills and documents, and when to read each. |
| `todo.md` | What is left to do, by priority. |
| `agents.md` | A map of the project's material. |
| `handoffs/` | Every previous handoff — the project's history. |
| `skills/` | Know-how the project relies on (see below). |
| `closing/readme.md` | The startup file you upload loose at each session. |

At startup Claude reads only four of them — setup, handoff,
conventions and router — and looks up the rest when needed. That
keeps the start of a session quick and leaves room for the work.

## Handoffs

A **handoff** is a letter from one session to the next: what was
done and why, what is left, what to watch out for. It is not a log.
Each session is numbered (v1, v2, v3…) with a counter that never
resets, and every handoff is kept in `handoffs/` under its number.
Nothing there is ever deleted: the handoffs are the project's
memory. When the session closes, the current handoff is first
*copied* into the history and only then replaced, so the state is
never without one.

## The router

The **router** is a short table: "if you need to do X, read Y". It
holds no knowledge itself; it points to where the knowledge is. It
is read at every startup because it is the map of everything else —
a session that skipped it could not know which instructions applied
to the task in front of it.

## Skills

A **skill** is a document that teaches Claude how to do one thing
well: write a handoff, deploy a website, follow a house style.
Skills come from three places:

- **Core** skills come with Metaproject and cover its own
  processes, such as how to keep the to-do list.
- **Imported** skills come from a [skill collection](skill-collections.md):
  a published set of skills on some subject, maintained elsewhere.
  The router records where each one came from, so it can be updated.
- **Personal** skills are the project's own, written for it and
  kept in `skills/personal/`.

A skill is active when the router lists it.

## Inbox files

Some files exist only when something is waiting, and their mere
presence is the message. Claude checks for them at the start of
every session:

- `pending-tasks.md` — work a previous session left to be picked up.
  Claude proposes each item; you decide.
- `framework-feedback.md` — remarks about Metaproject itself that
  came up while working, waiting for whoever maintains the framework
  to collect them. Claude only writes one with your approval.
- `interim-vN.md` — a note left by an intermediate packaging; it
  means the previous session never got to close (see
  [How a session works](sessions.md)).

## Versions of the standard

The rules themselves evolve. Every state says which version of the
standard it follows, in a line near the top of its `setup.md`:
`*Metaproject standard: v5.*`. When a new version comes out,
`upgrade` brings a state up to date, one step at a time, without
losing anything. See [Operations](operations.md) and the
[Changelog](../CHANGELOG.md).
