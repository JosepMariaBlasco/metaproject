---
name: rexxpub-slides
description: >
  How to author RexxPub slide decks in Markdown. Covers the required
  YAML front matter, the title-page and closing-page blocks, how to
  structure content slides and section dividers, typography and
  layout defaults, supported elements (lists, code, tables,
  blockquotes, footnotes), and the commands to render the PDF.
  Read this whenever you need to create or edit a presentation
  using the `slides` docclass.
---

# RexxPub Slides

## When to use this skill

When you need to create or edit a presentation in RexxPub slides
format. The source file is Markdown with specific conventions
described here.

## Structure of a slide deck

### YAML front matter

```
---
rexxpub:
  docclass: slides
  style: print
---
```

`docclass: slides` is required if the file is not named
`slides.md`. `style` controls the highlighting of Rexx blocks
(options: `dark`, `print`, `light`, etc.; default `dark`).

### Title page

```
::::: title-page

Presentation Title <small>Subtitle</small>
==========================================

::: author
Author Name
:::

::: affiliation
Institution
:::

::: event
Event Name
:::

::: venue
City, Dates
:::

:::::
```

Available elements: `author`, `affiliation`, `event`, `venue`,
`email`, `date`. All optional.

### Content slides

Each `<h1>` with `{.newpage}` starts a new slide. The heading
becomes the slide's title.

```
Slide Title {.newpage}
======================

Slide content.
```

Both Setext style (with `===`) and ATX style (with `#`) work.

### Section dividers

An `<h1>` with `{.part .newpage}` produces a section divider:
centred title without a bottom rule. Useful for separating
parts of the presentation.

```
Section Name {.part .newpage}
=============================
```

### Closing page

```
::::: closing-page

Thank You! {.newpage}
=====================

Questions?

email@example.com

:::::
```

## Typography and layout

- **Page size:** 254mm × 142.875mm (16:9, FHD).
- **Font:** Helvetica/Arial, 20pt, left-aligned.
- **No justification or hyphenation** — optimised for projection.
- **Code:** 14pt — smaller than body text but still legible when
  projected.
- **Margins:** 1.5cm top/bottom, 1.8cm sides.
- **Slide numbers:** bottom right, 10pt, grey. Hidden on the
  title page.

## Supported elements

### Lists

Bulleted and numbered lists, with generous spacing for
projection. Nested lists are supported.

### Code

Generic blocks (subtle background) and Rexx blocks with
highlighting:

````
~~~rexx
Parse Arg name
Say "Hello," name"!"
~~~
````

### Tables

`booktabs` style (horizontal rules, no verticals), with an
accent colour.

### Blockquotes

With a coloured side bar (visible when projected):

```
> Blockquote text.
```

### Footnotes

Supported, in a smaller font (12pt).

## Writing conventions for slides

- Keep content brief — slides are visual support, not documents.
- Prefer short lists and direct sentences.
- Code on slides should be concise — snippets, not full programs.
- Use section dividers to mark transitions between parts of the
  presentation.

## Pipeline

To generate the PDF from the command line:

```
[rexx] md2pdf --docclass slides slides.md
```

To view in the browser with pagination:

```
?print=pdf
```
