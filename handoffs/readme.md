# Handoffs

This folder contains the project's handoff history.

## Mechanics

The live handoff sits at the state root as `handoff.md`; archived
handoffs sit here, named `handoff-YYYYMMDD-vN.md`, where `vN` is the
number of the session that *wrote* it — a monotonic counter that
increments once per session across the whole project history, not a
per-day counter — and the date is the day it was written.

At the end of each session the live handoff is archived **by copy,
never by move**, so that state is never left without a `handoff.md`,
not even for an instant: first it is copied here under its own
number and date, the copy is verified, and only then is `handoff.md`
overwritten with the new handoff of the session that is closing.

Before archiving, the session verifies continuity: the incoming
handoff's number must be exactly the next after the highest one
archived here, with no number missing below it. A gap means a
handoff has been lost; the session stops and asks the user. If the
user confirms the loss, a *tombstone* takes the missing number: a
file with that number titled `# Handoff vK — lost`, recording when
the loss was confirmed. Tombstones are never replaced. The full
procedure is in `setup.md`, under *At the end of each session*.

No handoff is ever deleted — they constitute the project's memory.

## Naming convention

```
handoff-20260409-v1.md   ← first session of the project
handoff-20260409-v2.md   ← second session, same day
handoff-20260410-v3.md   ← third session, following day
handoff-20260412-v4.md   ← fourth session, days later
```
