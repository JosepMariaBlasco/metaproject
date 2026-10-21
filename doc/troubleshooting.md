# Troubleshooting

Every problem on this page has happened in real projects. Each
entry says what you see, why it happens, and what to do.

## My work "went back" to an older version in the middle of a session

**Why.** Some versions of the Claude app re-send the project
instructions on every turn. If those instructions say "unzip the
state", the old zip gets unpacked again over the work in progress.

**What to do.** Remove every startup instruction from the project
instructions (see step 3 of the [quickstart](quickstart.md)).
In the session, tell Claude not to unpack anything again and to
restore from what it has done; the readme's guard prevents it from
happening again. Projects on standard v5 are protected by design;
older ones should [`upgrade`](operations.md).

## Claude says it cannot find the zip

**Why.** Depending on the version of the Claude app, uploaded files
land in different folders, sometimes with a code added in front of
the name (`79d408a3-novel-state-…zip`).

**What to do.** Nothing, on v5: the readme looks in every known
place and accepts the prefix. If it still finds nothing, check the
file name: it must contain `yourslug-state-`. On older standards,
`upgrade`.

## Claude found more than one state zip

**Why.** Zips from an earlier session can still be lying around.

**What to do.** Claude uses the newest one and tells you which. If it
picked the wrong one, say so before doing any work: nothing has been
edited yet, and it can unpack the right one.

## Claude stops at close: a handoff is missing

**Why.** A handoff got lost at some point — a zip that was rebuilt
without it, a session that died. Claude checks the history before
closing so that losses do not go unnoticed.

**What to do.** Look in your older zips: if the missing handoff is in
one of them, give it back to Claude and it goes where it belongs. If
it is gone for good, tell Claude; it leaves a *tombstone* in its
place recording the loss, and the close goes ahead. This only has to
be done once per gap.

## The session filled up before I could close it

**Why.** Very long sessions eventually run out of context. A session
in that state can no longer do anything, and its files cannot be
reached from a new one.

**What to do.** If there was an intermediate packaging, start a new
session from the `…-interim.zip` files: it is the *rescue* of the
lost session and picks up from there. If there was none, start from
the last zips you have, upload any files Claude handed you during the
lost session, and explain what happened. To avoid it next time,
accept Claude's proposals of intermediate packaging, and attach big
outputs as files rather than pasting them (see
[How a session works](sessions.md)).

## Claude closed (or packaged) when I only wanted a break

**Why.** It should not happen: closing is your decision, and a pause
is not a close. The closing protocol in `setup.md` says so in its
first lines.

**What to do.** Say so, and tell Claude the session is still open:
the close is simply redone at the real end, with the same session
number, replacing the zips and the handoff it produced too early.
If your project's `setup.md` lacks that rule (states brought to v5
before release 5.2), [`audit`](operations.md) reports it; ask Claude
to copy the paragraph from the setup template into it.

## Something else

If you find a problem in Metaproject itself, open an issue on
[GitHub](https://github.com/JosepMariaBlasco/metaproject/issues).
Inside a project, Claude can note it in `framework-feedback.md` for
you.
