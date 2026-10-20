# How a session works

A session has three moments: the start, the work, and the close.
Sometimes there is a fourth, when a session runs long.

```
  ┌───────────┐     ┌──────────┐     ┌──────────┐
  │   START   │ ──► │   WORK   │ ──► │  CLOSE   │
  └───────────┘     └──────────┘     └──────────┘
   upload zips        as usual         new handoff
   + readme.md        │                new zips
   unpack once        ▼                + readme.md
   read 4 files     interim packaging
                    (long sessions)
```

## Start

You upload the state zip (and the project zip, if the project is
standalone) together with `readme.md`. Claude follows the readme:

1. **Unpack, once.** The readme unpacks the zips only if the state
   is not already there. This guard matters: some versions of the
   Claude app re-send the project instructions on every turn, and
   startup steps can get repeated; an unpack repeated in the middle
   of a session would replace the work done so far with the old zip.
   With the guard, repeating it does nothing. (That is also why the
   unpacking lives in the readme and not in the project
   instructions.)
2. **Read four files:** `setup.md`, `handoff.md`, `conventions.md`
   and `router.md`.
3. **Check the inbox files** — pending tasks, framework feedback, an
   interim note — and tell you about any of them.

Then Claude tells you where things stand, and you decide what to do.

## Work

You work as you normally would. Claude looks things up in the state
when it needs them, following the router. If something comes up that
belongs to the framework rather than the project, Claude may propose
noting it in `framework-feedback.md`; it never does so on its own.

## Close

Closing is always your decision. Saying you are taking a break is not
a close; asking Claude to close is. When you ask, Claude:

1. **Checks the handoff history** for gaps. If a handoff is missing,
   it stops and asks you. If you confirm it is lost for good, it
   leaves a *tombstone* in its place — a short file that records the
   loss — so the history stays continuous.
2. **Archives the current handoff by copy**, and only then writes the
   new one, describing this session.
3. **Packages** the new state zip (and project zip, if standalone),
   and checks that no archived handoff went missing on the way.
4. **Hands you** the zips and a fresh copy of `readme.md`.

Save all of them. They are what you upload next time.

## Long sessions: intermediate packaging

Sessions can last days, and a session that dies before closing —
the context window fills up, the environment is reclaimed — takes
everything since the last zip with it. So when enough work has piled
up to hurt, before a long pause, or when you ask about the context,
Claude proposes an **intermediate packaging**: zips named
`…-interim.zip` plus a short note, `interim-vN.md`, saying the
session is still open. Nothing is archived; the session goes on.

If the session then dies, start the next one from the interim zips.
Claude sees the note and knows this is a **rescue**: it continues the
interrupted session's work and closes with that session's number, as
if nothing had happened.

## Big outputs as files

Long logs, data dumps and tables fill a conversation very fast —
numbers in particular. Attach them as files rather than pasting them,
and let Claude process them with tools. It keeps the session alive
much longer.
