# [ReviewerAgent -> Week 1 verdict]

*Reviewed: 2026-05-27. Scope: all of Week 1 — ch11–ch15, ps1.1–ps1.5 + solutions,
lab1, mentor1, assessment1, nb1.1–nb1.5, and the w1-* handoffs. Arithmetic spot-checked
in `python3` (numpy/scipy); nb1.3 and nb1.5 code cells extracted and executed headless.*

## (a) Fixes APPLIED (>=3)

1. **Ch 1.5 §1.5.4 — power/sample-size inconsistency (math error in chapter prose).**
   File: `book/weeks/week-01/ch15-hypothesis-testing.md`. The paragraph is entirely a
   *one-sided* example (critical value 1.65, power = Pr(Z>0.59) ≈ 0.28), but it then
   claimed "to reach 80% power ... N ≈ 1,750 days — about seven years." That 1,750/7-year
   figure is the **two-sided** answer (noncentrality 1.96+0.84=2.80 → N≈1,764). The
   consistent *one-sided* answer is N≈1,390 (≈5.5 yr). **Fix:** rewrote the sentence to
   show the one-sided derivation (z_0.05+z_0.20 = 1.645+0.84 = 2.485 → N≈1,390) and added
   a parenthetical giving the two-sided 1,760/7-year figure explicitly, with a forward
   pointer to PS1.5 P3. This both corrects the arithmetic and pre-empts the one- vs.
   two-sided confusion flagged in check 2(b).

2. **nb1.5 "Your Turn" — same one-sided/two-sided N bug, in the notebook.**
   File: `notebooks/week-01/nb1.5-power-and-the-t-test.ipynb` (markdown cell 22, exercise 1).
   The hint specifies the *one-sided* formula `power = Φ(μ√N/σ − z_α)` (and Section 4 of
   the same notebook uses one-sided 1.65), yet told students "you should land near
   N ≈ 1,750 days — about seven years." With that one-sided formula the answer is N≈1,390
   (≈5.5 yr); N=1,750 gives ~87% power. **Fix:** changed to "N ≈ 1,390 days — about five
   and a half years (one-sided z_α=1.645, matching Section 4); two-sided z_{α/2}=1.96
   raises it to N ≈ 1,760 days, about seven years." nbformat re-validated; code cells
   re-run clean.

3. **PS1.2 P3 — undocumented new numbers.** File: `book/weeks/week-01/ps1.2.md`. Maya's
   two-asset table here (E[R]=0.09/0.03, Var=0.04/0.0025) differs from Ch 1.2's worked
   example (0.08/0.04, sd 0.20/0.10) but was not flagged, risking a student conflating the
   two. **Fix:** added an inline note that these are a *new* pair of assets with different
   numbers, "so re-derive rather than quoting Ch 1.2's results." (Per CONVENTIONS / brief
   check 2: where a PS deliberately uses new numbers, say so. PS1.1 P4 already does this
   for Sam's 3-value table; this brings PS1.2 into line.)

## (b) Remaining [CHECK] / human-decision items

- **assessment1 B3 (carried forward, not a defect).** The expected *direction* of the
  one-sample t-test size distortion on the lognormal is intentionally left open; the key
  accepts any internally consistent reading. If stricter grading is wanted, the instructor
  should run the sim once and pin the observed direction. (Recommendation: leave open —
  the rubric already rewards calibrated honesty and the open-ended framing is sound
  pedagogy.)
- **lab1 Step 4 framing.** Empirical size of the from-scratch z-test on discrete coin data
  comes in slightly above 0.05 (~0.054); the prose now states this honestly as the normal
  approximation being mildly liberal, not a bug. Confirmed this reads as intended — no
  change needed.
- No other [CHECK] items remain open; no fabricated citations anywhere to chase.

## (c) PASS/FAIL on the five checks

1. **Answer-key & solution correctness — PASS.** Every solutions file and the assessment
   key was spot-checked in python3; all load-bearing numerics reproduce exactly (PS1.1
   P2/P4 LTV 6.3684 & 37% across-share; PS1.2 P1/P3/P5/P6; PS1.3 P2/P6; PS1.4 P1 Chebyshev
   N≥4500, P2c 0.159, P3b N=348, P6 SE 0.002 / 0.421; PS1.5 t=1.69, p=0.046/0.092,
   CI [-0.013%,0.173%], 0.64, Bonferroni 0.0025, k=14; assessment A1 4.8, A3 0.004,
   A4 |μ|<3699, B4 MC SE 0.00154). The only math defects found were in *chapter/notebook
   prose* (fixes 1 & 2), not in any answer key.
2. **Cross-file consistency — PASS.** (a) Trimmed mean: Ch 1.3 §4 frames "Recipe B wins"
   as a hypothetical explicitly to be tested ("an empirical question — exactly what you'll
   measure in nb1.3"); nb1.3 (re-run: plain-mean MSE 390,978 vs trimmed 1,576,606) and
   PS1.3 P5(d) both honestly report the trimmed mean LOSES (bias ~−$1,160 dominates). No
   contradiction. (b) Power figure: Ch 1.5 ≈28% is one-sided, PS1.5 P3 ≈18% is two-sided;
   both verified correct and now both explicitly attributed (PS1.5 already had the note;
   Ch 1.5 and nb1.5 now do too after fixes 1–2). (c) Sam's regime numbers, Maya's two-asset
   figures, t=1.69/CI example all agree across chapter↔nb↔PS; new-number cases are flagged.
3. **Notation & voice — PASS.** Bold vectors/matrices (𝐰, 𝐗, 𝚺, 𝛃), hats, plim/→p/→d per
   CONVENTIONS; student cast (Maya/Devon/Priya/Sam/Leah) used consistently with stable
   interests; no emojis; no marketing voice; reveal-the-trick structure followed.
4. **p-value / CI / causation interpretation — PASS.** No "p = P(null true)", no "accept
   the null", no "95% probability μ in this interval" used as truth. Every appearance of
   those phrases is a deliberately-quoted error to spot/avoid (assessment A7, the rubric,
   lab1 reflections), correctly handled.
5. **No fabricated citations — PASS.** The sole external citation in Week 1 is the mentor
   session's Gao, Han, Li & Zhou (2018), "Market Intraday Momentum," *Journal of Financial
   Economics*, 129, 394–414 — matches the brief exactly. (Gosset/Student, Cauchy–Schwarz,
   Chebyshev, Bessel referenced as common knowledge without bibliographic claims.)

## (d) Thin-chapter note vs. TOC word targets

Per the writer handoffs: ch12 ≈4,097 words, ch13 ≈4,336, ch14 ≈4,771 — all at/above the
planner's ~4,000-word floor though below their nominal ceilings (~4,800/~5,200/~5,400).
On reading, none is substantively thin: ch12 covers linearity, covariance algebra, the full
Maya diversification worked example, correlation-as-cosine with the Cauchy–Schwarz proof,
and the Σ quadratic form; ch13 covers estimator/estimate, bias, variance, MSE decomposition,
consistency, and the simulated sampling distribution; ch14 gives WLLN+Chebyshev, CLT with the
exponential demo, and a thorough fat-tail/Student-t failure-mode section. They are dense, not
padded. **Recommendation: accept as-is.** If later expansion is desired for uniformity, the
highest-value additions would be (ch13) a second worked estimator beyond the trimmed mean and
(ch14) a short numeric Chebyshev-vs-actual table — but neither is needed for correctness or
pedagogy now.

**Overall: Week 1 is in strong shape — ACCEPT after the three applied fixes.**
