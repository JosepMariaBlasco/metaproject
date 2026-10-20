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
4. **User name** — how the user is called (used in the project
   instructions so that Claude knows who uploads the zips).
5. **Project hosting** — does the project travel in a zip
   together with state (standalone), or is the project zip built
   from a source hosted elsewhere such as a repository or server
   (external)? If external, how is the project zip obtained?

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
├── handoffs/
│   └── readme.md
└── skills/
    ├── core/               — distributed with Metaproject, synchronised by `update`
    ├── personal/           — empty, for the project's own material
    └── (other skills and subdirectories from Metaproject,
         copied as reference material — see instructions below)
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

For `handoffs/readme.md`, copy the one from Metaproject
directly.

For `skills/`, copy the entire `skills/` folder from
Metaproject, **except `skills/personal/`** (which is the
project's own space and never travels from the Metaproject).
Create an empty `skills/personal/` in the new project as the
space for anything project-local.

## Step 5 — Package and deliver

### Standalone project (travels in zip)

Package state and project separately:

```bash
cd /home/claude && zip -r /mnt/user-data/outputs/{{PROJECT_SLUG}}-state-YYYYMMDD-v1.zip state/
cd /home/claude && zip -r /mnt/user-data/outputs/{{PROJECT_SLUG}}-project-YYYYMMDD-v1.zip project/
```

Remind the user to configure the project Instructions with:

```
# At the start of each session

{{USER_NAME}} uploads two zips: one for state and one for project.
Always unzip them to their canonical paths:

\`\`\`bash
unzip -qo /mnt/user-data/uploads/{{PROJECT_SLUG}}-state*.zip -d /home/claude/
unzip -qo /mnt/user-data/uploads/{{PROJECT_SLUG}}-project*.zip -d /home/claude/
\`\`\`

Read `/home/claude/state/setup.md` and `/home/claude/state/handoff.md`.

Interaction language: {{WORK_LANGUAGE}}. Always use this language
unless the user explicitly switches to another.
```

### External project (hosted elsewhere)

Only state is packaged and delivered here. The project is not shipped
with state — it lives at its external source and is retrieved during
the session when (and if) the task requires it. The retrieval
procedure is documented in `setup.md` under *Project setup*, which
the template leaves as a placeholder for the user to fill in.

Package state only:

```bash
cd /home/claude && zip -r /mnt/user-data/outputs/{{PROJECT_SLUG}}-state-YYYYMMDD-v1.zip state/
```

Remind the user to configure the project Instructions with:

```
# At the start of each session

{{USER_NAME}} uploads a state zip. Always unzip it to `/home/claude/state/`:

\`\`\`bash
unzip -qo /mnt/user-data/uploads/{{PROJECT_SLUG}}-state*.zip -d /home/claude/
\`\`\`

Read `/home/claude/state/setup.md` and `/home/claude/state/handoff.md`.

Interaction language: {{WORK_LANGUAGE}}. Always use this language
unless the user explicitly switches to another.
```

## Step 6 — Confirm

Inform the user of what has been generated and ask if they want
to adjust anything before closing.

When the session ends, apply the session closing protocol
described in the `setup.md` you just generated: move the
current `handoff.md` to `handoffs/` with the standard naming
convention, write a new `handoff.md` reflecting what was done,
and repackage the state zip.
