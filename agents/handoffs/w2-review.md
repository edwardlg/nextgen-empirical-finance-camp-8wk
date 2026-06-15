# [ReviewerAgent -> Week 2 verdict]

Reviewed all of Week 2 (ch21–ch25, ps2.1–ps2.5 + solutions, lab2, mentor2, assessment2, nb2.1–nb2.5,
and the writer/coder handoffs). Spot-checked every solution's math in `python3` (numpy/statsmodels
0.14.6) and re-ran the live notebook code where numbers are cross-claimed. Applied fixes below.

## (a) Fixes APPLIED (file · change · why)

1. **ch25 §Your Turn — broken notebook filename.**
   `nb2.5-biased-estimators.ipynb` → `nb2.5-biased-estimator-lab.ipynb` (the real file). Why: the cited
   notebook did not exist under the old name; ps2.5 already used the correct name, so this reconciles
   chapter↔ps↔notebook. (Flagged by w2-nb25 / w2-ps25.)

2. **ch25 §2.5.4 — size–momentum SIGN [CHECK] removed and reworded.** Rewrote Sam's second example so the
   *mechanics* of the two-sign rule are the point, framed entirely as "suppose these signs" with an
   explicit "flip either sign and the rule flips the bias." Why: the spec said not to assert an empirical
   sign; the [CHECK] tag is now gone and no empirical claim is made.

3. **ch24 §6 Maya SE table — marked illustrative + pointed at nb2.4 for live numbers.** Added a lead-in and
   caption noting the table figures are illustrative (not script output) and citing nb2.4's real,
   reproducible magnitudes (β̂=0.30; classical t=5.70 → firm-cluster t=2.19 on the 120×20 panel). Why:
   WriterAgent flagged the table as hypothetical; this makes it honest while keeping the qualitative point.
   VERIFIED by re-running nb2.4 code: β̂=0.30 exactly, classical SE=0.0526 (t=5.70), firm-cluster SE=0.1372
   (t=2.19) — the cited numbers are correct to the decimal.

4. **ch23 §7 — inserted verified Frisch–Waugh (1933) and Lovell (1963) citations** as footnotes, replacing
   the in-text-only attribution flagged `[CHECK]` in the handoff. Full bib per the verified reference list.

5. **ch24 §3.4 — inserted verified Cameron, Gelbach & Miller (2008, RESt) and (2011, JBES) citations**,
   dropping both `[CHECK pages]` tags. Full bib per the verified reference list.

6. **ch24 §3.2 — corrected the HC0-vs-classical statement (statistics precision).** Old text said HC0
   "just converges to the classical answer" under homoskedasticity. Rewrote to state correctly that **HC1**
   (which carries the N/(N-K) factor) reproduces the classical SE almost exactly under homoskedasticity,
   while raw HC0 runs a touch *smaller*. Why: spec check 5 — HC1, not HC0, matches classical. The ps2.4
   solution already had this right (HC0 differs by √((N-K)/N)); the chapter now agrees.

## (b) Remaining [CHECK] / human items

- **No [CHECK] tags remain** anywhere in Week 2 chapters, problem sets, solutions, lab, mentor, or
  assessment (grep clean).
- **lab2 Path A (WRDS/CRSP query)** is illustrative and was NOT executed (no off-infra creds, by design).
  An editor with Hopper access should sanity-check exact `crsp.msf` / `ff.factors_monthly` table/column
  names against the current WRDS schema before students run it. Carried forward from w2-lab2.
- **mentor2** cites Gao, Han, Kim & Pan (2024) JCF for method/inference only (no fabricated results) — fine
  as written; confirm the exact JCF volume/pages against Prof. Gao's CV at typeset time.

## (c) PASS/FAIL on the 5 checks

1. **Answer-key & solution correctness — PASS.** Every numeric result re-derived and confirmed:
   - ps2.1: β̂=(-0.5, 1.6), det=20, leverages (3/5,3/10,1/5,3/10,3/5) sum=2, TSS=13/ESS=12.8/RSS=0.2,
     R²=64/65≈0.985, κ(0.98)=99 with ~1.4% RHS nudge → ~99% β swing. ✓
   - ps2.2: se(β̂₁)=√(1.44/99600)=0.003802; OVB → 0+(-0.6)(0.7)=-0.42. ✓
   - ps2.3: FWL slopes 7/3.5=2.0, 24/16=1.5, short 37/26≈1.423; matches ch23/nb2.3 β̂₁=1.838 story. ✓
   - ps2.4: classical SE 0.462, HC0 0.548, ratio 1.18; Moulton 1+199(0.04)=8.96, √=2.99, t 6.0→2.0;
     NW L=4*(1.2)^(2/9)=4.17→4. ✓
   - ps2.5: λ=1/1.25=0.8, plim -0.15, attenuation sweep 2.0/1.6/1.0/0.4. ✓
   - assessment A5: 1+999(0.05)=50.95 (~51×), √=7.14 (~7×). ✓
   - lab2 Path B re-run: γ₁=0.00550 (planted 0.006), FM t=3.78, corr=0.935, pooled SE 0.000598 ≪ FM SE
     0.001456. ✓
   No errors found.
2. **Flagged items (filename, SE table, cross-number agreement) — PASS** (fixes 1, 3 above; ch23/nb2.3/ps2.3
   β̂₁=1.838 confirmed identical by running the chapter code; ch24 t-collapse confirmed against live nb2.4).
3. **[CHECK] citations — PASS.** FWL/Lovell and CGM 2008/2011 inserted from the verified list; size–momentum
   sign reworded to mechanics-only. No [CHECK] remain.
4. **Notation & voice — PASS.** Bold $\mathbf{X}/\mathbf{H}/\mathbf{M}$ and hats used consistently; student
   cast stable (Maya/Sam/Priya/Devon/Leah); reveal-the-trick structure intact; no emojis/marketing.
5. **Correct statistics throughout — PASS.** OVB/measurement-error/functional-form correctly framed as BIAS
   (ledger §2.5.11 keeps the bias vs SE columns clean); heteroskedasticity/clustering kept in the SE column;
   no "p = P(null true)" anywhere; robust flavors now described correctly (HC1↔classical fix applied).

## (d) Chapter length vs. TOC targets

Chapters came in (wc -w incl. code/LaTeX): ch21 ~4,800 (~6,200 effective), ch22 ~5,100, ch23 ~6,140,
ch24 ~6,280 (slightly higher after citation/precision edits), ch25 ~6,415. The technical chapters (23–25)
hit their ~6,400 targets; ch21–22 sit in the established Week-1 technical band (4,300–5,500). **Recommendation:
ACCEPT as-is.** ch21 and ch22 are foundational linear-algebra/Gauss–Markov chapters where the LaTeX-heavy
display math undercounts prose density; they read complete and do not need padding. No expansion warranted.

**Overall: Week 2 PASSES.** Six concrete fixes applied; all solution math independently verified; chapter
↔ notebook ↔ problem-set numbers agree where claimed.
