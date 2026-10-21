# update

Synchronises a project's skills, and its copy of `handoffs/readme.md`,
with their upstreams. Not a blind
copy — it is an intelligent diff that respects local changes.

A project's skills have three origins, and each is synchronised
against its own upstream:

| Layer | Where it lives in state | Upstream |
|---|---|---|
| **Core** | `skills/core/` | Metaproject's `skills/core/` |
| **Imported** | `skills/` root and subdirectories other than `core/` and `personal/` | The external collection named in the **Source** column of the router's *Imported* section |
| **Personal** | `skills/personal/` | None — the project's own space, never synchronised |

Metaproject itself distributes only `skills/core/`. Domain skills (a
language reference, a deployment recipe, a publishing pipeline) are
not part of the framework: they are published and maintained in
their own collections, and a project imports the ones it uses.

Besides the core skills, Metaproject distributes one file outside
`skills/` that projects carry as a copy: **`handoffs/readme.md`**,
the description of the handoff mechanics. It holds nothing specific
to the project, so it is synchronised with the core layer (see
step 2).

## Procedure

### 1. Obtain the upstreams

- **Core:** Metaproject's `skills/core/` and `handoffs/readme.md`,
  from the framework zip or a clone of its repository.
- **Imported:** for each distinct collection named in the router's
  **Source** column, obtain an exact copy of its skills — the
  collection's zip or a clone of its repository. If the environment
  cannot download it, ask the user to upload it. Do not compare
  against a rendered web page or a summarising fetch: the comparison
  must be byte-exact.

If an imported skill has no **Source** (an older state, or a row
added by hand), ask the user where it comes from. If it comes from
nowhere, it is a project-local skill: propose moving it to
`skills/personal/` (and its row to the router's *Personal*
section).

### 2. Compare and act

For each layer, compare the project's files with their upstream.
**`skills/personal/` never participates.**

| Category | Meaning | Action |
|---|---|---|
| **New** | Exists upstream but not in the project. | Core: copy to the project. Imported: **propose** it to the user, but do not copy it unasked — a project imports only the skills it uses. |
| **Identical** | Exists in both and the content is exactly the same. | Nothing. |
| **Modified** | Exists in both but the content differs. | Do not touch — will be reviewed with the user. |
| **Local** | Exists in the project but not upstream. | Do not touch. Report it: it may have been renamed or withdrawn upstream (see below). |

For imported skills, compare only against the collection the skill
declares; a skill that is new in a collection the project does not
use is not news for this project.

**`handoffs/readme.md`.** Compare it with Metaproject's. If it
differs, show the user the difference and propose replacing it with
Metaproject's copy (translated, if the project keeps its state in
another language, but with the same substance). A stale copy is not
harmless: earlier versions of this file described rules the standard
has since changed (a per-day counter, archiving by moving), and they
contradict the project's own `setup.md`.

**Renamed or withdrawn upstream.** A *Local* imported skill whose
upstream no longer has it has usually been renamed or withdrawn.
Look for a *New* skill in the same collection that plausibly
replaces it (same subject, similar content) and propose the
replacement to the user: import the new one, update the router row,
and remove the old one only with the user's explicit agreement.

### 3. Report and resolve conflicts

Present to the user what has been done and what remains pending.
For example:

> I've updated the core skill X. The collection *Some Skills* has a
> new skill, Y (what it does); shall I import it? Two imported skills
> differ from their upstream: A (a new section was added about
> such-and-such) and B (step 3 was reworded). And C no longer exists
> upstream; it seems to have been renamed to D. What would you like
> to do with each one?

If there is nothing new or modified, simply say that everything is
up to date.

### 4. Review the router

Once conflicts are resolved (if any), show the user the current
state of the router: which skills are active, and which are present
in `skills/` but inactive. Ask whether to activate or deactivate
any. Every imported row must keep its **Source**.

## Notes

- This process does not modify Metaproject or any collection. The
  flow is always upstream → project, never the other way around.
- If the user has improved a skill and believes it should be
  promoted upstream (to Metaproject's core, or to its collection),
  that is a separate decision handled manually.
