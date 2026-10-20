# edit-todo

How `state/todo.md` is maintained across sessions.

Read this when you are about to add, remove or reorder entries in
the todo.

## Closed tasks do not remain in the todo

When a task is done, it is removed from the todo — not struck
through, not moved to a "done" section, not annotated. Removed.

The rationale: the todo is a view of actionable future. The
completed past lives in the handoffs, where it is already
recorded with full context (what was done, why, how). Keeping
closed tasks in the todo duplicates information, dilutes priority,
and makes the file grow without bound.

## Renumbering is allowed, but not required

Because closed tasks disappear, numbering will naturally develop
gaps. That is fine. **Do not renumber to close the gaps routinely
— references in past handoffs would become incorrect.** If the
gaps become genuinely disruptive (very large, or the numbering
starts far from 1 for no reason), renumbering is a legitimate
operation; it relies on the double-anchor convention of handoffs
(see `skills/core/write-handoff.md`) to keep old references
findable.

In short: numbers are ephemeral, meant to be renumbered when
useful. The short descriptions that accompany them in handoffs
are the durable anchor.

## What an entry looks like

No strict format. Typical shape is a numbered item with a short
descriptive title and, when useful, a sub-bullet or two with
context, constraints or links into the project.

Tags in square brackets (`[meta]`, `[blocked]`, `[docs]`, etc.)
are a common lightweight way to group or flag entries; use them
as the project finds helpful, not by prescription.

## What goes here, what does not

- **Goes here:** concrete actionable tasks for this project.
- **Does not go here:** the history of what was done (handoffs),
  project conventions (`conventions.md`), reference material
  (`skills/`), ideas that are not yet decisions (keep them in
  the session's handoff or somewhere else until they crystallise).
