# upgrade

Moves a project's state from an older version of the Metaproject
standard to the current one. Unlike `update`, which synchronises
content within the same version, `upgrade` changes the state's
*structure* and conventions.

Upgrades are **cumulative**: each hop between consecutive versions
(v1 → v2, v2 → v3, etc.) lives in its own `upgradeN-M.md`. The
orchestrator below detects the state's current version and applies
the hops in sequence until the state is at the current standard.

## Detecting the current version

Read `/home/claude/state/setup.md` and look for the marker:

```
*Metaproject standard: vN.*
```

This line appears near the top, under the title. If the marker
is absent, the state predates versioning — treat it as **v1**.

Some states carry a hybrid marker of the form

```
*Metaproject standard: v3 + reinjection-proof startup (…).*
```

These adopted the v5 startup and closing rules by hand before v5
existed, copying them from another project. Treat them as **v3**: apply `upgrade3-4` as usual, and
`upgrade4-5` will recognise what is already in place and complete
only what is missing.

## Available hops and current standard

The current version of the standard is recorded in two redundant
channels that must agree:

1. **The hops.** The `upgradeN-M.md` files sitting next to this
   document. List them:

   ```bash
   ls /path/to/metaproject/upgrade*-*.md
   ```

   Each file name has the shape `upgradeN-M.md`, meaning "takes
   the state from vN to vM". The current version derived from
   this channel is the highest M among the available hops.

2. **The setup templates.** The line

   ```
   *Metaproject standard: vN.*
   ```

   near the top of both `templates/setup-standalone.md` and
   `templates/setup-external.md`. Both templates must declare
   the same N.

**Coherence check.** Before doing anything else, verify that the
three numbers agree: `max(hops)`, the N in `setup-standalone.md`,
and the N in `setup-external.md`. If any of the three diverges,
**stop immediately** and report the divergence to the user. Do
not attempt to pick a winner — the divergence is the bug, and
the fix belongs upstream in Metaproject, not in this operation.

When the three agree, that number is the current version of the
standard. If the state is already at the current version, there
is nothing to do — tell the user and stop.

## Procedure

1. **Working language.** Ask the user what language they want to
   work in. Default is English.

2. **Detect current version** as described above. Tell the user
   what was found.

3. **Discover the current standard** by listing `upgradeN-M.md`
   in Metaproject, reading the version marker in both setup
   templates, and verifying that the three agree as described
   above. If they diverge, stop and report.

4. **Plan the chain of hops** from the project's current version
   to the current standard. If the project is at v1 and the
   standard is v5, the chain is `upgrade1-2 → upgrade2-3 →
   upgrade3-4 → upgrade4-5`. Show the chain to the user before
   executing.

5. **Execute each hop in order.** For each, read the
   corresponding `upgradeN-M.md` and follow its instructions.
   After each hop:
   - Update the version marker in `setup.md` to the new version.
   - Verify that the resulting state is coherent before moving
     to the next hop.

6. **Propose running `update`.** After the final hop, the state
   is *structurally* at the current standard but its distributed
   content (skills, `handoffs/readme.md`) may not yet be — only `update` synchronises
   that. Propose to the user running `update` now, in the same
   session.

   - If the user agrees, execute `update.md` before closing.
   - If the user prefers to do it another time, write an entry
     in `/home/claude/state/pending-tasks.md` (create the file
     if it does not exist) of the form:

     ```
     - Run `update` to complete the upgrade vN→vM.
     ```

     where vN and vM are the original and final versions of this
     upgrade. Also mention it in the handoff.

7. **Close the session** with the usual protocol: archive the
   current handoff, write a new one describing the upgrade (and
   the deferred update if applicable), and package, all as the
   closing protocol in `setup.md` says.

## Principles

- **Do not destroy information.** Rename and reorganise. If
  something needs to be removed, it must be an explicit decision
  by the user.
- **Respect the history.** Old handoffs stay as they are, even
  if they refer to files with old names or in old locations.
  They are the project's memory.
- **Go at the user's pace.** If a hop is large and the user
  prefers to stop between hops, stop — upgrades do not need to
  finish in one session.
- **Each hop is self-contained.** An `upgradeN-M.md` assumes the
  state is at vN and leaves it exactly at vM. It does not skip
  versions. The one allowance: a hop may recognise pieces of vM
  that a state adopted by hand ahead of time, and then completes
  only what is missing (see the hybrid marker above).
