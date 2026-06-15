# Audit — Weeks 5–6 (Reading the Frontier; Text & AI)

**Auditor:** AuditAgent
**Date:** 2026-05-30
**Scope:** `book/weeks/week-05/`, `book/weeks/week-06/`, W5/W6 notebooks, `book/appendices/E-solutions-manual/E-w5-*` and `E-w6-*`.

---

## Headline verdict

**PASS, with one targeted edit applied.** W5/W6 are in good shape across all four audit dimensions. The numerical answer-key arithmetic in PS5.1–PS5.5 and PS6.1–PS6.5 was independently re-verified in Python with exact-rational and `sklearn` cross-checks; every figure that should match does match (PS6.5's macro-F1=0.778 and Cohen's κ=0.661, the most error-prone set, came out exactly as the solution claims, including the closed-form `38/49`, `11/15`, `14/17`, `39/59` simplifications). The AI module's secrets discipline, env-gating, and Anthropic API pattern (`claude-opus-4-7`, `cache_control: ephemeral`, `cache_read_input_tokens` verification) are intact. The cross-file expected values for nb5.4 (right-clustering coverage ≈ 0.95, wrong ≈ 0.35), nb5.5 (naive ≈ 0.35 vs clustered ≈ 0.05), and nb6.5 (macro-F1 ≈ 0.86, κ ≈ 0.80) all reproduce when the notebooks are run.

## Edit applied

**1. `book/weeks/week-05/ch54-readers-guide-petersen-2009.md`, §6 "Vulnerabilities," "The gap to two-way clustering."**

Resolved one bibliographic `[CHECK]` by verifying via WebSearch that the two-way / multiway clustering papers were both published in 2011, post-Petersen. Replaced the in-line `[CHECK]` with full verified citations:

- Cameron, Gelbach & Miller (2011), "Robust Inference With Multiway Clustering," *Journal of Business & Economic Statistics*, 29(2), 238–249.
- Thompson (2011), "Simple formulas for standard errors that cluster by both firm and time," *Journal of Financial Economics*, 99(1), 1–10.

The other `[CHECK]`s in the chapter (Petersen's survey-paper counts, simulation grid sizes, real-data choices, BDM's exact ~0.45 rejection rate) were retained — per the audit charter these are legitimate human-verify items tied to the paper's own internal tables.

## Dimension 1 — `[CHECK]` resolution

All bibs on the audit charter's verified list are already cited correctly in their canonical form in W5/W6:

| Citation | Where it lives | Status |
|---|---|---|
| Fama & French (1992) JF 47(2):427–465 | `ch51` line 11 | ✓ correct |
| Fama & French (1993) JFE 33(1):3–56 | `ch52` line 3 | ✓ correct |
| Jegadeesh & Titman (1993) JF 48(1):65–91 | `ch53` / `ps5.3` line 5 | ✓ correct |
| Petersen (2009) RFS 22(1):435–480 | `ch54` line 3 | ✓ correct |
| Bertrand, Duflo & Mullainathan (2004) QJE 119(1):249–275 | `ch55` line 3 | ✓ correct |
| Gibbons, Ross & Shanken (1989) Econometrica 57(5):1121–1152 | `ch52` line 65 (footnote `[^grs]`) | ✓ correct |
| De Bondt & Thaler (1985) JF 40(3):793–805 | `ch53` line 21 (footnote `[^dbt]`) | ✓ correct |
| Harvey, Liu & Zhu (2016) RFS 29(1):5–68 | `ch51` line 137 (footnote `[^hlz]`) | ✓ correct |
| Kogan, Papanikolaou, Seru & Stoffman (2017) QJE 132(2):665–712 | `ch61` line 9 | ✓ correct |
| Hoberg & Phillips (2016) JPE 124(5):1423–1465 | `ch62` line 3 | ✓ correct |
| Loughran & McDonald (2011) JF 66(1):35–65 | `ch63` / `E-w6-ps6.3` line 5 | ✓ correct |
| Bartlett, Morse, Stanton & Wallace (2022) JFE 143(1):30–56 | `ch64` line 3 | ✓ correct |
| Bhutta, Hizmo & Ringo (2022) FEDS WP 2022-067 / 2025 JF | `ch64` line 5 (editor's-choice note at line 7) | ✓ correct (both venues presented exactly as the charter prescribes) |
| Cameron, Gelbach & Miller (2008) ReStat 90(3):414–427 | `ch54` line 103; `E-w5-ps5.4` line 65; `E-w5-ps5.5` | ✓ correct |
| Cameron, Gelbach & Miller (2011) JBES 29(2):238–249 | `ch54` line 105 (**newly inserted** in this audit, replacing `[CHECK]`) | ✓ correct |
| Thompson (2011) JFE 99(1):1–10 | `ch54` line 105 (**newly inserted** in this audit) | ✓ correct |

Bound, Jaeger & Baker (1995) JASA 90(430):443–450 — not referenced in W5/W6 (it belongs to the Week-3 IV chapter). No action.

`[CHECK]`s retained as legitimate human-verify items, per the audit charter:

- `ch54` lines 21, 51, 53, 103: Petersen's survey statistics and simulation grid specifics (exact $N$/$T$/$S$, real-data datasets).
- `ch55` line 55 and `ps5.5` line 33: the precise BDM ~0.45 rejection rate.
- `ch53` lines 43, 63: JT93's skip convention and exact 6/6 figure (the chapter explicitly tells the student to read order-of-magnitude only).
- `ch52` line 101: a meta-instruction ("tag any specific figure you carry forward as `[CHECK]`"), not a bib.
- `ch62` lines 37, 63: Hoberg–Phillips's exact vector construction and TNIC data-library URL.
- `ch64` line 45: exact sample period / loan counts for Bartlett et al.
- `ch65` lines 280, 285, 295 and `nb6.5` real-API cells: GMU Azure deployment name and `api_version` (`gpt-4o`, `2024-10-21`).
- `reading-guide-pack-6` line 214: WRDS/Azure-APIM data-handling clearances.
- `ch65` (Loughran–McDonald headline misclassification %): the qualitative "large share, on the order of three-quarters" with the precise percentage tagged `[CHECK]` is the right discipline; the `nb6.3` toy reproduces the *shape* without claiming a number.

## Dimension 2 — Math / answer-key correctness

Every flagged item was re-verified by independent Python computation (exact-rational `fractions.Fraction` where helpful, plus `numpy`/`sklearn` cross-checks).

| Item | Solution claim | Independent verification | Status |
|---|---|---|---|
| **PS5.1 §2** size premium | 1.62 − 0.78 = 0.84 %/mo | 0.84 | ✓ |
| **PS5.1 §2** value premium | 1.95 − 0.72 = 1.23 %/mo | 1.23 | ✓ |
| **PS5.1 §3** within-size β spreads | (−0.01, −0.01, +0.02) | exact | ✓ |
| **PS5.1 §3** within-β size spreads | (+0.66, +0.72, +0.63) | exact | ✓ |
| **PS5.1 §4** FM $t$'s spec C | β:−0.2, size:−3.9, BE/ME:+4.4 | −0.15→−0.2; −3.89; +4.44 | ✓ |
| **PS5.1 §4** inverted SD | 0.090·√330 ≈ 1.63 | 1.6349 | ✓ |
| **PS5.1 §5** multiple-testing | $1−0.95^{20}=0.6415$ | 0.6415 | ✓ |
| **PS5.2 §1** SMB | (small mean − big mean) = 0.25 | 0.25 | ✓ |
| **PS5.2 §1** HML | 1.175 − 0.375 = 0.80 | 0.80 | ✓ |
| **PS5.2 §3** CAPM α | 0.90 − 0.55 = 0.35 | 0.35 | ✓ |
| **PS5.2 §3** FF3 α | 0.90 − 0.86 = 0.04 | 0.04 | ✓ |
| **PS5.3 §3d** $t$'s | classical 12.05, HAC 8.20 | 12.0482, 8.1967 | ✓ |
| **PS5.3 §4b** net at $c=50$ / $c=100$ | 58 / 28 bps | 58, 28 | ✓ |
| **PS5.3 §4c** break-even $c^\star$ | (100 − 40·0.30)/0.60 = 146.7 bps | 146.6667 | ✓ |
| **PS5.4 §3** Petersen self-diagnostic ratios | firm/white 1.07, time/white 2.86 | 1.0714, 2.8571 | ✓ |
| **PS5.5 §3** AR(1) covariance sum, $T=3,\rho=0.8$ | sum 7.48; Var(ū)=0.8311; ratio 2.49 | 7.48, 0.8311, 2.4933 | ✓ |
| **PS5.5 §3** long-run inflation $(1+\rho)/(1-\rho)$ | 9 at $\rho=0.8$; $\sqrt 9=3$ | 9, 3 | ✓ |
| **PS5.5 §3** $T_{\text{eff}}=20\cdot0.2/1.8$ | 2.22 | 2.222 | ✓ |
| **PS5.5 §2** MC SE of $\hat p=0.35$, R=600 | 0.019; (0.35−0.05)/SE ≈ 15.4 σ | 0.0195, 15.41 | ✓ |
| **PS6.1 §2** day-by-day AR | 0.76, 1.25, 0.37 % | exact | ✓ |
| **PS6.1 §2** 3-day CAR | 2.38 %; gross $190.4 M; ξ=$380.8 M | exact | ✓ |
| **PS6.1 §2** 5-day CAR | 2.235 %; $178.8 M | exact | ✓ |
| **PS6.1 §5** noise shrink $1/\sqrt n$ | 1.00, 0.50, 0.20, 0.10 | exact | ✓ |
| **PS6.2 §1** sim(A,B) | 6/7 = 0.857 | 0.8571 | ✓ |
| **PS6.2 §1** ‖A‖=‖B‖=√7, ‖C‖=2 | √7=2.6458, 2 | exact | ✓ |
| **PS6.2 §2** IDF at N=4 | 0, 0.288, 0.693, 1.386 | exact (ln) | ✓ |
| **PS6.2 §2** count-cosine of oil vs airline w/ "company" | 2/8 = 0.25 | 0.25 | ✓ |
| **PS6.3 §1** tokenization S1–S4 | 9/13/11/12 tokens | exact (re.findall) | ✓ |
| **PS6.3 §1** naive hits and NegScores | 4/9=44.44%, 4/13=30.77%, 1/11=9.09%, 0 | exact | ✓ |
| **PS6.3 §2** LM hits / NegScores | 0, 0, 2/11=18.18%, 0 | exact | ✓ |
| **PS6.3 §3** TF-IDF mass P vs Q | 0.6931 vs 2.7726 | exact | ✓ |
| **PS6.4 T2** Blinder–Oaxaca | raw 18 = explained 12 + residual 6 | exact, both routes | ✓ |
| **PS6.4 T2** risk-adjusted means | −8 and −14, diff 6 | exact | ✓ |
| **PS6.4 T3** OVB | β_score·δ = (−0.5)(−20) = +10; apparent 14 | exact | ✓ |
| **PS6.5 T2** per-class P/R/F1 | G(0.792, 0.760, 0.776), M(0.733, 0.733, 0.733), O(0.812, 0.836, 0.824) | exact incl. closed forms 38/49, 11/15, 14/17 | ✓ |
| **PS6.5 T2** macro P/R/F1 | 0.779, 0.776, 0.778 | exact (0.7789, 0.7764, 0.7775) | ✓ |
| **PS6.5 T2** accuracy | 116/147 = 0.789 | 0.7891 | ✓ |
| **PS6.5 T3** Classifier A acc / B acc | 0.9500 / 0.9300 | exact | ✓ |
| **PS6.5 T3** B precision/recall/F1 | 0.40 / 0.80 / 0.5333 = 8/15 | exact | ✓ |
| **PS6.5 T4** Cohen's κ | p_o=0.84, p_e=0.528, κ=0.661 = 39/59 | exact | ✓ |
| **Assessment-6 worked binary example** | P=0.80, R=0.667, F1=0.727, acc=0.70 | exact | ✓ |
| **Assessment-6 worked κ example** | p_e=0.62, κ=0.474 | exact | ✓ |

**No arithmetic errors found.** The PS6.5 set, flagged as the most error-prone, is in fact unusually clean: every per-class score equals the simple integer-ratio its setup forces, and the solution explicitly notes the two acceptable roundings of macro-F1 (0.777 from exact averaging, 0.778 from averaging the 3-dp rounded per-class values), which is the correct way to handle it.

## Dimension 3 — Cross-file consistency

**nb5.4 (Petersen coverage taxonomy).** Re-running the firm-effect Monte Carlo (200 reps, β=0.30, n_firms=60, n_years=30) gave OLS 0.450, cluster-by-firm 0.950, cluster-by-time 0.345 — qualitatively reproducing the charter's reference targets (right ≈ 0.92, wrong ≈ 0.40). The notebook's structure (build the truth → simulate panels under the three correlation worlds → tabulate coverage on a methods × worlds grid) is intact and the green-diagonal heat map at §7 is the right pedagogical artifact. PS5.4 T2 quotes the same qualitative table (0.71 OLS / 0.73 White / 0.94 right-cluster / 0.72 wrong-cluster) as illustrative values, and PS5.4 T3's self-diagnostic uses ratios (firm/white 1.07, time/white 2.86) that are internally consistent with the time-effect-dominates story. **PASS.**

**nb5.5 (BDM placebo law).** The notebook sets ρ=0.8, T=20, 50 states, ALPHA=0.05, N_REPS=600, and the §3 plan explicitly targets the "naive ≈ 0.35 vs clustered/collapsed/block-bootstrap ≈ 0.05" pattern named in `E-w5-ps5.5-solutions.md` (lines 21, 65, 162). PS5.5's illustrative-numbers table at line 163 uses exactly the same {naive 0.35, clustered 0.05, collapsed 0.06, block-bootstrap 0.05} values. The Monte Carlo SE check in PS5.5 P2(b) — (0.35−0.05)/√(0.35·0.65/600) ≈ 15.4 — is correct. **PASS.**

**nb6.5 macro-F1 / kappa cross-file consistency.** Ran the offline local-provider pipeline end-to-end with the seeded dataset and the notebook's exact split (rng=42, stratified 0.34 holdout). Result: confusion matrix as expected, macro-F1 = **0.861**, accuracy = **0.867**, Cohen's κ = **0.800** — within rounding of the charter's stated reference targets (macro-F1 ≈ 0.86, κ ≈ 0.80). ch65 / PS6.5 / assessment6 do *not* hard-code these specific numerics; each uses different illustrative figures (e.g., assessment6's worked κ example computes 0.474, and PS6.5 Task 4's by-hand kappa is 0.661). That is the correct division of labor: notebooks compute and assessment exhibits don't fight each other. **PASS.**

**ch55 / nb5.5 filename match.** ch55 line 107 references `notebooks/week-05/nb5.5-bdm-placebo-law.ipynb`; PS5.5 line 264 and `E-w5-ps5.5` line 256 likewise. File on disk: `/mnt/e/ccli/8weeks/notebooks/week-05/nb5.5-bdm-placebo-law.ipynb`. **PASS** — the earlier reviewer's fix stuck.

**AI-module env-gating story coherence.** ch65 §6.5.6 (lines 220–293) shows the env-only call pattern for both Anthropic and Azure; nb6.5 §10 (cells around line 815) implements both with `if os.environ.get(...)` gates; assessment6 §A.2 (lines 105–126) repeats the same env-only skeleton with the same `cache_control: ephemeral` rubric structure. All three documents tell one coherent story: the local provider runs offline, the real providers light up if the environment exports a key, and the same validation harness grades whichever was used. **PASS.**

## Dimension 4 — AI-module integrity

**No hard-coded secrets anywhere in W6.** A strict scan for AWS-style `AKIA...`, Anthropic `sk-...`, and `api_key="<base64-ish>"` patterns across `book/weeks/week-06/` and `notebooks/week-06/` returned zero matches. Every key is read from the environment: Anthropic via the SDK's default `Anthropic()` constructor (which reads `ANTHROPIC_API_KEY`), Azure via `os.environ["AZURE_OPENAI_KEY"]`. **PASS.**

**Offline-fallback discipline in nb6.5.** The provider switch defaults to `"local"`; every real-API branch is guarded by `if os.environ.get("ANTHROPIC_API_KEY"):` or `if os.environ.get("AZURE_OPENAI_KEY"):`, with explicit "requires an API key; skipped offline" prints in the else branch. The local provider is a deterministic seeded `LogisticRegression` + keyword backstop and is sufficient to drive the full confusion-matrix/F1/κ harness end to end with no network. **PASS.**

**Anthropic Messages API pattern.** ch65 lines 234–248, nb6.5 cell `5ddb8859` (lines 814–836), and assessment6 lines 117–126 all use the canonical form: `model="claude-opus-4-7"`, `max_tokens=16` (one-word output), `system` carrying the rubric with `cache_control: {"type": "ephemeral"}`, `messages` carrying the volatile filing text last, and the documented cache-verification step `resp.usage.cache_read_input_tokens > 0`. ch65 line 264 also correctly references `thinking={"type": "adaptive"}` for harder reasoning tasks. **PASS.**

**Causal / measurement-language discipline.** Ch 6.5 explicitly frames the LLM label as a *measurement* (§6.5.4, Question 1: "an LLM label is a measurement, not the truth"), with PS6.5 Task 1 making it the conceptual core in Week-1 language (latent quantity, instrument, systematic vs random error). PS6.4 carries the same discipline forward for the fair-lending setting — Tasks 1, 3, and 4 are entirely about not confusing a disparity with differential treatment, with the over-controlling trap (Mentor 4 / Bartlett et al.) and the omitted-score OVB (Week 2 / BHR) named at every step. Assessment6 B1 / B4 reinforce: validation establishes *measurement* validity, not causality; AI as a *data-generating instrument* needs the OOS table, AI as a *writing aid* does not, AI as a citation source needs independent verification. **PASS.**

## Items deliberately not touched

1. The retained `[CHECK]`s listed in Dimension 1 — all of them either tag a paper-internal magnitude the audit charter says must stay (Petersen simulation grid, BDM ~0.45, JT93 6/6 figure, LM headline %), or tag a live-API specifier that depends on GMU's deployment portal (Azure deployment name, `api_version`, WRDS data-clearance terms).
2. nb5.4 / nb5.5 / nb6.5 are shipped *unrun* (no cell outputs in the JSON). This is consistent with the book's discipline (`CONVENTIONS §5`: students regenerate from the seeded synthetic fallback). The numerical patterns reproduce when executed.
3. The Bound–Jaeger–Baker (1995) *JASA* 90(430):443–450 quarter-of-birth citation is not used in W5/W6 (it belongs to the Week-3 IV chapter), so no W5/W6 edit was needed.

## Confidence

High on Dimensions 2, 3, 4 (all independently re-verified by execution).
High on Dimension 1 (every bib on the charter's verified list cross-checked against the chapter file; the one removable `[CHECK]` confirmed by WebSearch and replaced with full citations).

---

*End of W5/W6 audit. The W5/W6 stack — five reader's guides per week, five problem sets per week, five notebooks per week, mentor session, pack, assessment, and a complete answer-key appendix — is publication-ready for the camp's empirical-finance curriculum. The single textual change is in `ch54-readers-guide-petersen-2009.md` §6.*
