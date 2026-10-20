# upgrade3-4

Moves a state from Metaproject standard v3 to v4.

Assumes the state is at v3. Leaves the state at v4.

## What changes between v3 and v4

The v4 standard makes three related changes to the startup and
feedback protocol, all confined to `setup.md`. None of them
touches the structure of `skills/` or any other file.

### 1. The router becomes a startup read

Until v3, `setup.md` listed `router.md` among the on-demand reads
("read the router when you need a skill"). This was a latent bug:
the router is not *content*, it is the *index of content*. Telling
a session to read it "when you need it" is circular — knowing that
you need a particular skill is precisely what the router tells
you. A session that never opens the index cannot know what the
index would have told it to read, so it cannot know that a given
skill applied to the task at hand.

v4 reclassifies the router as a startup read, alongside `setup.md`
and the handoff. It is cheap (a table of pointers, no technical
content) and it is the map of everything else worth reading, so it
is the worst possible candidate for deferral. The context-economy
that motivates the minimal startup is preserved: a router costs
almost nothing, and reading it prevents the failure of skipping a
skill that did apply.

### 2. The startup check generalises to "inbox files"

v3 introduced a single conditional startup read, `pending-tasks.md`,
with its own dedicated section. v4 generalises the idea rather than
accreting a second special case: the startup defines a family of
**inbox files** — files that exist only when something is pending,
whose mere presence is the signal. The check becomes one
instruction ("check whether any of these files exist; for each one
that does, follow its protocol") instead of an enumeration of
cases that grows every time a new buffer is added.

Each inbox file declares its own protocol for what to do when
present, because they do not all behave the same way:

- `pending-tasks.md` is **inward-facing**: its entries are work to
  be proposed and, if accepted, executed *in this project*. An
  executed entry is removed; when the file empties, it is deleted.
- `framework-feedback.md` (introduced below) is **outward-facing**:
  its entries are messages *for the framework maintainer*, not work
  for this project. When present, the session mentions it so the
  maintainer can collect it; it is **not** emptied by this project,
  only by the maintainer once the feedback has been transported.

### 3. The framework-feedback inbox

v4 introduces `framework-feedback.md`, a deferred mailbox that lets
a project derived from the framework report feedback *about the
framework itself* — a friction in the protocol, a suggestion, a
correction — back to the maintainer, who is typically the only
human operator across many projects and cannot carry every remark
by hand.

The mechanism has two halves:

- **Deposit (assisted, maintainer-approved).** When a session
  detects something that is feedback for the framework rather than
  work for this project, it does not write it on its own. It
  *proposes* recording it ("this looks like a suggestion for the
  protocol — shall I note it in the framework-feedback mailbox?").
  Only if the user agrees does it append the entry to
  `framework-feedback.md`. Same stance as session closing: the
  session may detect and propose, but the act is the user's.
- **Collection (opportunistic sweep).** Because the file lives in
  the derived project's own state, it travels in that project's
  state zip. When the maintainer next works in that project, the
  startup check (generalised above) reports that there is
  uncollected feedback. Transport remains manual — the maintainer
  copies the entry into the framework — but the *reminder* is
  automatic and sits exactly where it is needed: in front of the
  maintainer while that project is open. The file is emptied only
  when the maintainer confirms the feedback has been carried over.

The file is **not created ex-ante** — it exists only when a project
has actually recorded feedback. Plain markdown, one bullet per
entry.

## Procedure

### 1. Reclassify the router as a startup read in setup.md

Open `/home/claude/state/setup.md` and find the **What to read on
startup** section. Currently it names `setup.md` and the handoff as
the startup reads and lists everything else — including
`router.md` — under "Do not read anything else from state
preventively".

Make two edits in that section:

- Remove `/home/claude/state/router.md` from the on-demand list
  (the bullet list under "the rest is for on-demand reference").
- Add the router as an explicit startup read, named alongside the
  handoff, with a one-line reason. For example, rewrite the
  opening of the section so it reads along these lines:

  ```
  After this file, the handoff, and the router you have enough
  context to get started. Read the router (`router.md`) at
  startup too: it is not content but the index of content — the
  map of which skills and documents exist and when to read them —
  so deferring it would mean not knowing what you might need. Do
  not read anything else from state preventively — the rest is
  for on-demand reference:
  ```

  Adjust the surrounding wording to fit the specific phrasing of
  the state being upgraded; the requirement is that the router is
  named as a startup read and no longer appears in the on-demand
  list.

- In the closing sentence of the section ("Look things up when
  you need them: ... the router when you need a skill, ..."),
  remove the clause about reading the router on demand, since it
  is now a startup read.

### 2. Generalise the startup check to inbox files

In the **Pending tasks** section of `setup.md`, the closing line
currently reads:

```
This is the only imperative read in the startup flow. Everything
else is on-demand.
```

Rename the section to **Inbox files** (or keep **Pending tasks**
as a subsection if preferred — see step 3) and restructure it so
that the imperative check covers the whole family. Replace the
"only imperative read" framing with text along these lines:

```
## Inbox files

Some files in state exist only when there is something pending;
their mere presence is the signal. Before anything else in this
session, check whether any of them exist, and for each one that
does, follow its protocol below. These are the only imperative
reads in the startup flow — everything else is on-demand.

### `pending-tasks.md`

[the existing v3 Pending-tasks text: propose each entry, execute
or postpone, delete when empty]

### `framework-feedback.md`

If this file exists, it holds feedback this project has recorded
*about the framework itself*, waiting to be collected. Mention to
the user that there is uncollected framework feedback and show the
entries. Do not act on them as if they were tasks for this project
and do not empty the file on your own — it is cleared only when
the user confirms the feedback has been carried back to the
framework. (How entries get *into* this file is described in the
session conventions / closing protocol: a session proposes
recording framework feedback and, with the user's approval,
appends it here.)
```

Keep the exact wording of the existing `pending-tasks.md`
paragraph from v3 intact inside its subsection; only its framing
("the only imperative read") moves up to the parent section and
becomes plural.

### 3. Add `framework-feedback.md` to the State files list

In the **State files** list of `setup.md`, add one bullet after
the `pending-tasks.md` bullet:

```
- `framework-feedback.md` — present only when this project has
  recorded feedback about the framework, waiting to be collected
  by the maintainer. See the Inbox files section above.
```

### 4. Update the version marker

Change the version marker near the top of `setup.md` from

```
*Metaproject standard: v3.*
```

to

```
*Metaproject standard: v4.*
```

### 5. Do not create the inbox files

Neither `pending-tasks.md` nor `framework-feedback.md` is created
during the hop. Both are created on demand by the operations that
use them. Do not create them.

### 6. Verify

- The **What to read on startup** section names the router as a
  startup read and no longer lists it among the on-demand reads.
- The startup check is generalised to inbox files, covering both
  `pending-tasks.md` and `framework-feedback.md`, each with its
  own protocol.
- The **State files** list mentions `framework-feedback.md`.
- The version marker says v4.
- No inbox files have been created.

### 7. Done

The state is now at v4. Control returns to the orchestrator.
