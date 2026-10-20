---
name: rexxpub-pipelines
description: >
  Operational detail of RexxPub's two core pipelines: CGI.markdown.rex
  (the Apache CGI handler) and md2html.rex (the batch/CLI tool).
  Covers their differences, shared modules, the pipeline flow,
  option precedence, template placeholders, docclass detection,
  customisation hooks, and the usedStyles hack. Read this whenever
  you need to debug, extend, or customise either pipeline.
---

# RexxPub Pipelines: CGI.markdown.rex and md2html.rex

Dependencies: ooRexx (`oorexx-run.md`), Project (*Canonical paths* section in `setup.md`),
and Pandoc (`apt-get install -y pandoc`).

## The two pipelines

| Aspect | CGI.markdown.rex | md2html.rex |
|--------|-------------------|-------------|
| Location | `cgi/` | `bin/` |
| Runs as | Apache CGI handler | CLI |
| Output | Streamed to the browser | `.html` file |
| Template | `::Resource HTML` (embedded) | `default.md2html` (external) |
| Customisation | `rexx.epbcn.com.optional.cls` | `md2html.custom.rex` |
| Default style | `"dark"` | `"dark"` |
| Batch mode | No | Yes |
| `--citeproc` | Yes | No |

## Shared modules (in `bin/`)

`FencedCode.cls`, `YAMLFrontMatter.cls`, `RexxPubOptions.cls`,
`ErrorHandler.cls`, `CLISupport.cls`, `BaseClassesAndRoutines.cls`.

The CGI loads the first three with `Call Requires` and explicit
`../bin/` paths (because `::Requires` does not resolve `../` well).

## Pipeline flow

1. Read the `.md` file into an array.
2. `YAMLFrontMatter(source)` → extract the YAML front matter.
3. `ParseRexxPubYAML(yaml)` → produce an options directory.
4. Apply option precedence.
5. `FencedCode(filename, source, style, options.)` → process the Rexx blocks.
6. Pandoc → convert Markdown to HTML.
7. Extract the title from the first `<h1>`.
8. Perform template substitution (`%placeholder%`).
9. usedStyles hack → detect used styles and insert `<link>` tags.
10. Write the output.

## Option precedence

**CGI** (the reader can change the style):
- `style`: URL > YAML > default
- Everything else: YAML > URL > default

**md2html**: YAML > default for everything.

## Template placeholders

`%title%`, `%header%`, `%contentheader%`, `%contents%`, `%footer%`,
`%sidebar%`, `%usedStyles%`, `%filenameSpecificStyle%`,
`%sizeSpecificStyle%`, `%highlightStyle%`, `%printStyle%`,
`%printJs%`, `%printSections%`, `%printFigures%`, `%printTOC%`,
`%listingsStyle%`, `%Language%`, `%SectionNumbers%`,
`%NumberFigures%`, `%ListingsAttrs%`, `%FiguresAttrs%`.

Those matching `Strip(Lower(line))` are replaced line-by-line;
those under the `Otherwise` branch are substituted inline.

## Docclass

Determines which CSS is loaded. Detected by filename, overridden
with `docclass:` in YAML. Default: `"markdown"` (loads
`markdown.css`). When it starts with `"print/"`, it loads
`rexxpub-base.css` plus the docclass's CSS.

## Customisation

**CGI**: `Markdown.XXX` routines in `rexx.epbcn.com.optional.cls`,
called via `OptionalCall`.

**md2html**: `md2html.XXX` routines in `md2html.custom.rex`, called
via `OptionalCall`. Lookup order: current dir > destination >
source > `--path` > Rexx external search.

## usedStyles hack

After expanding `%contents%`, scan the output for
`class="highlight-rexx-STYLE"`, gather unique styles, build `<link>`
tags, and replace `%usedStyles%`. This must happen AFTER contents is
expanded, but the `%usedStyles%` line must be BEFORE `</head>`.
