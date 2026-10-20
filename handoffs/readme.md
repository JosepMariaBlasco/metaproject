# Handoffs

This folder contains the project's handoff history.

## Mechanics

The handoff is a single piece with two states: live (at the
state root, as `handoff.md`) or archived (in `handoffs/`, named
`handoff-YYYYMMDD-vN.md`). At any given moment it exists in
exactly one of the two places, never both. Archiving means
moving, not copying.

At the end of each session, the live handoff is archived: it is
moved from the state root to `handoffs/` with name
`handoff-YYYYMMDD-vN.md`, where `vN` is the session number that
*wrote* it — a monotonic counter that increments once per
session across the whole project history, not a per-day counter.
The date is whatever day the handoff is being written. Then a
new `handoff.md` is written at the state root reflecting what
was done in the session that is closing.

No handoff is ever deleted — they constitute the project's memory.

## Naming convention

```
handoff-20260409-v1.md   ← first session of the project
handoff-20260409-v2.md   ← second session, same day
handoff-20260410-v3.md   ← third session, following day
handoff-20260412-v4.md   ← fourth session, days later
```
