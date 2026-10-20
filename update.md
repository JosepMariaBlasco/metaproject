# update

Synchronises a project's state with Metaproject's distributed
material. Not a blind copy — it is an intelligent diff that
respects local changes.

Currently the distributed material consists of `skills/` (minus
`skills/personal/`). In the future this may grow to include more
items; the procedure below applies to all of them.

## Procedure

### 1. Compare and act

Recursively compare the contents of the project's `skills/`
(at `/home/claude/state/skills/`) with Metaproject's `skills/`,
**excluding `skills/personal/` on both sides** — that directory
is the project's own space and never participates in
synchronisation.

The comparison includes individual files and entire subdirectories
(e.g. `skills/core/`, `skills/rexxpub/`). For each file or
subdirectory, classify it and act:

| Category | Meaning | Action |
|---|---|---|
| **New** | Exists in Metaproject but not in the project. | Copy to the project. |
| **Identical** | Exists in both and the content is exactly the same. | Nothing. |
| **Modified** | Exists in both but the content differs. | Do not touch — will be reviewed with the user. |
| **Local** | Exists in the project but not in Metaproject. | Do not touch. |

### 2. Report and resolve conflicts

Present to the user what has been done and what remains pending.
For example:

> I've added the new skills X, Y and Z. I've noticed that some
> differ between your project and Metaproject: A (a new
> section was added about such-and-such) and B (step 3 was
> reworded). What would you like to do with each one?

If there are no new or modified skills, simply say that
everything is up to date.

### 3. Review the router

Once conflicts are resolved (if any), show the user the current
state of the router: which skills are active and which are
available but inactive. For example:

> Active skills in the router: `oorexx` (ooRexx compilation and
> execution), `pagedjs-cli` (PDF generation with Paged.js) and
> `ebnf2svg` (syntax diagrams).
>
> Available but inactive skills: `apache-cgi-rexx` (CGI
> deployment with Apache), `rexxpub` (publishing framework).
>
> Would you like to activate or deactivate any?

## Notes

- This process does not modify Metaproject. The flow is
  always Metaproject → project, never the other way around.
- If the user has improved a local skill and believes it should
  be promoted to Metaproject, that is a separate decision
  handled manually.
