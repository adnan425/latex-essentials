# Compile Error Catalog

Keyed by the error message LaTeX actually prints, so you can match against the log directly rather than guessing. Always fix the *first* error in the log; everything after it is often noise caused by the first one (see `troubleshooting.md` #5).

## `! Undefined control sequence.`

**Meaning:** A command was used that LaTeX doesn't recognize, in this document as currently loaded.

**Common causes:**
- Typo in a command name (`\textb{}` instead of `\textbf{}`)
- The command belongs to a package that isn't loaded (e.g. using `\toprule` without `\usepackage{booktabs}`)
- A command from a different engine/format leaked in (e.g. plain TeX primitives that don't exist in LaTeX, or a Markdown-style construct that isn't LaTeX at all)

**Fix:** Check the log for which line triggered it, check spelling, check that the owning package is loaded.

## `! Missing $ inserted.`

**Meaning:** A math-mode-only character or command was used outside math mode.

**Common causes:** A bare `_`, `^`, or a command like `\alpha` used in running text without wrapping it in `$...$`.

**Fix:** Wrap the offending content in `$...$` (inline) or `\[...\]` (display), or escape the character if it wasn't meant to be math (`\_` for a literal underscore).

## `! Extra alignment tab has been changed to \cr.`

**Meaning:** A `tabular`/`array` row has more `&`-separated columns than the column specification declares.

**Fix:** Count the columns in `\begin{tabular}{...}` against the actual number of `&` in each row. A common cause is a `\\` missing at the end of the previous row, which merges two rows' cells together.

## `! LaTeX Error: \begin{X} on input line N ended by \end{Y}.`

**Meaning:** Mismatched environment nesting; an `\end{}` doesn't match the most recent `\begin{}`.

**Fix:** Check for a missing `\end{}` somewhere above the reported line, not necessarily right above it; environments must close in strict LIFO order.

## `! LaTeX Error: File 'X.sty' not found.`

**Meaning:** A required package isn't installed in the current TeX distribution.

**Fix:** Install it via the distribution's package manager (`tlmgr install <package>` for TeX Live) or, in a constrained environment, check if a bundled alternative exists before assuming the exact package must be installed.

## `! LaTeX Error: Environment X undefined.`

**Meaning:** An environment was used (`\begin{X}...\end{X}`) that no loaded package defines.

**Fix:** Almost always a missing `\usepackage{}` for the package that defines it (e.g. `tcolorbox` for `tcolorbox` environments, `algorithm`/`algorithmic` for pseudocode environments). Confirm the package name matches the environment, not just a similar-sounding one.

## `Overfull \hbox` / `Underfull \hbox` warnings

**Meaning:** Not fatal, but a real layout signal: text is running past the margin (overfull) or has excessive stretched spacing (underfull).

**Fix:** Usually caused by a long unbreakable string (URL, wide table cell, long unhyphenated word). For URLs, use `\usepackage{url}` or `\usepackage{hyperref}` and wrap with `\url{}`. For tables, check if a column needs a fixed width with line-wrapping (`p{...}` column type) instead of natural width. Don't ignore these near final output; they're often visible as text running into the margin.

## `Package fancyhdr Warning: \headheight is too small`

See `troubleshooting.md` #1. This is a layout warning, not a hard error, but it corresponds to a real visible defect (header/footer overlap) and should be treated as one.

## `! Package biblatex Error: ...` or empty bibliography

See `citations.md` for the full citation/bibliography compile sequence and common causes.

## General debugging approach

1. Find the **first** error in the `.log` (or terminal output), not the last.
2. Note the line number it reports, but check a few lines above too. LaTeX often reports the error slightly after the actual mistake (the point where it could no longer make sense of the input).
3. Fix one error, recompile, repeat. Don't try to fix everything the log shows in one pass; later errors are frequently side effects of the first.
4. If the error is genuinely unclear, isolate: comment out sections with `%` (or wrap in `\iffalse ... \fi`) until the document compiles, then reintroduce sections one at a time to find the actual trigger.