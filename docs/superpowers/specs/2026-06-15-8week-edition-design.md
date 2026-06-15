# Design — NextGen Empirical Finance Camp, 8-Week Edition (`8weeks2`)

**Date:** 2026-06-15
**Author:** Lei Gao (via Claude Code)
**Status:** Approved (decisions captured interactively)

## Goal

Produce a standalone, genuinely **8-week** edition of the NextGen Empirical
Finance Research Camp, under the same branding as the existing 12-week book
(`8weeks/`), and publish it to a new GitHub repo with GitHub Pages.

## Source of truth

`8weeks/` — the full 12-week Quarto book (`edwardlg/nextgen-empirical-finance-camp`).
Weeks 1–8 are the instructional core; Weeks 9–12 are a fully-written
(~250k words) symposium + paper-refinement arc.

## Decisions

1. **Identity:** Standalone 8-week NextGen edition, same branding, retitled
   "8-Week Edition." Weeks 1–8 are the scheduled spine.
2. **Weeks 9–12:** NOT kept as a separate section, and NOT deleted wholesale
   (they are not empty — they hold real content). Instead, **compress each of
   the four themes to its essentials** and fold those into Weeks 3–8; remove the
   standalone Week 9–12 folders, their notebooks, and their PS solutions.
3. **Publish:** New repo `edwardlg/nextgen-empirical-finance-camp-8wk` + Pages.

## Theme → home mapping (compressed chapters)

| Source week | Theme | New consolidated chapter |
|---|---|---|
| Week 10 | Robustness v2 (multiple testing, heterogeneity/CATE, mechanisms, external validity) | `week-04/ch46-robustness-v2.md` |
| Week 11 | Manuscript build (abstract, tables, figures, intro, lit review) | `week-07/ch76-manuscript-build.md` |
| Week 9 | The conference (talk, poster, Q&A, feedback) | `week-08/ch86-the-talk-the-poster-the-defense.md` |
| Week 12 | Submission & the long arc (Young Scholars Journal/MARS, SSRN, circuit) | `week-08/ch87-submission-and-the-long-arc.md` |

Each consolidated chapter distills five source chapters into ~2,500–3,500 words,
preserves (never invents) citations, follows `CONVENTIONS.md`, and cross-links
to the relevant appendices.

## Structural changes

- `_quarto.yml`: retitle to 8-Week Edition; drop Week 9–12 parts; add the four
  new chapters to their weeks; drop E-w9..12 solutions; repoint repo/site URLs
  to the new repo.
- Delete `_quarto-8wk.yml` (default profile is now the 8-week book).
- Delete `book/weeks/week-09..12/`, `notebooks/week-09..12/`,
  `book/appendices/E-solutions-manual/E-w{9,10,11,12}-*.md`.
- Branding sweep: `README.md`, `index.qmd`, `SITE.md`, `book/TOC.md`,
  front-matter, instructor pacing guide, week-04/07/08 READMEs,
  `_includes/notebook-buttons.html` (repo/branch), CI workflow.

## Verification

Local `quarto render --to html` (notebooks not executed) must succeed before
push; CI render on the new repo must go green; Pages must serve the site.
