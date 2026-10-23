# Setup — {{PROJECT_NAME}}

*Metaproject standard: v5.*

This project works with two independent zips. The **state zip**
contains cross-session continuity: handoffs, conventions, pending
tasks, project map and reference skills. You receive it at the
start of every session. The **project zip** contains the project's
own material — whatever the project is made of. You will unpack it
at the start of each session alongside the state zip.

## Canonical paths

| What | Fixed path |
|---|---|
| State zip | → `/home/claude/state/` |
| Project zip | → `/home/claude/project/` |

## Session startup — reinjection-proof

The startup instruction (unzipping the zips) does **not** live in
the project instructions. Some orchestrators reinject the project
instructions on **every turn**, not just the first; an `unzip -o`
reinjected mid-session would overwrite the live, edited state on
disk with the incoming (older) zips, silently destroying the
session's work.

The startup instruction lives instead in `closing/readme.md`, which
{{USER_NAME}} hands over, loose, together with the zips at the start
of each session. It wraps the decompression in a guard anchored to
this file (`if [ ! -f /home/claude/state/setup.md ]; then … fi`), and
looks for the uploads in `/mnt/user-data/uploads/`,
`/mnt/user-data/attachments/` and `/root/.claude/uploads/` (the last
two are where some environments place them, in a per-session folder
and with a hash prefix in the file name).

The project instructions hold no startup instruction. They may hold
lines that do no harm when reinjected, such as this one, which only
acts before state exists: *"If `/home/claude/state/setup.md` does not
exist yet, look for an uploaded `readme.md` and follow it."* (`create`
recommends it together with a line fixing the interaction language.)
Otherwise {{USER_NAME}} tells you to follow the readme.

`setup.md` is the anchor precisely because it is always present and
the closing routine never moves or renames it. The guard makes the
decompression idempotent: running it N times equals running it once.
If state ever appears to "revert" to an older version mid-session,
that is the reinjection bug — do **not** decompress again; restore
from your working copy and continue.

## Inbox files

Some files in state exist only when there is something pending;
their mere presence is the signal. Before anything else in this
session, check whether any of them exist, and for each one that
does, follow its protocol below. These are the only imperative
reads in the startup flow — everything else is on-demand.

### `pending-tasks.md`

If this file exists, read it and propose each entry to the user
before proceeding with the regular startup flow. Each entry is
something a previous session decided needed to happen but could
not or preferred not to do at the time.

For each entry, ask the user whether to execute it now. If they
say yes, do it conversationally and remove the entry from the
file — and if that was the last entry, delete the file itself.
If they say no, leave the entry where it is; it will come up
again next session.

### `framework-feedback.md`

If this file exists, it holds feedback this project has recorded
*about the framework itself* (a friction in the protocol, a
suggestion, a correction), waiting to be collected. Mention to the
user that there is uncollected framework feedback and show the
entries. Do not act on them as if they were tasks for this project,
and do not empty the file on your own — it is cleared only when the
user confirms the feedback has been carried back to the framework.

Entries get *into* this file during the session: when something
comes up that is feedback for the framework rather than work for
this project, propose recording it ("this looks like a suggestion
for the framework — shall I note it in the feedback mailbox?") and,
only with the user's approval, append it here. Never record it
unprompted.

### `interim-vN.md`

If a file `interim-vK.md` exists (for some number K), the zips you
were given are an **interim packaging** of session vK, which never
reached its close (see *Intermediate packaging* below). Read it and
tell the user. This session is the **rescue** of vK, not a new
session: it continues vK's work and closes with vK's number — the
handoff you read at startup is still the incoming one, so the
closing protocol below applies unchanged, with vK as this session's
number. At close, fold the content of `interim-vK.md` into the new
handoff and delete the file.

## State files

- `setup.md` — this file.
- `conventions.md` — working conventions, terminology.
- `todo.md` — to-do list ordered by priority.
- `agents.md` — map of the project's material.
- `handoffs/` — handoff history. No handoff is ever deleted —
  they constitute the project's memory.
- `skills/` — skill collection. See the router for the structure
  and what is active.
- `router.md` — skill router pointing to the correct documents.
- `closing/readme.md` — the startup file {{USER_NAME}} hands over
  at the start of each session. See *Session startup* above.
- `pending-tasks.md` — present only when a previous session left
  something to be picked up. See the Inbox files section above.
- `framework-feedback.md` — present only when this project has
  recorded feedback about the framework, waiting to be collected
  by the maintainer. See the Inbox files section above.
- `interim-vN.md` — present only in an interim packaging of a
  session that has not closed yet. Not a handoff. See the Inbox
  files section above.

## What to read on startup

Read four files at startup: this file, the handoff, the
conventions and the router. The conventions (`conventions.md`) are
read preventively because they apply to any work in the project;
deciding whether they are needed would require knowing what they
say. The router (`router.md`) is not content but the index of
content — the map of which skills and documents exist and when to
read them — so deferring it would mean not knowing what you might
need. Do not read anything else from state preventively — the rest
is for on-demand reference:

- `/home/claude/state/agents.md`
- `/home/claude/state/todo.md`
- `/home/claude/state/skills/`
- `/home/claude/state/handoffs/`

Look things up when you need them: the project map when you're
about to modify the project, the to-do list if the session has no
clear goal from the start.

## Project setup

*Pending configuration. Document here any operations the project
needs to be set up or exercised — running tests, building outputs,
or whatever applies to this project.*

## Intermediate packaging

Long sessions can die before they close (the context fills up, the
environment is reclaimed), and the work done since the last
packaging dies with them. When enough work has accumulated that
losing it would hurt (a milestone, new files, substantial
decisions), before a long pause, or when the user asks about the
context, **propose** an interim packaging; the user decides.

Let vM be the number this session will close with (see *At the end
of each session*).

1. Write or update `/home/claude/state/interim-vM.md`: a short note —
   not a handoff — with what has been done so far, stating that the
   session is still open.
2. Package both zips, complete, as
   `{{PROJECT_SLUG}}-state-YYYYMMDD-vM-interim.zip` and
   `{{PROJECT_SLUG}}-project-YYYYMMDD-vM-interim.zip`, after
   removing any earlier interim zips from the output directory.
3. Deliver them to the user (how to deliver, and where to package,
   as *At the end of each session* says).

Do not archive or touch `handoff.md` during an interim packaging.

## At the end of each session

A session closes only when the user asks for it, or confirms it
when you propose it. A pause is not a close: if the user says they
are taking a break, going for lunch or carrying on later, do not
archive the handoff or package the closing zips. (Proposing an
intermediate packaging before a long pause is a different thing:
see *Intermediate packaging*.) When the work seems to have reached a
natural end, or the context is getting heavy, propose closing (or an
intermediate packaging) and wait for the answer.

* Update `/home/claude/state/agents.md` if the project's structure has changed.

* **Session numbers.** Sessions are numbered with an absolute
  counter: a monotonic number that increments once per session
  across the whole project history, not a per-day counter. In this
  section, **vN** is the number of the *incoming* handoff — the
  `handoff.md` you read at startup, written by the previous session
  — and **vM** is *this* session's number: vM = v(N+1), except in a
  rescue session, where vM is the number of the interrupted session
  (see the `interim-vN.md` inbox file). Each handoff is archived
  under the number and date of the session that wrote it.

* **Verify handoff continuity before archiving.** List
  `/home/claude/state/handoffs/` and confirm that the incoming
  handoff's number vN is exactly the next after the highest archived
  one, with no number missing below it. If you find a gap or any
  other break in the sequence, **stop**: do not archive, do not
  package, do not close. Tell the user exactly what you found and
  wait for their explicit decision.

  If the user confirms that a missing handoff is lost for good,
  write a **tombstone** in its place, so that the sequence is
  continuous again and the loss is recorded where it happened: a
  file `handoffs/handoff-YYYYMMDD-vK.md` (K the missing number; the
  date, today's) containing only a title `# Handoff vK — lost` and a
  line saying when the loss was confirmed and what is known about
  it. A tombstone is never replaced, even if the handoff turns up
  later — the recovered handoff is archived next to it with a
  `-recovered` suffix.

* **Archive the handoff by copy, never by move,** so that state is
  never left without a `handoff.md`, not even for an instant:

  1. Copy `/home/claude/state/handoff.md` to
     `/home/claude/state/handoffs/handoff-YYYYMMDD-vN.md` (date and
     number of the incoming handoff). Verify the copy exists.
  2. Only then overwrite `/home/claude/state/handoff.md` with the
     new handoff, reflecting what was done in this session (whose
     number is vM). If an `interim-vM.md` exists, fold its content
     into the new handoff and delete it.

* Package the complete state zip (all files in
  `/home/claude/state/`, not just the ones that changed). Name:
  `{{PROJECT_SLUG}}-state-YYYYMMDD-vM.zip` (this session's number,
  vM). Make sure the output directory exists and holds no interim
  zips of this session any more, and always package
  from `/home/claude/` so that the zip contains `state/` as its root
  directory.

  How the files reach {{USER_NAME}}: if the environment gives you a
  tool for sending files to the user, deliver every file with it,
  the loose `closing/readme.md` included, even when
  `/mnt/user-data/outputs` is also shown (at worst a file arrives
  twice). Package in `/mnt/user-data/outputs` as below; only if it
  cannot be written, use another writable directory (the session's
  scratchpad, say) in every command of this section and of
  *Intermediate packaging*.

  ```bash
  mkdir -p /mnt/user-data/outputs
  rm -f /mnt/user-data/outputs/*-interim.zip
  cd /home/claude && zip -r /mnt/user-data/outputs/{{PROJECT_SLUG}}-state-YYYYMMDD-vM.zip state/
  ```

* Also package the project zip with all files in
  `/home/claude/project/`. Name:
  `{{PROJECT_SLUG}}-project-YYYYMMDD-vM.zip`.

  ```bash
  cd /home/claude && zip -r /mnt/user-data/outputs/{{PROJECT_SLUG}}-project-YYYYMMDD-vM.zip project/
  ```

* **Verify that the outgoing state zip keeps every archived
  handoff** the incoming one had, plus the one just archived and any
  tombstones written this session. Handoffs have been lost before by
  erosion between repackagings, not only by a single failure. The
  incoming state zip is the one deployed at startup (the newest
  matching upload):

  ```bash
  IN=$(find -H /mnt/user-data/uploads /mnt/user-data/attachments /root/.claude/uploads \
         \( -name '{{PROJECT_SLUG}}-state-*.zip' -o -name '*-{{PROJECT_SLUG}}-state-*.zip' \) \
         -printf '%T@ %p\n' 2>/dev/null | sort -rn | head -1 | cut -d' ' -f2-)
  echo "Incoming: $IN"
  unzip -l "$IN" | grep -c 'state/handoffs/handoff-'
  unzip -l /mnt/user-data/outputs/{{PROJECT_SLUG}}-state-YYYYMMDD-vM.zip | grep -c 'state/handoffs/handoff-'
  ```

  The second count must be the first plus one (plus the tombstones
  written this session). If it is not, stop and tell the user. If
  there is no incoming state zip (the first close of a project just
  created or migrated), skip this check.

* Deliver the zips, and deliver `/home/claude/state/closing/readme.md`
  **loose**, as a file of its own outside the zips, so that
  {{USER_NAME}} has it at hand before anything is decompressed next
  session. It cannot be read from inside the state zip without
  first decompressing — the very thing it explains how to do — so it
  travels both ways: inside the zip (canonical copy) and loose at
  every close.
