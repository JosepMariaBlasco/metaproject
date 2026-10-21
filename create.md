# create

Metaproject protocol for creating the state structure of a new
project. Follow these steps in order.

## Architecture

The standard separates two spaces with fixed absolute paths:

| Space | Path | Contents |
|---|---|---|
| **State** | `/home/claude/state/` | Continuity infrastructure: handoffs, conventions, skills, project map, to-dos. Travels in a zip that the user uploads at the start of each session. |
| **Project** | `/home/claude/project/` | The project's own material — whatever the project is made of. Arrives as a zip too: either shipped alongside state (standalone) or built from a source hosted elsewhere and retrieved when the session needs it (external). |

Fixing these paths is deliberate. It eliminates ambiguity: no file
requires interpretation to locate, which saves tokens and avoids
errors. Whenever the project is in play in a given session — always
in standalone, on demand in external — it sits at the canonical path
and everything inside is then addressable by absolute path. In
external, a session that does not need the project at all simply
leaves the canonical path empty.

Everything that matters must travel in these zips, where it is
versioned and under control. The Files/Knowledge section of the
Claude project (`/mnt/project/`) is not used in principle — if
there are files there, they should be reviewed and moved to state
or project when appropriate.

## Step 1 — Working language

Ask the user what language they want to work in (English,
Spanish, Catalan, etc.). Default is English — if the user
doesn't specify, use English. From this point on, conduct
the entire interaction in that language.

## Step 2 — Review project files

First, check whether there are files in `/mnt/project/` (the
Files/Knowledge section of the Claude project). If there are,
list their contents to the user and propose for each one whether
it should go into state (skills, conventions, references), project
(the project's own material), or be discarded. The goal is to leave the Files
section empty, if at all possible: everything that matters must
travel in the zips,
where it is versioned and under control. If `/mnt/project/` is
empty, skip to step 3 without mentioning it.

## Step 3 — Gather information

Ask the user the following (all in a single interaction):

1. **Project name** — short and descriptive.
2. **Description** — one sentence summarising what it's about.
3. **Slug** — short prefix for zip filenames (e.g. `yaml`,
   `rexxparser`). Propose one derived from the project name
   (lowercase, no spaces) and let the user confirm or change it.
4. **User name** — how the user is called (used in the state
   files so that Claude knows who uploads the zips).
5. **Project hosting** — does the project travel in a zip
   together with state (standalone), or is the project zip built
   from a source hosted elsewhere such as a repository or server
   (external)? If external, how is the project zip obtained?
6. **Skill collections** — does the project use skills from an
   external collection (a published set of skills on some subject,
   such as a programming language)? If so, which collection, and
   which of its skills?

Do not ask about project-specific conventions (naming styles,
terminology, the language of any identifiers or artefacts, etc.)
at this point. The `conventions.md` file is generated with an
empty *Project-specific conventions* section, to be filled in
later as those conventions emerge or are established.

## Step 4 — Generate the structure

Create the following structure in `/home/claude/state/`:

```
state/
├── handoff.md
├── setup.md
├── router.md
├── agents.md
├── conventions.md
├── todo.md
├── closing/
│   └── readme.md       — the startup file, handed over at each session
├── handoffs/
│   └── readme.md
└── skills/
    ├── core/               — distributed with Metaproject, synchronised by `update`
    ├── personal/           — empty, for the project's own material
    └── (imported skills, if any — see instructions below)
```

For each file, use the corresponding template from `templates/`,
substituting the variables:

| Variable | Value |
|---|---|
| `{{PROJECT_NAME}}` | Project name (step 3.1) |
| `{{PROJECT_DESCRIPTION}}` | Description (step 3.2) |
| `{{PROJECT_SLUG}}` | Project slug (step 3.3) |
| `{{USER_NAME}}` | User name (step 3.4) |
| `{{WORK_LANGUAGE}}` | Working language (step 1) |
| `{{DATE}}` | Current date in DD/MM/YYYY format |

For `setup.md`, use the template that matches the project hosting
(step 3.5):

- **Standalone** → `templates/setup-standalone.md`
- **External** → `templates/setup-external.md`

For `closing/readme.md`, use `templates/closing/readme.md`. For an
external project, remove from it everything about the project zip
(the sentence about it, and every line of the guard that handles `$P`): the project
is obtained as *Project setup* says, and a stray project zip uploaded
out of habit must not be unpacked over it.

For `handoffs/readme.md`, copy the one from Metaproject
directly.

For `skills/`, copy `skills/core/` from Metaproject and create
an empty `skills/personal/` as the space for anything
project-local (the personal layer never travels from Metaproject).
If the project uses an external collection (step 3.6), obtain an
exact copy of the chosen skills (the collection's zip or
repository; ask the user to upload it if the environment cannot
download it), place them at the root of `skills/`, and add a row
for each to the *Imported* section of the router, with the
collection's URL in the **Source** column.

## Step 5 — Package and deliver

Make sure the output directory exists, then package. Name the zips
with session number `v1`.

### Standalone project (travels in zip)

Package state and project separately:

```bash
mkdir -p /mnt/user-data/outputs
cd /home/claude && zip -r /mnt/user-data/outputs/{{PROJECT_SLUG}}-state-YYYYMMDD-v1.zip state/
cd /home/claude && zip -r /mnt/user-data/outputs/{{PROJECT_SLUG}}-project-YYYYMMDD-v1.zip project/
```

### External project (hosted elsewhere)

Only state is packaged and delivered here. The project is not shipped
with state — it lives at its external source and is retrieved during
the session when (and if) the task requires it. The retrieval
procedure is documented in `setup.md` under *Project setup*, which
the template leaves as a placeholder for the user to fill in.

```bash
mkdir -p /mnt/user-data/outputs
cd /home/claude && zip -r /mnt/user-data/outputs/{{PROJECT_SLUG}}-state-YYYYMMDD-v1.zip state/
```

### The startup file, and the project instructions

Deliver the zip(s), and deliver `/home/claude/state/closing/readme.md`
**loose**, as a file of its own. Explain to the user how sessions
start from now on:

> At the start of each session, upload the zip(s) **and** this
> `readme.md`, loose. It tells Claude how to deploy the zips safely.
> I will hand you an updated copy at the end of every session.

Then tell the user what to do with the Claude project instructions:
**do not put any startup instruction there** — no `unzip`, no "read
setup". Some orchestrators reinject the project instructions on
every turn, and an `unzip` reinjected mid-session overwrites the
session's work with the incoming zip (see *Session startup* in
`setup.md`). The instructions may stay empty, or hold only things
that do no harm if repeated on every turn. Recommend these two, so
that a fresh session finds the startup file without the user having
to point at it (the first acts only before state exists, so a
reinjection does nothing):

```
If `/home/claude/state/setup.md` does not exist yet, look for an
uploaded `readme.md` and follow it.

Interaction language: {{WORK_LANGUAGE}}. Always use this language
unless the user explicitly switches to another.
```

## Step 6 — Confirm

Inform the user of what has been generated and ask if they want
to adjust anything before closing.

This first session is session `v1`. The `handoff.md` generated
from the template is its handoff: it is archived at the end of the
*next* session, under its own number, following the closing
protocol in the `setup.md` you just generated.
