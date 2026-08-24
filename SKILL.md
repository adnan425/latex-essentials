---
name: latex-essentials
description: >
  Write, compile, fix, and style LaTeX documents of any kind: reports, letters, articles, resumes/CVs, invoices, quotations, proposals, papers, books, presentations (Beamer), or any other PDF built from .tex source. Use this whenever the user mentions LaTeX, .tex files, or asks for a document with professional/print-quality typography, precise page layout, branded headers/footers, ruled tables, mathematical notation, bibliographies/citations, or multi-page structured PDFs. Also use for LaTeX troubleshooting: compile errors (undefined control sequence, missing $, mismatched environments), page-margin/header-footer overlap, table formatting, box/callout environments, font issues, and bibliography compile failures (natbib/BibTeX or biblatex/biber). Trigger even when the user doesn't say "LaTeX" explicitly but describes something LaTeX is well suited for, e.g. "make this look more professional as a PDF," "I need flat ruled tables," "match this reference PDF's layout."
---

# LaTeX

General-purpose skill for producing and fixing LaTeX documents. Not tied to any particular document type. The same core practices apply whether the output is a one-page letter, a 40-page report, an academic paper, or a branded business document.

## When to use this

- The user mentions LaTeX, `.tex`, `pdflatex`/`xelatex`/`lualatex`, or `latexmk`
- A request for a PDF where layout precision matters: consistent margins, ruled tables, running headers/footers, page numbering, footnotes, cross-references, a bibliography
- "Make this look more professional" for something that will become a printed/PDF document
- Matching or replicating an existing PDF's layout
- Debugging a LaTeX compile error or a rendering problem (overlapping elements, broken tables, missing fonts, page breaks in the wrong place)

## Core workflow

### 1. Clarify the document, not just the content

Before writing anything, get clear on: what kind of document this is (letter, report, invoice, CV, paper, slides...), roughly how long it'll be, and whether there's an existing document or brand style to match. Ask for missing structural facts (sections needed, whether it needs a bibliography, table of contents, etc.) rather than guessing. Don't over-ask, though; a short, reasonable document usually needs only 1-2 clarifying questions at most.

If the user has a reference PDF they want matched, ask for it. Reproduce the layout first, then flag anything that's genuinely improvable (cramped tables, weak type hierarchy, poor spacing) rather than blindly cloning weaknesses.

### 2. Pick the right document class and packages

Don't reach for the same boilerplate every time. Match the class to the job:

| Document type | Typical class/packages |
|---|---|
| Letter | `letter` or `scrlttr2`, or `article` with manual header for more control |
| Report / long document | `report` or `scrreprt`, with `tocloft`, `titlesec` for structure |
| Academic paper | `article`, often a journal-provided `.cls`, `natbib`/`biblatex` for citations |
| CV / resume | `article` with `moderncv`, `awesome-cv`, or a hand-built grid layout |
| Slides | `beamer` |
| Business doc (quote/invoice/proposal/branded report) | `article` with `geometry`, `fancyhdr`, `booktabs`, `xcolor`, `tcolorbox` |
| Book | `book` or `memoir` |

Common packages worth defaulting to regardless of class:
- `geometry`: explicit, predictable margins instead of class defaults
- `booktabs`: for any table; never leave default `tabular` vertical rules in a document meant to look finished
- `fancyhdr` + `lastpage`: custom headers/footers with page numbering
- `xcolor`: any color use, defined once via `\definecolor{name}{HTML}{...}`
- `hyperref`: clickable links/cross-references/PDF metadata (load last, after most other packages)
- `fontspec`: only if using non-default fonts, and only with `xelatex`/`lualatex`, never `pdflatex`

### 3. Write the `.tex`

- Escape LaTeX special characters in any user-supplied text before inserting it: `& % $ # _ { } ~ ^ \`. Free text (names, notes, descriptions) will silently break or mis-render otherwise.
- Use semantic structure (`\section`, `\subsection`, environments) rather than manual spacing hacks. It keeps the document maintainable and makes later edits (reordering sections, regenerating a TOC) trivial.
- For tables: `booktabs` rules (`\toprule`/`\midrule`/`\bottomrule`), no vertical rules, `\renewcommand{\arraystretch}{1.2–1.4}` for breathing room.
- For any box/callout that could hold variable-length content (notes, warnings, terms): use `tcolorbox` with the `breakable` option, or it will overflow the page instead of splitting cleanly.

### 4. Compile

```bash
latexmk -pdf -interaction=nonstopmode -halt-on-error document.tex
```

Switch `-pdf` to `-xelatex` or `-lualatex` if the document uses `fontspec` or non-Latin scripts. `-halt-on-error` stops at the first real error instead of burying it under cascading follow-on errors; use it while iterating.

If compilation fails or the output doesn't look right, check `references/troubleshooting.md` (layout/rendering issues) or `references/error-catalog.md` (compile-error messages) before guessing. Most issues are one of a small, well-known set.

### 5. Citations and bibliographies (when the document needs them)

See `references/citations.md` for the natbib/BibTeX vs. biblatex/biber decision, the correct multi-pass compile sequence for each, and the most common "bibliography won't show up" causes. Getting the compile sequence wrong (missing a pass, using `bibtex` against a `biblatex` document) is the most common failure here, not a LaTeX syntax problem.

### 6. Starting from a skeleton (optional)

`assets/skeletons/` has minimal, dependency-light starting points for four common document types: `letter.tex`, `report.tex`, `cv.tex`, `beamer.tex`. Each compiles cleanly on its own and is meant to be edited, not used verbatim. Use one as a starting point when the user's request matches its type and they don't already have their own structure to build from; write from scratch instead when the request is more specific than any skeleton covers (a particular brand style, an unusual structure, matching a reference PDF).

### 7. Verify, don't assume

A clean compile log does not mean the layout is correct. Always rasterize and actually look at the output before calling it done:

```bash
pdftoppm document.pdf preview -png -r 100
```

Check: no header/footer overlap, tables aligned and not overflowing the page width, no orphaned boxes split awkwardly across a page break, and, if there's a reference document, a genuine side-by-side comparison rather than an assumed match.

## Files

- `references/troubleshooting.md`: recurring layout/rendering problems (margin/header conflicts, table styling, box page-breaks, font/engine mismatches) and their fixes
- `references/error-catalog.md`: compile-time error messages (undefined control sequence, missing $, alignment tab, environment mismatch, missing package, overfull/underfull hbox) matched to their actual LaTeX output and fixes
- `references/citations.md`: natbib/BibTeX vs. biblatex/biber, correct compile sequences, and common bibliography failures
- `assets/skeletons/`: minimal starting points for a letter, report, CV, and Beamer presentation