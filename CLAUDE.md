# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-page LaTeX CV/resume. `main.tex` is the only source file; `CV_Volkov.pdf` is the committed compiled output. The template is derived from Jake Gutierrez's resume (sb2nov/resume), MIT licensed.

## Building

The document uses `fontspec`, so it **must be compiled with XeLaTeX or LuaLaTeX** — pdfLaTeX will fail.

```sh
xelatex -jobname=CV_Volkov main.tex
# or
latexmk -xelatex -jobname=CV_Volkov main.tex
```

`-jobname=CV_Volkov` makes the output match the committed PDF name (otherwise it is `main.pdf`). MiKTeX is installed at `C:\Users\Dima\AppData\Local\Programs\MiKTeX\miktex\bin\x64\` but is not on the Bash tool's PATH, so call `xelatex`, `pdftotext` and `pdftoppm` by full path. Run `xelatex` twice when hyperref asks for a rerun. `.gitignore` covers LaTeX build artifacts and `main.pdf`; only `CV_Volkov.pdf` is tracked.

Durations in `\resumeSubheading` (e.g. "2 yrs 7 mos") are hard-coded and counted inclusively, LinkedIn-style (May 2023 – Nov 2025 = 31 months). Update the "present" role's duration whenever the CV is touched.

## Fonts

- Name/title font: `Fonts/basictitlefont.ttf`, loaded by path through `\newfontfamily{\basictitle}`; compile from the repo root so `Path = Fonts/` resolves.
- Body font: `\setmainfont{XCharter}` is looked up **by name** (system install or the TeX distribution's XCharter package). It does *not* use the `Fonts/Charter-*.otf` files in the repo.
- Icons come from the `fontawesome5` package (`\faEnvelope`, `\faLinkedin`, `\faGithub`, `\faTelegram`).

## Structure of main.tex

- The preamble defines the custom layout macros; the content uses them. Use these rather than raw `itemize`/`tabular`:
  - `\resumeSubheading{org}{duration}{title}{dates}` for a company or school entry.
  - `\resumeSubSubheading{title}{dates}` for an extra role at the same company (promotions).
  - `\resumeItem{...}` for a bullet, wrapped in `\resumeItemListStart` / `\resumeItemListEnd`.
  - Each employer block is wrapped in `\resumeSubHeadingListStart` / `\resumeSubHeadingListEnd`.
- Contact links use `\myuline{}`, a custom underline built from `contour` + `ulem`.
- Optional features are switched with `\iffalse` / `\iftrue` blocks: the extra margin adjustments and the header photo. The photo expects `Images/main_photo.jpg`, and the `Images/` directory does not exist yet.
- Escape LaTeX special characters in content (`\&`, `\_`, `\%`). The existing text already does this, e.g. `P\&G` and `Volkov\_Di`.

## Hard rule: strictly one A4 page

The CV must always compile to exactly one page. After **any** content or layout change, rebuild and confirm the log says `Output written on CV_Volkov.pdf (1 page).` Never finish a change that leaves it at two pages.

If it overflows, run `pdftotext -f 2 -l 2 CV_Volkov.pdf -` to see what spilled over, then reclaim space in this order:
1. Tighten wording: merge or shorten bullets, and cut bullets that wrap onto a second line by only a word or two.
2. Drop the least relevant bullets (oldest or least related roles first).
3. Adjust layout: `geometry` margins (currently 0.6in top/bottom, 0.75in sides) or the `\vspace` values in the custom macros. Don't go below ~0.5in margins or 11pt body text, because that makes the page look cramped.

Ask the user before deleting content they wrote; wording and spacing changes are fine to make directly.
