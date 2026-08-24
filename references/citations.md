# Citations and Bibliographies

Two systems handle this in LaTeX. Pick one, don't mix them.

## natbib + BibTeX (the older, still very common combination)

```latex
\usepackage{natbib}
...
\bibliographystyle{plainnat}   % or apalike, unsrtnat, etc.
\bibliography{references}      % looks for references.bib
```

Cite with `\citep{key}` (parenthetical) or `\citet{key}` (textual). Compile sequence matters:

```bash
pdflatex document.tex
bibtex document
pdflatex document.tex
pdflatex document.tex
```

Two `pdflatex` passes after `bibtex` are not optional. The first resolves the citation keys into the `.bbl` file; the second resolves the resulting cross-reference numbers. Skipping either pass leaves `[?]` or stale numbers in the output.

## biblatex + biber (the modern default, especially for anything beyond natural sciences)

```latex
\usepackage[backend=biber, style=authoryear]{biblatex}
\addbibresource{references.bib}
...
\printbibliography  % near the end of the document
```

Cite with `\cite{key}`, `\parencite{key}`, or `\textcite{key}` depending on the style. Compile sequence:

```bash
pdflatex document.tex
biber document
pdflatex document.tex
pdflatex document.tex
```

`biber`, not `bibtex`. Using the wrong backend against biblatex is the single most common cause of "my bibliography won't show up" reports. Check which one the `backend=` option specifies before troubleshooting further.

## Common problems

**"Citation undefined" warnings after a full compile sequence**
The key in `\cite{key}` doesn't match any entry in the `.bib` file, or the `.bib` file isn't on the path given to `\bibliography{}`/`\addbibresource{}`. Check the exact key spelling (case-sensitive) and the file path.

**Bibliography doesn't appear at all**
Either `\bibliography{}`/`\printbibliography` is missing from the document body, or the compile sequence skipped the `bibtex`/`biber` step, or a `.bib` syntax error (unbalanced braces, missing comma between fields) silently dropped entries. Run `bibtex document` or `biber document` directly and read its own log output, not just the LaTeX log.

**Using `latexmk`, none of this needs to be tracked manually**
`latexmk -pdf` (or `-xelatex`) automatically detects whether a bibliography step is needed and runs the correct number of passes with the correct tool. For any document with citations, prefer `latexmk` over manually chaining `pdflatex`/`bibtex`/`biber` calls; manual chains are where pass-count mistakes happen.

## Choosing between them for a new document

- Journal or conference has a required `.cls`/style file that already assumes one system: use what it assumes, don't fight it.
- No constraint, humanities/multilingual/complex sorting needs: `biblatex` + `biber`.
- No constraint, simple reference list, maximum compatibility with older templates: `natbib` + `bibtex` is still fine and slightly lighter-weight.