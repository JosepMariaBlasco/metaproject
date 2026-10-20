# Quickstart

Your first Metaproject project, in about five minutes. You need the
Claude app and nothing else.

## 1. Get the framework

Download the zip of the
[latest release](https://github.com/JosepMariaBlasco/metaproject/releases/latest)
from GitHub, or clone
[the repository](https://github.com/JosepMariaBlasco/metaproject).
Do not unzip it yourself: Claude will.

## 2. Create the project

In the Claude app, create a new project for your work and open a
conversation in it. Upload the framework zip and write:

> Unzip this and follow `create.md`.

Claude asks a handful of questions in one go:

- **Working language** — the language you want to talk to Claude in.
- **Name and a one-line description** of the project.
- **Slug** — a short prefix for the zip file names, like `novel` or
  `thesis`. Claude proposes one.
- **Your name** — so the state knows who uploads the zips.
- **Hosting** — does the project's material travel in a zip too
  (*standalone*), or does it live somewhere else, such as a GitHub
  repository (*external*)? If you are not sure, choose standalone.
- **Skill collections** — whether you want to import ready-made
  skills on some subject. You can say no and add them later.

Claude then builds the state and hands you:

- `novel-state-YYYYMMDD-v1.zip` — the project's memory;
- `novel-project-YYYYMMDD-v1.zip` — the project's material
  (standalone only);
- `readme.md` — the startup file.

Keep all three somewhere safe. Claude will hand you new copies at the
end of every session; the old ones can be kept as backups.

## 3. Set up the project instructions

Claude will suggest what to put in the project's instructions. Keep
them short, and **never put "unzip this" there**: some versions of
the app re-read the instructions on every turn, and an unzip
repeated mid-session would overwrite the session's work with the old
zip. A safe pair of lines is:

```
If `/home/claude/state/setup.md` does not exist yet, look for an
uploaded `readme.md` and follow it.

Interaction language: English. Always use this language unless the
user explicitly switches to another.
```

The first line only does something before the state is unpacked, so
it is harmless if it is repeated.

## 4. Every session from now on

**Start:** open a new conversation in the project and upload the
latest state zip (and project zip, if standalone) plus `readme.md`.
Then say what you want to do — or just "let's continue". Claude
unpacks the zips once, reads where things stand, and tells you.

**Work** as you normally would.

**Close:** when you are done, ask Claude to close the session. It
writes a *handoff* (a note for the next session), packages new zips,
and hands them to you together with `readme.md`. Save them; they are
what you upload next time.

A pause is not a close. If you say you are stepping away for lunch,
nothing is packaged; you just carry on afterwards. In a very long
session, Claude may propose an *intermediate packaging* so that a
crash cannot cost you the whole day — see
[How a session works](sessions.md).

## Next

- [Concepts](concepts.md) — what is inside the state, and why.
- [Troubleshooting](troubleshooting.md) — when something looks wrong.
