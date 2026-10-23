# Session startup — {{PROJECT_NAME}}

{{USER_NAME}} uploads the zips for this session: always the **state**
zip, and also the **project** zip if the project travels with state
(standalone). They must be decompressed to their canonical paths, BUT
ONLY ONCE PER SESSION, in your first turn.

**Why "only once":** if you read these instructions again mid-session
(because the orchestrator reinjects instructions on every turn,
because you re-read them, or for any other reason) and run the
`unzip` again, you would overwrite on disk all the work done so far:
the uploaded zips are the INCOMING state, not the current one.
Treating this startup as a recurring order destroys state. To make
that IMPOSSIBLE, the decompression sits behind a guard: it only
happens if state is not deployed yet.

The guard checks whether `/home/claude/state/setup.md` already
exists. If it does, state is already deployed in this session and
nothing must be decompressed. If it does not, this is your first
turn: decompress. (The guard is anchored to `setup.md` because it is
always present and the closing routine never moves or renames it.)

```bash
if [ ! -f /home/claude/state/setup.md ]; then
  # Uploads arrive in /mnt/user-data/uploads/ or, in some environments,
  # in /mnt/user-data/attachments/<session>/<hash>-name or
  # /root/.claude/uploads/<session>/<hash>-name: look in all three.
  # setup.md repeats this list in its closing check: keep both in step.
  # Newest first, so that a zip left over from an earlier session never wins.
  find_zips() {
    find /mnt/user-data/uploads /mnt/user-data/attachments /root/.claude/uploads \
         \( -name "{{PROJECT_SLUG}}-$1-*.zip" -o -name "*-{{PROJECT_SLUG}}-$1-*.zip" \) \
         -printf '%T@ %p\n' 2>/dev/null | sort -rn | cut -d' ' -f2-
  }
  S=$(find_zips state)
  P=$(find_zips project)
  echo "State zip(s), newest first:";   echo "$S"
  echo "Project zip(s), newest first:"; echo "$P"
  if [ -n "$S" ]; then
    unzip -qo "$(echo "$S" | head -1)" -d /home/claude/
    if [ -n "$P" ]; then
      unzip -qo "$(echo "$P" | head -1)" -d /home/claude/
    fi
  else
    echo "STOP: no state zip found. Ask the user."
  fi
fi
```

If the guard found no state zip, stop and ask {{USER_NAME}}. If it
listed more than one zip of a kind, it used the newest: tell
{{USER_NAME}} which one and confirm it before going on. If it was the
wrong one, nothing has been edited yet: move `/home/claude/state/`
aside and decompress the right zip by hand. The same goes for the
project zip in a standalone project: if it is missing, ask for it.

Then — always, whether you decompressed now or state was already
deployed — read, in this order:

1. `/home/claude/state/setup.md` — how to work and how to close,
   including the check for **inbox files** (files whose mere
   presence means something is pending), the only imperative reads.
2. `/home/claude/state/handoff.md` — state of the previous session.
3. `/home/claude/state/conventions.md` — project conventions (read
   preventively: they apply to any work).
4. `/home/claude/state/router.md` — the index of skills and
   documents, and when to read each.

The rest of state (`agents.md`, `todo.md`, `skills/`, `handoffs/`,
etc.) is for on-demand reference; `setup.md` explains when.

If an edit of yours "reverts" to an old version mid-session, do NOT
assume {{USER_NAME}} uploaded state again: it is the reinjection bug.
Do not decompress again; restore from your work and carry on.
