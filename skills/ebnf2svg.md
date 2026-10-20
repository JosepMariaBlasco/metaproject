---
name: ebnf2svg
description: >
  How to generate SVG railroad diagrams from EBNF grammars using
  rr.war (a Java tool) plus two ooRexx scripts. Covers prerequisites,
  the toolchain, usage, EBNF format notes, and exit codes. Read this
  whenever you need to produce railroad diagrams from an EBNF source.
---

# EBNF → SVG — Railroad Diagram Generation

## Prerequisites

- ooRexx (`oorexx-run.md`)
- Project (*Canonical paths* section in `setup.md`)
- Java 11+: `apt-get install -y default-jre-headless`
- `rr.war` in the project's `bin/`:
  `curl -o /home/claude/project/bin/rr.war https://rexx.epbcn.com/rr-barcelona2026.war`
  If the download fails, ask the user to upload it.

## Toolchain

```
.ebnf → rr.war (Java) → XHTML → extract_svg_from_rr_xhtml.rex → individual .svg files
```

Two ooRexx scripts: `ebnf2svg.rex` (orchestrator) and
`extract_svg_from_rr_xhtml.rex` (extractor). Both in `bin/`.

## Usage

```bash
cd /home/claude/project
rexx bin/ebnf2svg.rex path/to/grammar.ebnf
```

The `.ebnf` extension is optional. SVGs are generated in the same
directory as the EBNF file.

## EBNF format

```
production_name ::= definition
another_name ::= 'KEYWORD' ( option1 | option2 ) suffix?
```

Elements: `'KEYWORD'` (terminal), `name` (non-terminal),
`( ... | ... )` (alternatives), `?` (optional), `*` (zero or more),
`+` (one or more), `/* ... */` (comments).

Each production generates an SVG named after it.

## Exit codes

0=OK, 3=EBNF not found, 4=rr.war not found, 5=Java not
found, 7=rr.war error, 8=no output.
