# latex-essentials

A general-purpose Agent Skill for writing, compiling, and troubleshooting LaTeX documents: reports, letters, articles, resumes/CVs, invoices, quotations, proposals, academic papers, books, and Beamer presentations.

## What's in here

```
latex-essentials/
├── SKILL.md                     # the skill definition
├── references/
│   ├── troubleshooting.md       # layout/rendering issues (margins, tables, page breaks, fonts)
│   ├── error-catalog.md         # compile-time errors, matched to actual LaTeX output
│   └── citations.md             # natbib/BibTeX vs biblatex/biber, compile sequences
└── assets/
    └── skeletons/                # minimal starting points
        ├── letter.tex
        ├── report.tex
        ├── cv.tex
        └── beamer.tex
```

## What it does

Rather than shipping a large fixed template library, this skill teaches the workflow: pick the right document class and packages for the job, write clean LaTeX, compile with the correct engine, and verify the rendered output rather than trusting a clean compile log. The reference files cover the specific, recurring problems that come up doing this in practice (header/footer margin conflicts, booktabs table styling, box page-break behavior, font/engine mismatches, bibliography compile sequences), and the skeletons give a working starting point for four common document types.

## Installation

Clone the repo, then copy `latex-essentials/` into your agent's skills directory.

```bash
git clone https://github.com/adnan425/latex-essentials.git
```

**Claude Code** (personal, all projects):
```bash
mkdir -p ~/.claude/skills
cp -r latex-essentials ~/.claude/skills/latex-essentials
```

**Claude Code** (this project only):
```bash
mkdir -p .claude/skills
cp -r latex-essentials .claude/skills/latex-essentials
```

**Cursor**:
```bash
mkdir -p .cursor/skills
cp -r latex-essentials .cursor/skills/latex-essentials
```

**Gemini CLI** (personal):
```bash
mkdir -p ~/.gemini/skills
cp -r latex-essentials ~/.gemini/skills/latex-essentials
```

**Gemini CLI** (this project only):
```bash
mkdir -p .gemini/skills
cp -r latex-essentials .gemini/skills/latex-essentials
```

**Codex CLI**:
```bash
mkdir -p .codex/skills
cp -r latex-essentials .codex/skills/latex-essentials
```

Or, without cloning, just fetch `SKILL.md` directly:

```bash
mkdir -p ~/.claude/skills/latex-essentials
curl -fsSL https://raw.githubusercontent.com/adnan425/latex-essentials/main/SKILL.md \
  -o ~/.claude/skills/latex-essentials/SKILL.md
```

(swap the target directory for whichever agent you're using above). Note this grabs `SKILL.md` only, not the `references/` and `assets/` folders it points to. Clone the full repo if you want the error catalog, citation guide, and skeleton starting points available too.

After installing, start a new session in your agent. The skill activates automatically on LaTeX-related requests, based on its `description` in the frontmatter.

## Using it without installing

Copy the contents of `SKILL.md` and paste it into your conversation wrapped in `<skill>...</skill>` tags, followed by your request. Works in any chat-based agent without setup, for that one conversation.

## Using it on claude.ai (no terminal)

1. Download `SKILL.md` from this repo (or the whole repo as a ZIP via the green **Code** button).
2. At [claude.ai](https://claude.ai), click **+** in the chat box, then go to **Skills**, **Manage Skills**, **+**, **Create Skill**, **Upload Skill**.
3. Upload `SKILL.md` (or the ZIP) and save.
4. Start a new conversation and describe a LaTeX task; the skill activates automatically.

## Compatibility

Uses the standard `SKILL.md` + YAML frontmatter format, so it works with any agent that reads that format: Claude Code, Claude.ai (as an uploaded/pasted skill), Cursor, Gemini CLI, Codex CLI, and others following the same convention. Directory paths differ per agent; see above.

## License

MIT