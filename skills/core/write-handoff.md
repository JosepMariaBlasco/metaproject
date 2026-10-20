# write-handoff

How the `handoff.md` at the state root is written.

Read this when you are about to write or edit the current
handoff. The mechanics of archiving (when to move, how to name)
live in `handoffs/readme.md` — this skill is about what goes
inside the file.

## Purpose of a handoff

A handoff is the session's message to the next session. It is
written so that a fresh Claude, starting from zero, can pick up
where this session left off after reading it.

It is not a log and not a report to the user. It is working
memory for future sessions.

## Shape

Two things must be present: **what was done in this session** and
**what is left to do**. The rest is judgement. A handoff may be
three paragraphs or several pages depending on the session. Long
handoffs benefit from section headers; short ones do not need
them.

Do not duplicate what is already captured elsewhere. If a
decision is recorded in the todo or in `conventions.md`, the
handoff can reference it instead of restating it.

## Double-anchor convention for references

When a handoff references a task from the todo, use **both the
number and a short description**:

> Task 14 (Unicode handling in the scanner) is done.

not just:

> Task 14 is done.

The reason is that the todo can be renumbered (see
`skills/core/edit-todo.md`). Numbers are ephemeral; short
descriptions are durable. If a future renumbering moves task 14
to task 9, the number in this handoff becomes wrong — but the
description still identifies the task unambiguously.

The short description should be genuinely identifying, not
generic. "Task 14 (the Unicode one)" works; "Task 14 (a fix)"
does not.

This convention applies to any reference from a handoff into
the todo. It does not apply to references between handoffs, or
to references to files, which are stable by other means.

## What to include

Enough for the next session to act without re-reading the
project. Typically:

- What was done, with enough context to understand *why*, not
  just *what*. Decisions matter more than keystrokes.
- Open questions the user did not resolve in this session.
- Pointers into the project: which files were touched, which
  directories are relevant.
- State of things that a reader would otherwise have to
  reconstruct (e.g. "the project was not downloaded this
  session", "the test suite is currently failing on X").

## What to leave out

- Mechanical detail that adds no signal ("ran `ls`, then `cat`,
  then...").
- Praise or commentary on the session itself, unless it carries
  information the next session needs.
- Material that belongs in another state file (conventions,
  todo, agents) — put it there and reference it.
