# [ReviewerAgent -> Week 3 verdict]

Full referee + answer-key pass over all of Week 3 (ch31–35, ps3.1–3.5 + solutions,
lab3, mentor3, assessment3, nb3.1–3.5, w3-* handoffs). Notation, identification honesty,
and every load-bearing number checked; IV/weak-IV/AIPW/IPW/EB math re-derived and
re-run in Python (numpy/scipy/statsmodels/linearmodels). Verdict: **strong PASS** with
citation polish applied. Details below.

---

## (a) Fixes APPLIED (>= 3)

1. **ch34-instrumental-variables.md — added full footnote citations.** Inserted
   bibliographic footnotes for Imbens & Angrist (1994, *Econometrica* 62(2):467–475),
   Stock & Yogo (2005, Cambridge, full editor/title/pp. 80–108), Montiel Olea & Pflueger
   (2013, *JBES* 31(3):358–369), and Deng, Gao & Kim (2020, *J Corp Fin* 60, 101498).
   *Why:* CONVENTIONS §6 requires full bibliographic info; ch34 previously cited these
   by name/year only (the handoff [CHECK] items), creating an inconsistency with ch33,
   which already used full footnotes. All four are on the VERIFIED reference list, so the
   [CHECK]s are resolved, not carried.

2. **ch35-reading-iv-weak-iv-pathology.md — added full footnote citations.** Inserted
   footnotes for Anderson & Rubin (1949, *Ann. Math. Stat.* 20(1):46–63), Bound, Jaeger &
   Baker (1995, *JASA* 90(430):443–450), Sargan (1958, *Econometrica* 26(3):393–415),
   Hansen (1982, *Econometrica* 50(4):1029–1054), and Montiel Olea & Pflueger (2013).
   *Why:* same CONVENTIONS §6 requirement; ch35 cited all by name/year only. All are on
   the VERIFIED list.

3. **ch35 — Angrist & Krueger (1991) given a full cite inside the BJB footnote.** The
   handoff flagged A&K as in-text-only context and asked whether to keep/drop. Kept (it is
   the canonical case BJB critique, essential narrative context) and upgraded to a full
   standard cite (*QJE* 106(4):979–1014) embedded in the BJB footnote rather than a bare
   surname-year — more honest and verifiable. *Note:* A&K 1991 is NOT on the supplied
   verified list; it is a universally-standard reference used as context only, not as an
   empirical-claim source — see remaining items.

All four footnote-bearing files now use one consistent footnote style (matching ch33's
existing `[^label]` pattern). Verified: every inline `[^x]` has exactly one `[^x]:`
definition, no label collisions, well-formed GFM.

---

## (b) Remaining [CHECK] / human items

- **No `[CHECK]` tags remain** anywhere in the Week 3 prose or solutions (grep-confirmed,
  before and after edits).
- **ch34 in-text "Lee, McCrary, Moreira, Porter (and others)"** (the "F may need to exceed
  100" robust-inference critique): deliberately LEFT as an in-text mention with no full
  cite, because the paper (Lee–McCrary–Moreira–Porter 2022 *AER* "Valid t-ratio Inference
  for IV") is NOT on the supplied verified reference list. Per instructions I did not
  fabricate a cite; it is appropriately hedged ("and others", no year/venue asserted).
  HUMAN: add the full cite if desired once verified.
- **Angrist & Krueger (1991)** now carries a full cite (see fix 3) but is not on the
  supplied verified list. Standard/uncontroversial; flagging for the record.
- **mentor3 word count** ~2,490 vs ~2,200 target (carried from handoff) — minor; see (d).

---

## (c) PASS/FAIL on the 5 prioritized checks

1. **Answer-key & solution correctness — PASS.** Every numeric claim re-derived/re-run:
   - PS3.1: observed Y=(45,20,52,25,53,30), τ=(10,24,12,24,8,18), ATE 16 / ATT 10 / ATC 22,
     naive 25 = ATT 10 + selbias 15; P3 0.24 = 0.09 + 0.15, counterfactual 0.61. ✓
   - PS3.2: naive 0.1978, ATT 0.0867, ATE 0.0947, selbias 0.1111; all 9 logit propensities
     (e=0.818/0.500/0.964/0.599; controls 0.769/0.401/0.250/0.641/0.142) exact; matching
     T1→C1 (0.049), T2→C2 (0.099), T3 dropped (0.196>caliper), T4→C4 (0.042); ATT 1/3,
     forced 0.25. ✓
   - PS3.3: HT −11/6, Hájek −1.000, naive −7/6; N_eff 121/45→1.24 after explosion; EB
     w=(3/5,2/5), λ=−ln2/6, EB-ATT −2.0; **all four AIPW double-robustness cases re-run
     exactly**: both-correct 3.0, prop-wrong 3.0, outcome-wrong 3.0, both-wrong 13/3=naive. ✓
   - PS3.4: cov-ratio ΣZ̃D̃=4, ΣZ̃Ỹ=−12, 2SLS −3, OLS −2; Wald −10; F 36/2.56; weak-IV
     1/F bias fractions 0.0278/0.391 → E[2SLS] 2.04/2.59 (39% back to OLS). ✓
   - PS3.5: 1/(F+1) at F=42/3.8/5.2/7.5 = 0.023/0.208/0.161/0.118. ✓ Subpart points sum
     to headers and to 100. ✓
   - Assessment key: A5 Wald −300 (matches ch34/PS3.4), A6 1/(F+1)=0.21 at F=3.8; rubric
     rows 18+16+14 (=48 Part A) + 24+18 (=42 Part B) + 10 = 100. ✓
   - Chapter worked numbers: ch32 two-cell (naive 0.242, ATT 0.1375, selbias 0.104) ✓;
     ch33 Hájek 8.45/9.23/−0.78 ✓; ch31 5-unit ATE 0.4, Maya 0.07+0.14=0.21 ✓.
   - **Independently re-ran the lab/nb3.5 weak-IV Monte Carlo** (seed 20260528, 1200 reps):
     reproduced conv coverage 0.800 (text 0.801), AR 0.973, AR-unbounded 0.911, median
     2SLS +0.259 strictly between OLS 0.400 and truth 0 (~65% pull), mean F 1.40; strong
     case 2SLS −0.001, conv cov 0.948, AR 0.953, mean F 52. Matches the chapter/lab to MC
     noise. The IPW Hájek-vs-HT, the Wald/2SLS cov-ratio, the 1/(F+1) heuristic, and the AR
     coverage claims are all correct. **No math errors found.**

2. **Notation consistency — PASS.** Y_i(1)/Y_i(0), D_i, τ_i, ATE/ATT/LATE, selection bias
   = E[Y(0)|D=1]−E[Y(0)|D=0], e(X) for propensity all used consistently with ch31's locked
   definitions across chapters, PSs, solutions, assessment, lab, and notebooks. ATC introduced
   only in PS3.1 (clearly defined). No drift. Intentional, pedagogically-flagged refinement:
   ch34 presents the bias heuristic as 1/F (simpler), ch35 upgrades to 1/(F+1) ("the engine
   behind F>10"); PS3.4 uses 1/F, PS3.5/assessment use 1/(F+1) — consistent with the chapter
   each belongs to. Not an inconsistency.

3. **[CHECK] citation resolution — PASS.** All name/year citations now carry full
   bibliographic footnotes drawn from the VERIFIED list (ch33 already had them; ch34/ch35
   now do). Mentor3's Deng-Gao-Kim rendered exactly per the anchor list. Only the
   non-verified Lee-McCrary-Moreira-Porter stays as an in-text hedge (correctly not
   fabricated).

4. **Voice & audience — PASS.** No emojis (grep-confirmed), no marketing voice. Reveal-the-
   trick structure intact throughout; student cast (Maya/Devon/Priya/Sam/Leah) consistent
   with CONVENTIONS hooks; terms defined on first use; concrete-before-abstract maintained.

5. **Identification honesty — PASS.** CIA and exclusion both repeatedly and correctly
   described as UNTESTABLE (ch32 §3.2.8, ch33 §6, ch34 §3, mentor3, assessment honesty row).
   Matching/PSM explicitly framed as still selection-on-observables = the same OVB threat as
   Week 2 (ch32 maps it to β₂δ₁). Weak-IV correctly described: 2SLS biased TOWARD OLS,
   conventional CI too narrow/over-confident, AR valid regardless of strength, unbounded AR
   = unidentified, empty AR = over-id/exclusion failure. AIPW "doubly robust = either model
   right" stated correctly and the rubric explicitly penalizes the "need both" error. No
   causal overclaiming detected.

---

## (d) Length-vs-target & accept/expand recommendation

Word counts (from handoffs, spot-confirmed): ch31 ~6,168 / ch32 ~5,780 / ch33 ~6,330 /
ch34 ~7,350 / ch35 ~6,230 (all on/near targets; ch34 long but justified by IV's load).
PS3.1–3.5 each 6 escalating problems / 100 pts with full solutions. lab3 ~5,470 words,
fully runnable. mentor3 ~2,490 (slightly over the ~2,200 target). assessment3 ~3,986 words.
Notebooks nb3.1–3.5 all valid nbformat v4, outputs cleared, 23–26 cells.

**Recommendation: ACCEPT Week 3 as-is** (with the citation fixes now applied). This is the
cleanest week reviewed so far — all answer-key arithmetic is correct to the digit, the
weak-IV simulation reproduces independently, and identification honesty is exemplary. No
expansion needed. Optional, low-priority human follow-ups: (i) verify + add the
Lee-McCrary-Moreira-Porter (2022) cite, (ii) trim mentor3 by ~290 words if a hard 2,200 cap
is enforced. Neither blocks publication.
