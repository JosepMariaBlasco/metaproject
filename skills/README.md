# Skills

A skill is a document that teaches Claude how to do one thing well.
Metaproject ships only its own skills; this folder is copied, in
part, into every project's state.

| Folder | What it holds |
|---|---|
| `core/` | Skills about Metaproject's own processes: how to keep the to-do list (`edit-todo.md`) and how to write a handoff (`write-handoff.md`). Copied into every project and kept in sync by `update`. |
| [`personal/`](personal/readme.md) | A placeholder. In a project, this is where its own skills live; nothing here is ever synchronised. |

Domain skills — a programming language, a publishing tool, a house
style — are not part of the framework. They come from
[skill collections](../doc/skill-collections.md), and a project
imports the ones it uses.

This page is not copied into projects.
