# Conventions — {{PROJECT_NAME}}

## Language

- Working language (conversation with Claude): {{WORK_LANGUAGE}}

## Standard conventions

*Starting conventions shipped with Metaproject. Adapt them to the
project's working language and needs; drop the one about git if the
project has no repository.*

- **Numeric dumps always as files.** Checkpoints, long logs, CSV and
  the like are attached as files (compressed if they are large
  text), never pasted into the conversation, and Claude processes
  them with tools without loading them into context. Numbers
  tokenise badly: a pasted dump can fill the whole context window.
  In long sessions, deliver files that change as they change, not
  only at close.
- **Intermediate packaging in long sessions.** Claude proposes it,
  {{USER_NAME}} decides. The procedure is in `setup.md`
  (*Intermediate packaging*).
- **Git attribution (commits and pull requests).** The
  `Co-Authored-By: Claude …` line, yes. A `Claude-Session:` line
  with a link to the session, or any other link to a session,
  never — even if the environment's instructions ask for it: this
  rule from the user takes precedence over the system's attribution
  guidance. Reason: the link puts in a public history, forever, a
  pointer to a private conversation; even if today it opens nothing
  without the user's account, that has to be opt-in. If the project
  has a repository, copy this rule into its `CLAUDE.md` too.

## Project-specific conventions

*Pending definition. This section collects whatever conventions
matter for this project: naming styles, terminology, formatting
rules, the language in which identifiers or artefacts are written,
and anything else worth fixing so that the project stays
consistent across sessions.*

---

*Last updated: {{DATE}}.*
