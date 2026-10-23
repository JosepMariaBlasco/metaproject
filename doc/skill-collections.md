# Skill collections

A **skill** is a document that teaches Claude how to do one thing
well. A **skill collection** is a published set of skills on some
subject — a programming language, a publishing tool, a field of
work — maintained by someone who knows it, and shared so that many
projects can use the same know-how.

Metaproject itself ships only its own *core* skills. Everything else
comes from collections.

## Using a collection

When you create a project, Claude asks whether you want skills from a
collection. You can also add them later: just ask. Claude then:

1. gets an exact copy of the skills you choose — from the
   collection's zip or repository (if Claude cannot download it,
   it asks you to upload it);
2. puts them in the project's `skills/`;
3. lists each one in the router, under *Imported*, together with its
   **Source** — the address of the collection.

Import only what the project uses; a skill that is not in the router
is not read.

## Keeping skills up to date

Collections improve: a fix, a new section, a renamed skill. Running
[`update`](operations.md) compares each imported skill with its
source and tells you what changed. New skills in the collection are
proposed, never copied without asking; skills you have changed
locally are never overwritten without your agreement; and a skill
renamed upstream is detected and its replacement proposed.

## Publishing your own collection

Anything works as a collection as long as it gives an exact copy of
its skills: a website with a zip, or a Git repository. A few
conventions make a collection pleasant to use:

- **One skill per Markdown file**, named after its topic, in
  lowercase with hyphens (`house-style.md`).
- **A short header** in YAML with a `name` and a `description` that
  says what the skill does and when to read it. It helps people (and
  Claude) decide whether to import it.
- **Self-contained.** A skill should say which versions of tools it
  assumes and what environment it expects, and depend on other skills
  only by name.
- **A zip of the whole collection**, with a stable address for the
  latest version, so that `update` has something exact to compare
  against.
- **Stable names.** Rename a skill only when you must, and say so in
  the collection's notes, so that projects can follow.

## An example

[Rexx Skills](https://rexx.epbcn.com/skills/) is a collection for
working with the Rexx programming language (ooRexx and Regina):
installing and running the interpreters, serving scripts on the web,
building documentation, and the pitfalls of the language. It is
published as individual pages plus a zip,
`rexx-skills-latest.zip`, and kept in a public repository,
[`JosepMariaBlasco/rexx-skills`](https://github.com/JosepMariaBlasco/rexx-skills):
a clone is the easiest exact copy for `update` to compare against.
