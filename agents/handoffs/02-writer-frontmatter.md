# Handoff — WriterAgent: Front Matter

**Date:** 2026-05-26
**Agent:** WriterAgent
**Slice:** Front matter (Preface, Articulation Matrix, How to Use, Prerequisite Self-Test)
**Inputs read:** `CONVENTIONS.md`, `docs/superpowers/specs/2026-05-26-frontmatter-toc-design.md`

## Files written

| File | Approx. words | Target | Status |
|------|---------------|--------|--------|
| `book/00-front-matter/00-preface.md` | ~1,580 | 1,200–1,800 | In range |
| `book/00-front-matter/01-articulation-matrix.md` | ~930 | ~2 pages | In range |
| `book/00-front-matter/02-how-to-use.md` | ~1,590 | ~1,500 | In range |
| `book/00-front-matter/03-prerequisite-self-test.md` | ~2,120 (20 Q + full solutions + routing table) | — | Complete |

## Notes on content decisions

- **Preface** is in Prof. Gao's first-person voice. Grounded only in verified CV facts (GMU Associate Prof.; SEC DERA; Congressional fair-lending testimony; HUD/Fed talks). Cited exactly three of his papers per spec §5: Gao & Sun (2019, PNAS), Gao, Han, Li & Zhou (2018, JFE), Gao, Han, Kim & Pan (2024, JCF). PhD/MS/BS background was available in the prompt but I chose not to lean on it heavily; only the professional standards angle is foregrounded. Ends noting the camp complements NextGen.
- **Articulation Matrix** maps Weeks 1–8 to the real NextGen structure (12 wks, Jun 26–Sep 11 2026; Phase 1 = 7 Fri sessions 12–2pm EST; Phase 2 presentation ~Aug 15; Phase 3 = 4 wks refinement; CRSP/Compustat/EDGAR/Bloomberg; Claude/ChatGPT; Young Scholars Journal + MARS; awards to $2,000; Dr. Lei Gao directs). Explicit leads/deepens column throughout, plus framing + handshake paragraphs.
- **How to Use** covers reveal-the-trick model, daily rhythm, interlocking parts, prereq-test → Appendix A/B routing, reproducibility + honest-writing standards, the 8-week ladder. Introduces all five cast members (Maya, Devon, Priya, Sam, Leah); uses Maya/Devon in the routing narrative.
- **Prerequisite Self-Test:** 7 calculus, 10 prob/stats (Q8–Q17), 3 Python/data (Q18–Q20) — total 20. Calc count is 7 (Q1–Q7) as specified; prob/stats runs 10 rather than 8 to cover all listed sub-topics (expectation/variance, scaling, Bayes, total expectation, binomial, normal, t-test, corr-vs-cause, CI, sampling dist), Python is 3 (output-reading, vectorized-vs-loop, groupby). All have full worked solutions + a cluster-based routing table referencing Appendix A (Math Toolkit) and Appendix B (Python & LaTeX Setup), confirmed to be the actual appendix dir names.

## [CHECK] items

- **[CHECK]** Preface biographical framing: confirm Prof. Gao is comfortable with the SEC/Congress/HUD/Fed sentence as phrased and with the "2,200+ citations" fact being *omitted* (I left it out of the prose to keep the voice un-self-promotional; re-add if desired).
- **[CHECK]** Articulation Matrix assigns specific Gao papers to specific reading weeks (Intraday Momentum → Week 5; Common Ownership → Week 6; Gao & Sun fair lending → Week 4). These week assignments are my editorial placement, not specified in the spec — confirm against the eventual Weeks 5–6 reading-guide plan so they stay consistent.
- **[CHECK]** Appendix B is titled "Python & LaTeX Setup" per the dir name `B-python-latex-setup`; the How-to-Use and routing table point Python remediation there. Confirm that is where pandas/numpy fundamentals will live (vs. Appendix A).
- No fabricated facts or citations introduced. POM manuscript (spec §9) not referenced anywhere in front matter.

## Suggested next

ReviewerAgent: verify voice/notation/citation compliance against CONVENTIONS.md and confirm the [CHECK] items with Prof. Gao.
