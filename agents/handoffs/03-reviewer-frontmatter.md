# Handoff — ReviewerAgent: Front Matter + TOC verdict

**Date:** 2026-05-26
**Agent:** ReviewerAgent (empirical-finance referee + pedagogy editor)
**Slice reviewed:** Front matter (Preface, Articulation Matrix, How to Use, Prerequisite Self-Test) + `book/TOC.md`
**Inputs read:** `CONVENTIONS.md`, spec `2026-05-26-frontmatter-toc-design.md`, `book/TOC.md`, all four front-matter files, handoffs 01 & 02.

---

`[ReviewerAgent -> verdict]`

## (a) Concrete improvements APPLIED

1. **`03-prerequisite-self-test.md`, Q10 (Bayes) solution note — statistical correctness fix.**
   The original note claimed the prompt's "flags 5% of all applications" figure "is implied by the other
   numbers and not needed." That is false: the stated conditionals imply an overall flag rate of
   $P(+) = 0.90\cdot0.02 + 0.04\cdot0.98 = 0.0572 \approx 5.7\%$, which is *not* 5%. The figure is an
   approximate/inconsistent marginal, not an implied one. Rewrote the note to label it a deliberate
   distractor, show the true 5.7% marginal, and explain it is simply unused. The numeric answer
   (≈0.31) was already correct and is unchanged. **Why:** an answer key that mis-explains an internal
   inconsistency teaches a wrong inference; this is exactly the "highest priority" front-matter error class.

2. **`01-articulation-matrix.md`, Week 5 & Week 6 rows — internal consistency with the canonical TOC.**
   The Matrix had assigned *Market Intraday Momentum* (Gao et al. 2018) to Week 5; the TOC's Week 5
   reading guides are the asset-pricing/panel classics (FF92, FF93, JT93, Petersen 2009, BDM 2004) with
   the Week 5 mentor session anchored to *The Rainbow of Credits* (Gao, Liu & Wang, AEA 2025), and Week 6
   anchored to supply-chain common ownership (Gao, Han, Kim & Pan 2024) plus fair-lending (Gao & Sun 2019,
   Ch 6.4). Per the instruction, I adopted the **TOC as canonical** and rewrote both Matrix rows to match.
   **Why:** the TOC is the structural source of truth that every later week inherits; the front-matter map
   must not contradict it. The TOC plan is also pedagogically better — it reserves the named Gao papers for
   the weeks whose mentor sessions actually use them.

3. **`01-articulation-matrix.md`, Week 1 & Week 2 rows — corrected a topic-placement error.**
   The Matrix described Week 1 as "the mechanics of OLS" and Week 2 as pure "inference," but the TOC has
   Week 1 = *Probability, Sampling, and the Logic of Inference* and Week 2 = *The OLS Engine* (matrix OLS,
   Gauss–Markov, FWL, SE flavors, misspecification). Rewrote Week 1 to probability/sampling/inference (and
   added the *Intraday Momentum* mentor anchor that the TOC places at Week 1, completing the Gao-paper→week
   map) and Week 2 to the OLS-engine content. **Why:** OLS appearing a week early in the front-matter map
   would mis-signal the curriculum to students and instructors on page two.

## (b) Remaining [CHECK] items for the human (Prof. Gao) to resolve

- **[CHECK] Preface biography phrasing** (carried from WriterAgent): confirm comfort with the SEC /
  Congressional fair-lending testimony / HUD / Federal Reserve sentence as worded, and with the
  "2,200+ citations" fact being intentionally omitted. All facts used are in CONVENTIONS.md §8; nothing
  fabricated.
- **[CHECK] Non-Gao classic-paper page ranges** (carried from TOC/PlannerAgent): FF92, FF93, JT93,
  Petersen 2009, BDM 2004 (Week 5) and KPSS 2017, Hoberg–Phillips 2016, Loughran–McDonald 2011,
  Bartlett et al. 2022 (Week 6) — years confident, exact pages to confirm before publication.
  **Bhutta–Hizmo–Ringo** venue/year (Fed FEDS working paper vs. journal) still unverified. These live in
  the TOC, not the front matter, and remain correctly tagged [CHECK].
- **[CHECK] POM ms POM-Nov-25-OA-1924** — correctly excluded everywhere (not in CV selected list); leave
  out until bib confirmed.
- **Minor (no fix applied):** FM-2 Articulation Matrix actual length (~990 w after edits) runs under its
  TOC budget of 1,600 w. Acceptable for this slice; flag for possible expansion when the matrix is
  finalized, but not a correctness issue.

## (c) Pass/fail on the 5 mandatory checks

1. **Statistical correctness of all 20 solutions — PASS (after fix).** Re-derived Q1–Q20: derivatives
   (Q1 f'(1)=9, Q2, Q3 p=12.5, Q4), optimization (Q5 → sample mean), integral (Q6 = 12), limit (Q7 = 3),
   E/Var (Q8 = 3.5, 35/12), scaling (Q9 → 3, 36), Bayes (Q10 ≈ 0.31), LTE (Q11 = \$3.7M), binomial
   (Q12 ≈ 0.21), normal/68-95-99.7 (Q13 ≈ 2.5%), t-test (Q14 t=2.8, reject), confounding (Q15),
   CI reading (Q16), SE of mean (Q17 = 2, halves), and the three Python snippets (Q18 = 7, Q19 = B,
   Q20 = mean ret by sector) all verified correct. The only defect was Q10's explanatory note, now fixed.
2. **Citation integrity — PASS.** All Gao citations in the Preface match spec §5 verbatim (Gao & Sun 2019
   PNAS; Gao, Han, Li & Zhou 2018 JFE; Gao, Han, Kim & Pan 2024 JCF). Matrix additions (Rainbow of Credits,
   common ownership, Intraday Momentum) match §5 and are framed as working/anchor papers, not over-claimed.
   No fabricated facts in the Preface. Non-Gao papers carry [CHECK] in the TOC, not presented as certain.
3. **Internal consistency — PASS (after fixes).** Matrix↔TOC Gao-paper/week mapping reconciled to the
   canonical TOC; Week 1/2 topic placement corrected. Routing table targets ("Appendix A: Math Toolkit,"
   "Appendix B: Python & LaTeX Setup") match the TOC's actual appendix names and contents.
4. **Word-count sanity — PASS.** TOC states a grand total (~690,800 maximal) and an authoritative published
   target of **~400,000**, squarely inside the 300k–500k band; framing is explicit that slices write to the
   target column. Clear enough; no change needed.
5. **Voice & audience — PASS.** Reveal-the-trick structure is stated and honored; no emojis or marketing
   voice; terms defined on first use; concrete-before-abstract observed; full student cast (Maya, Devon,
   Priya, Sam, Leah) used consistently with CONVENTIONS.md interests across the front matter.

**Overall: PASS** — front-matter slice is publication-ready for this build stage, pending the human-only
[CHECK] confirmations above (biography phrasing and non-Gao page ranges).
