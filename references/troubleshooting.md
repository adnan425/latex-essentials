# Troubleshooting

Recurring LaTeX problems, roughly in the order you're likely to hit them.

## 1. Header/footer overlapping body text

**Symptom:** Footer text collides with the last line of body content, or the header rule overlaps the top line of the page.

**Cause:** `fancyhdr`'s `\headheight`/`\footskip` don't match the margins set by `geometry`. The reserved header/footer space is smaller than what's actually being drawn into it.

**Fix:** Set geometry explicitly with enough headroom, and size `\headheight` to fit the tallest header content (usually a logo or two-line header):

```latex
\usepackage[a4paper, top=2.5cm, bottom=2.5cm, left=2cm, right=2cm, headheight=1.6cm, footskip=1.2cm]{geometry}
```

If LaTeX emits `Package fancyhdr Warning: \headheight is too small`, don't ignore it. Fix the value; it's telling you exactly what's wrong.

## 2. Tables look cramped or read as a raw data dump

**Symptom:** Default `tabular` output with `|` vertical rules and no row spacing looks unfinished.

**Fix:** Use `booktabs` (`\toprule`, `\midrule`, `\bottomrule`) with no vertical rules, and add row padding:

```latex
\usepackage{booktabs}
\renewcommand{\arraystretch}{1.3}
```

This one change does more for "finished document" vs. "spreadsheet export" than anything else in a table.

## 3. A box/callout environment splits awkwardly across a page break

**Symptom:** A `tcolorbox` (or similar) gets cut mid-content at a page boundary, or throws an overflow error instead of breaking.

**Fix:** Always pass `breakable` to any box environment that could contain variable-length content:

```latex
\begin{tcolorbox}[breakable, colback=accent!5, colframe=accent]
...
\end{tcolorbox}
```

Without `breakable`, `tcolorbox` does not split across pages by default.

## 4. Fonts not rendering / "Font X not found"

**Symptom:** Compilation fails, or silently falls back to Computer Modern despite a `\setmainfont` call.

**Cause:** `fontspec` (needed for system/OpenType fonts like TeX Gyre families, custom brand fonts, etc.) requires **XeLaTeX or LuaLaTeX**. It does not work under plain `pdflatex`.

**Fix:** Compile with the matching engine:

```bash
latexmk -xelatex -interaction=nonstopmode -halt-on-error document.tex
```

If the font itself isn't installed on the system, install the relevant package (e.g. `texlive-fonts-extra` on Debian/Ubuntu for the TeX Gyre family) or substitute an available font and tell the user rather than silently changing the visual identity.

## 5. Cascading errors after one real mistake

**Symptom:** A single missing brace or unclosed environment produces dozens of unrelated-looking errors further down the log.

**Fix:** Always compile with `-halt-on-error` while iterating, and fix from the *first* error in the log, not the last. LaTeX's error recovery is poor; one real problem reliably produces a wall of noise after it.

## 6. General verification step (don't skip this)

After a successful compile, rasterize the output and actually look at it:

```bash
pdftoppm document.pdf preview -png -r 100
```

A clean compile log does not guarantee correct layout. Margin, overflow, and spacing issues frequently compile without error and only show up visually.