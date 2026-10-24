# Operations

Metaproject has five operations. Each is a document at the root of
the repository, written as instructions for Claude: you give Claude
the framework and say *"follow `create.md`"* (or whichever applies),
and it drives the session from there.

| You want to… | Operation | What happens |
|---|---|---|
| Start a new project | [`create.md`](../create.md) | Claude asks a few questions and builds a fresh state. |
| Bring an existing project into Metaproject | [`migrate.md`](../migrate.md) | Claude looks at what the project already has (notes, instructions, files), proposes how it maps onto the standard, and does it once you agree. May take several sessions. |
| Check that a project follows the standard | [`audit.md`](../audit.md) | Claude lists what conforms and what deviates, comparing the state with the current templates. Then, if you want, it fixes the deviations you approve, one by one. |
| Bring a project's skills up to date | [`update.md`](../update.md) | Claude compares each skill with where it came from — Metaproject for core skills, its collection for imported ones — and asks you about every difference. It also offers to refresh the project's copy of `handoffs/readme.md`. |
| Move a project to a newer version of the standard | [`upgrade.md`](../upgrade.md) | Claude applies the steps between versions (`upgrade3-4.md`, `upgrade4-5.md`…) one at a time, then offers to run `update`. |

## Which one, when

- **New project:** `create`.
- **A project you have been running with Claude another way** — long
  instructions, a notes file, your own handoffs: `migrate`.
- **A project created with an older Metaproject:** `upgrade`. If you
  are unsure, run `audit` first; it tells you which version the
  state is on.
- **A project already on the current version, after a maintenance
  release** (for instance v5.5): `audit`, and accept the fixes you want.
  `upgrade` proposes the same when it finds there is no hop to apply.
- **A skill collection you use has published fixes:** `update`.

## Things all operations respect

- **Nothing is destroyed without your say-so.** Files are renamed and
  reorganised; anything removed is removed because you decided it.
- **History is kept.** Old handoffs stay as they are, even when they
  mention files by old names.
- **Your pace.** An operation can stop between steps and continue in
  a later session.
