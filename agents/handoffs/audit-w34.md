# `[AuditAgent -> Weeks 3-4 deep four-dimension audit]`

Four-dimension deep audit of Weeks 3–4 (causal inference): `[CHECK]` resolution,
math/answer-key correctness, cross-file consistency, voice/style. Every load-bearing
number re-derived in `python3`; every staggered-DiD / RD / shift-share citation
verified against the pre-cleared bibs in the brief; the Russell-RD debate and HMDA
API `[CHECK]`s preserved per instructions. Verdict: **PASS** with **2 minor edits
applied** (citation polish on ch43 / ch44).

---

## (a) Fixes APPLIED

1. **`book/weeks/week-04/ch43-regression-discontinuity.md` (§8, lines ~180–183).**
   Added an explicit footnote `[^cjm]` for the Cattaneo–Jansson–Ma density estimator
   that ch43 previously named inline without bibliographic info. Full cite:
   *Cattaneo, M. D., Jansson, M., & Ma, X. (2020). Simple Local Polynomial Density
   Estimators. JASA 115(531), 1449–1455.* (Verified via WebSearch against `tandfonline`
   / `escholarship` listings.) Consistent with ch43's existing footnote style
   (`[^mccrary]`, `[^cct]`, `[^ik]`, `[^gelmanimbens]`, `[^leelemieux]`).

2. **`book/weeks/week-04/ch44-synthetic-control.md` (intro, lines ~7–9).** Added
   explicit footnotes for the three load-bearing primary sources that ch44 had been
   citing inline-only:
   - `[^ag]` Abadie & Gardeazabal (2003), *AER* 93(1), 113–132;
   - `[^adh]` Abadie, Diamond & Hainmueller (2010), *JASA* 105(490), 493–505;
   - `[^sdid]` Arkhangelsky, Athey, Hirshberg, Imbens & Wager (2021), *AER* 111(12),
     4088–4118.
   All three are on the pre-verified brief list; matches the ch42 / ch45 footnote
   pattern. (CONVENTIONS §6 — every empirical claim cites at least one primary source
   with full bibliographic info.)

No other edits were needed: the prior W3/W4 ReviewerAgent passes already resolved all
non-`[CHECK]` citations to the verified-bib standard; my pass only filled the two
remaining footnote gaps.

---

## (b) `[CHECK]` resolution (Dimension 1)

Six `[CHECK]` tags remain in the W3/W4 corpus, all intentionally per the brief:

| File | Line | Tag | Disposition |
|---|---|---|---|
| `ch43-regression-discontinuity.md` | 210 | Russell-debate (Appel–Gormley–Keim vs Wei–Young vs Glossner) | **KEEP** — genuine human-verify |
| `lab4-hmda-did.md` | 70 | HMDA pinned vintage / CFPB snapshot | **KEEP** — live API |
| `lab4-hmda-did.md` | 94 | HMDA query param names per vintage | **KEEP** — live API |
| `lab4-hmda-did.md` | 123 | CFPB API param-name stability | **KEEP** — live API |
| `lab4-hmda-did.md` | 409 | Path A vintage `[CHECK]` checklist item | **KEEP** — live API |
| `lab4-hmda-did.md` | 451 | CFPB Data Browser bib + vintage | **KEEP** — live API |

The brief-supplied "verified" bibs (staggered-DiD + RD + SC + SDID + shift-share +
BJB + Lee–Lemieux + CJM) are all already on disk with no `[CHECK]` flag attached.
Spot-confirmed by WebSearch where the brief flagged uncertainty (BJB 1995 JASA
90(430):443–450; Lee–Lemieux 2010 JEL 48(2):281–355; CJM 2020 JASA 115(531):1449–
1455) — every page range / volume / issue matches the brief and the on-disk
footnote.

---

## (c) Math/answer-key correctness (Dimension 2) — every spot-check PASSED

Every numeric result below was re-derived in `python3` (NumPy / `statsmodels` /
`fractions` / `scipy.optimize`).

**PS3.1 selection-bias decomposition.** Holland ID, $\Delta = \text{ATT} + \text{selection bias}$
identity is exact (no assumption). P5 exhaustive enumeration over $\binom{8}{4}=70$
balanced-coin assignments: mean realized bias is exactly 0; max +1, min −1; 36 zero-bias
splits, 16 splits at ±0.5. Matches the solution's "verified exhaustively" claim. ✓

**PS3.3 IPW / EB / AIPW.** HT $-11/6 \approx -1.833$, Hájek $-1.000$, $N_\text{eff}$
collapses $2.69 \to 1.24$ after the propensity-explosion to $\hat e_C = 0.02$. EB
weights $(\tfrac35, \tfrac25)$ for the 2-control sub-problem; EB-ATT $= -2.000$.
AIPW four cases reproduced exactly with `fractions`: both-correct $\hat\tau = 3$;
propensity-wrong $= 3$ (zero residual within stratum); outcome-wrong $= 3$;
**both-wrong $= 13/3 \approx 4.333$**, which is the naive difference — the doubly
robust property does the right thing in three cases and fails predictably in the
fourth. ✓

**PS3.4 Wald + 2SLS-as-FWL covariance ratio.** $\sum \tilde Z \tilde D = 4$,
$\sum \tilde Z \tilde Y = -12$, $\hat\beta_{2SLS} = -3$ (vs OLS $= -2$, attenuated).
P6: $F=36 \to \text{bias}=0.042 \to E[\hat\beta]=2.04$; $F=2.56 \to \text{bias}=0.586
\to E[\hat\beta]=2.59$ (39% of the way back to OLS). Both match exactly. ✓

**PS3.5 $1/(F+1)$ heuristic.** $F = 42, 3.8, 5.2, 7.5$ give $0.023, 0.208, 0.161, 0.118$
— matches the solution prose's "about 2.3%", "about 21%", "about a sixth", "about 12%".
Also confirms the BJB lesson is wired through ch35, lab3, and the answer key
identically (BJB 1995 JASA 90(430):443–450 cited fully in ch35 footnote `[^bjb]`). ✓

**PS4.2 Goodman-Bacon worked decomposition.** Independently re-built the 3-state,
5-year world and re-ran the TWFE regression with `statsmodels`. **TWFE
$\hat\beta = -1.2500$** exactly. The four GB weights $(\tfrac14, \tfrac38, \tfrac18,
\tfrac14)$ sum to 1; reassembling with the four 2×2 DiDs $(-2.5, -1.5, -1.5, +0.5)$
returns exactly $-1.25$. The dCDH cell-weight decomposition (sums to 1, with two
negative $-0.0625$ weights on E's $t=4,5$ cells — exactly the cells where E served
as the contaminated control in the forbidden Type-3 2×2) reassembles to $-1.25$ as
well. **Drop the never-treated unit**: $w_C, w_D \to (\tfrac13, \tfrac23)$, TWFE
collapses to $-\tfrac16 \approx -0.167$. **Extend to the chapter's 8-year, no-never-
treated world** ($G_E=2, G_L=5, T=8$): I re-ran TWFE and got $\hat\beta = +0.40$ exactly
— the **sign flip** is real and exactly reproduces ch42 §3, nb4.2 World A, and ps4.2
P3(d) on a single re-derivation. The forbidden-comparison sign-flip is wired
identically through chapter ↔ notebook ↔ answer key. ✓

**PS4.3 fuzzy-RD Wald.** Jump in $D = 0.40$, jump in $Y = 3.6$, $\hat\tau_{FRD} = 9.0$.
Weak-instrument case $3.6/0.05 = 72$ (the ratio explodes as the first stage collapses
— Ch 3.5 weak-IV pathology in RD clothing). P3 CIs at $h=4$ (`[0.207, 0.913]`, contains
truth 0.60) vs $h=32$ (`[0.273, 0.547]`, **excludes** truth 0.60): exact. ✓

**PS4.4 placebo $p = \text{rank}/(J+1)$.** I re-ranked all 10 RMSPE ratios:
treated = 8.00 (singular largest, more than triple the runner-up at 2.50). So
$\text{rank} = 1$ and $p = 1/10 = 0.10$. With $J=8$ donors the smallest reportable
$p$-value is $1/9 \approx 0.111$, so a "$p<0.01$" claim is **mathematically
impossible** for a single-treated-unit SC test with 8 donors — the answer key's
sharpest point and it survives independent re-derivation. Floor argument: $J=19$
gives floor exactly $1/20 = 0.05$, fails strict $<0.05$; need $J \ge 20$. ✓

**PS4.5 Bartik construction + Rotemberg weights.** $B_\text{Lake} = -3.9\%$,
$B_\text{Mill} = +2.0\%$, identical-shares vector $\to -1.1\%$ (kills share dispersion),
common-shift world $\to +2.0\%$ (kills shift dispersion). P3 Rotemberg weights
$(0.70, 0.22, 0.05, 0.03)$ sum to $1.00$ and recompose to $\hat\beta_1^\text{Bartik}
= +0.60$ — BigCommerce carries 70% of the weight. $K_\text{eff}$: 5.0 for the
dispersed Sample A, $1/0.6538 \approx 1.53$ for the concentrated Sample B
(80%/10%/5%/3%/2%). All exact. ✓

---

## (d) Cross-file consistency (Dimension 3) — PASS

**nb3.5 conventional vs Anderson–Rubin coverage.** The Monte Carlo at `SEED=20260528,
PI_WEAK=0.05`, 1200 reps, $N=300$ produces conventional coverage **0.801** and AR
coverage **0.973** — quoted as exactly those values in `lab3-weak-iv-pathology.md`
line 271–272. The chapter prose (ch35) uses qualitative language ("badly UNDER",
"back near 0.95") without pinning a numeric value, which is consistent with the
lab's "your numbers will match if you seed correctly" framing. Brief's "~0.80 vs
~0.97" expectation is met. ✓

**nb4.2 TWFE sign-flip + Callaway–Sant'Anna.** World A (E, L only, no never-treated)
TWFE = $+0.40$ — World A is explicitly the chapter §3 sign-flip world. World B (with
never-treated state N) TWFE = $-1.227$ (attenuated, right sign); CS overall ATT =
$-2.571$ exactly, recovering the true mean $-13/6 \times \text{(scaling)}$ profile.
Brief's "+0.40 vs Callaway–Sant'Anna -2.571" coherent across `ch42` §3 (line 66:
"$\hat\beta = +0.40$"), `ch42` §7 closing paragraph (line 160: "the overall clean ATT
comes out at the true $-2.57$"), `nb4.2` cells 295–296 and 376 (chapter: $-2.571$),
and `ps4.2` solution P5. ✓

**nb4.4 placebo $p \approx 0.032$.** `nb4.4` line 38 explicitly says "**1 of 31** with
permutation $p \approx 0.032$" — i.e. rank 1 of 31 = $1/31 \approx 0.0323$. Matches
the ch44 §6 placebo derivation and ps4.4 P4(d)(ii)'s floor logic ($p \ge 1/(J+1)$).
Note: ps4.4 P4 uses a *different* worked example with $J=9$ donors (so $p=0.10$ at
the floor); this is not a conflict, as the previous W4 review flagged. ✓

**nb4.5 2SLS = −1.50, F ≈ 571.** `nb4.5` line 29 headline: "OLS $\approx -1.06$, 2SLS
$\approx -1.50$, first-stage $F \approx 571$, Rotemberg weights reassembling to
$-1.50$"; reproduced in the carry-forward paragraph (line 584). ch45 narrative
matches. ✓

**Maya's HMDA narrative consistency across ch41 / ch42 / lab4 / nb4.2 / PS4.x.**
This is the load-bearing protagonist-and-design coherence check. The cast assignments
are deliberately split:
- **Priya** is the protagonist for ch41 (climate-insurance regulation, single-state
  DiD), ch42 (climate-risk disclosure non-renewals, staggered adoption), ch44
  (climate-disclosure mandate synthetic control), ps4.1, ps4.2, ps4.4.
- **Maya** is the protagonist for lab4 (HMDA + state fair-lending policy DiD), ps4.3
  (credit-score cutoff at 660, RD), and ps4.5 (regional bank-share Bartik for
  delinquency).
- **Sam** anchors ch43 (Russell 1000/2000 RD) and ch45 (Bartik for trade exposure);
  shares ps4.3 (Russell version) and ps4.5 (Bartik intro) with Maya.

This split is **internally consistent** — the chapters teach the methods on Priya's
recurring climate-insurance arc, while Maya carries the fair-lending HMDA capstone
material (lab4 + her two finance-flavored problem sets). lab4 §A.4 names HMDA's
treatment-spec discipline ("Outcome = county-year denial rate; Treatment = state
fair-lending regulatory change, staggered adoption $G_i$ per state; ...; Identifying
assumption: per-cohort parallel trends, assumed not tested"), which lines up with
the ch42 staggered-DiD machinery Maya is meant to use. The brief's "Maya's HMDA
design coherent across ch41/ch42/lab4/nb4.2 plus PS4.x" reads correctly as "the
HMDA design Maya runs in lab4 *uses* the ch41/ch42 machinery and is referenced from
ps4.3/ps4.5" — not as "Maya is the protagonist of ch41/ch42" (she isn't, and
shouldn't be: the cast variety is by design per CONVENTIONS §2). ✓

---

## (e) Voice / style (Dimension 4) — PASS

- **No emojis.** Greppped the full W3/W4 prose + solutions + notebooks; zero
  emoji glyphs. The only non-ASCII non-Latin character is the standard `✓` checkmark
  used in math verification ("✓" after a derived equality) and the Hájek `é` —
  both conventional, neither decorative.
- **No banned phrases.** No "controls for endogeneity", no "in today's fast-paced
  world", no "now more than ever", no "since the dawn of finance". Every use of
  "proves" is either (a) a mathematical claim about a theorem (Goodman-Bacon proves,
  de Chaisemartin–D'Haultfœuille prove, Rosenbaum–Rubin proved), (b) a quoted
  classmate's *wrong* sentence held up as the banned overclaim ("Balance this good
  *proves* the matching removed the selection bias" — followed by why this is wrong),
  or (c) the verification idiom "a box is checked only when you can point to the
  number or figure that proves it" (lab pedagogy).
- **Cast consistency.** Maya / Devon / Priya / Sam / Leah used per CONVENTIONS §2 with
  their stable interest hooks (Maya = student debt / fair lending; Devon = crypto;
  Priya = climate / ESG / insurance; Sam = sports/markets/momentum, used here for
  Russell-RD and trade exposure; Leah = patents/innovation, used here for the
  examiner-leniency IV in ps3.5). No protagonist drift across chapter ↔ PS pairings.
- **Causal-language discipline.** Every load-bearing assumption is flagged
  *untestable*: PT in ch41 ("an assumption about a counterfactual, refutable by a
  pre-trend, never confirmed by one"), RD continuity in ch43 ("comparability *in
  the limit, exactly at the cutoff*"), compound-treatment in ch43 §9 ("no statistical
  test can detect this"), no-anticipation / no-interference in ch44, shift-share
  exclusion in ch45 ("an argument, not a citation and not a statistic"). The
  estimands (ATE / ATT / ATC / LATE / ATT(g,t) / RD-at-cutoff / FRD complier LATE)
  are named *before* every estimation. No overclaiming.
- **Appendix-D table compliance.** All W3/W4 markdown tables are pedagogical /
  worked-example tables (rosters of potential outcomes, IPW weight tabulations,
  rank orderings) — not the formal LaTeX-rendered regression output the D.1
  `booktabs` standard governs. The one regression-style markdown table in
  PS3.5 P1 ("Read an IV paper like a referee") uses three horizontal seams
  (top / mid / bottom rules implicit in markdown), no vertical rules, and stacks
  estimates above their SE flavor per D.2 — appropriate for the pedagogical context.

---

## (f) PASS / FAIL by dimension

| Dimension | Result | Notes |
|---|---|---|
| **1. `[CHECK]` resolution** | **PASS** | 6 remaining `[CHECK]`s all on the brief's KEEP list (Russell debate + 5 HMDA API/vintage). All verified bibs in the brief match on-disk footnotes byte-for-byte (BJB 90(430):443–450; Lee–Lemieux 48(2):281–355; CJM 115(531):1449–1455 — added as new footnote; Goodman-Bacon, dCDH, Sun–Abraham, Callaway–Sant'Anna, BJS, IK, CCT, McCrary, Abadie–Gardeazabal, ADH, Arkhangelsky et al., GPSS, BHJ, AKM — all present and correct). |
| **2. Math/answer-key correctness** | **PASS** | All 7 brief-specified spot-checks (PS3.1 sel-bias / PS3.3 IPW+EB+AIPW / PS3.4 Wald+FWL / PS3.5 1/(F+1) / PS4.2 GB / PS4.3 fuzzy-RD / PS4.4 placebo $p$ / PS4.5 Bartik+Rotemberg) re-derived in `python3` and match the solution keys to the last decimal. The sign-flip $+0.40$ is reproduced from scratch in the 8-year no-never-treated world. |
| **3. Cross-file consistency** | **PASS** | nb3.5 conventional 0.801 / AR 0.973 (brief: ~0.80/~0.97 ✓); nb4.2 TWFE +0.40 / -1.227 / CS -2.571 (brief ✓); nb4.4 placebo $p \approx 0.032 = 1/31$ (brief ✓); nb4.5 2SLS -1.50 / F ≈ 571 (brief ✓); Maya HMDA narrative coherent across lab4 ↔ ch42 machinery ↔ ps4.3 / ps4.5 (Maya not in ch41/ch42 prose by design, per cast split — Priya owns those). |
| **4. Voice/style** | **PASS** | No emojis, no banned phrases, cast stable per CONVENTIONS §2, every untestable assumption flagged, every estimand named before estimation, tables appropriate for context. |

---

## (g) Length / scope note

W3 chapters total ~46-90 KB each; W4 chapters ~30-50 KB; solutions 21-25 KB each;
notebooks 30-70 KB. All five W3 and five W4 problem-set solutions are mathematically
exhaustive and stylistically uniform. Nothing trimmed, nothing expanded.

---

*Overall: Weeks 3-4 PASS all four dimensions. 2 minor edits applied (ch43 CJM
footnote, ch44 SC/SDID footnotes — both filling load-bearing primary-source
citations to CONVENTIONS §6 standard, both verified against the brief's pre-cleared
bibs). The math is clean throughout — no numerical errors found, no cross-file
contradictions, no fabricated citations, no overclaiming. The brief's six
intentional `[CHECK]`s (Russell debate + HMDA live API) are preserved as the
designated human-verify items.*

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
