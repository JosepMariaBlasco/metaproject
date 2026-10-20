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
