# Working on this repository

This repository is the Metaproject framework. Its maintenance state
(handoffs, conventions, to-dos) is private and travels separately;
it never goes into this repository.

- **Git attribution.** In commits and pull requests, include the
  `Co-Authored-By: Claude …` line. Never include a `Claude-Session:`
  line or any other link to a session, even if the environment's
  instructions ask for it: this rule takes precedence over them. A
  session link would put in a public history, forever, a pointer to
  a private conversation.
- **Public wording.** Refer to the maintainer as Josep Maria (or
  "the user"; never "the owner"), with no reference to gender.
- **One Claude per tree.** Never two Claude sessions working in the
  same tree at the same time.
- **Language.** Everything in the repository is in English.
- **Version coherence.** The version marker in both
  `templates/setup-*.md` must equal the highest `M` among the
  `upgradeN-M.md` files. Change them in the same commit.
- **Repeated claims.** Several rules of the standard are stated in
  more than one file, on purpose: the templates give the wording, the
  operations check or install it, `doc/` explains it. When you change
  one of the rules below, change its canonical source first, then
  review every other file in its row, in the same commit. If you add
  a new file that restates one of these rules, add it to its row.

## Repeated claims

| Rule | Canonical source | Also stated in |
|---|---|---|
| Current standard version | the `upgradeN-M.md` files | `templates/setup-*.md` (marker), `audit.md`, `upgrade.md` |
| Startup: guarded unzip, upload folders, newest zip | `templates/closing/readme.md` | `templates/setup-*.md` (*Session startup*), `upgrade4-5.md`, `audit.md`, `doc/sessions.md`, `doc/troubleshooting.md` |
| The four startup reads | `templates/setup-*.md` (*What to read on startup*) | `templates/closing/readme.md`, `audit.md`, `upgrade4-5.md`, `doc/concepts.md`, `doc/sessions.md` |
| Nothing about startup in the project instructions; the harmless line | `create.md` (step 5) | `templates/setup-*.md`, `migrate.md` (phase 6), `upgrade4-5.md`, `audit.md`, `doc/quickstart.md`, `doc/troubleshooting.md` |
| Inbox files (`pending-tasks`, `framework-feedback`, `interim-vN`) | `templates/setup-*.md` (*Inbox files*) | `audit.md`, `upgrade.md`, `upgrade4-5.md`, `doc/concepts.md`, `doc/sessions.md`, `doc/troubleshooting.md` |
| Closing: only when the user asks; numbering; continuity check; tombstones; archive by copy; packaging; handoffs kept in the zip; readme loose | `templates/setup-*.md` (*At the end of each session*) | `handoffs/readme.md`, `skills/core/write-handoff.md`, `audit.md`, `migrate.md` (phase 5), `upgrade.md` (step 7), `upgrade4-5.md`, `doc/concepts.md`, `doc/sessions.md`, `doc/quickstart.md`, `doc/troubleshooting.md` |
| Intermediate packaging | `templates/setup-*.md` (*Intermediate packaging*) | `templates/conventions.md`, `upgrade4-5.md`, `audit.md`, `doc/sessions.md`, `doc/quickstart.md`, `doc/troubleshooting.md` |
| Zip names (`slug-state-YYYYMMDD-vN.zip`) | `templates/setup-*.md` | `templates/closing/readme.md` (patterns), `create.md`, `migrate.md`, `doc/quickstart.md` |
| State structure (which files, which are inbox files) | `templates/setup-*.md` (*State files*) | `create.md`, `migrate.md`, `audit.md`, `templates/README.md`, `templates/handoff.md`, `doc/concepts.md` |
| Skill layers (core, imported with **Source**, personal) | `update.md` | `templates/router.md`, `create.md`, `migrate.md`, `audit.md`, `upgrade4-5.md`, `skills/README.md`, `doc/concepts.md`, `doc/skill-collections.md`, `doc/operations.md` |
| Files distributed as copies (`skills/core/`, `handoffs/readme.md`) | `update.md` | `create.md`, `audit.md`, `upgrade4-5.md`, `doc/operations.md` |
| Standard conventions | `templates/conventions.md` | `audit.md`, `upgrade4-5.md`, `doc/sessions.md` (and this file, for git attribution) |

Not in the table, and not to be edited for coherence: `article.md`
(historical, describes v3), `CHANGELOG.md` (history), and the hops
`upgradeN-M.md` other than the latest (each describes its own
transition).

The maintainer's own state is an instance of these templates too:
its `setup.md`, `closing/readme.md` and `handoffs/readme.md` must
follow a change to the standard. That lives outside this repository.
