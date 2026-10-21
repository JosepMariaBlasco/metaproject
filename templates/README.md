# Templates

The files a new project's state is made from. `create.md` and
`migrate.md` copy them into the state, replacing the placeholders
(`{{PROJECT_NAME}}`, `{{PROJECT_SLUG}}`, `{{USER_NAME}}`…). The
layout mirrors the state's.

| Template | Becomes, in the state |
|---|---|
| `setup-standalone.md`, `setup-external.md` | `setup.md` — one or the other, depending on where the project lives. Both carry the version marker of the standard. |
| `closing/readme.md` | `closing/readme.md` — the startup file uploaded loose at each session. |
| `handoff.md` | `handoff.md` — the first handoff, of session v1. |
| `conventions.md` | `conventions.md`, with the standard starting conventions. |
| `router.md` | `router.md` — core, imported and personal skills. |
| `agents.md` | `agents.md` — the map of the project's material. |
| `todo.md` | `todo.md` — the to-do list. |

This page is not copied into projects.
