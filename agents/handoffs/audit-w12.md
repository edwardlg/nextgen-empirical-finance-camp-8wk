# [AuditAgent → Weeks 1–2 verdict]

**Date:** 2026-05-30
**Scope audited:**
- `book/weeks/week-01/*.md` (ch11–ch15, lab1, mentor1, assessment1, ps1.1–ps1.5, README)
- `book/weeks/week-02/*.md` (ch21–ch25, lab2, mentor2, assessment2, ps2.1–ps2.5, README)
- `notebooks/week-01/nb1.1–1.5.ipynb`, `notebooks/week-02/nb2.1–2.5.ipynb` (read JSON; not executed)
- `book/appendices/E-solutions-manual/E-w1-ps1.{1..5}-solutions.md` and `E-w2-ps2.{1..5}-solutions.md`
- W1/W2 handoffs in `agents/handoffs/w1-*.md`, `w2-*.md`

---

## (a) Fixes applied (file · change · why)

1. **`book/weeks/week-02/ch24-heteroskedasticity-clustering-hac.md`** — Added the missing
   footnote definition `[^cgm2011]` at line ~427. The chapter referenced
   `[^cgm2011]` at line 525 (two-way clustering) but no `[^cgm2011]:` definition existed
   anywhere in the file, so the footnote would have rendered as a dangling reference.
   Verified citation via WebSearch (JBES Vol 29 No 2; DOI 10.1198/jbes.2010.07136) and
   inserted:
   > `[^cgm2011]: Cameron, A. C., Gelbach, J. B., & Miller, D. L. (2011). Robust Inference
   > With Multiway Clustering. *Journal of Business & Economic Statistics*, 29(2), 238–249.`
   This matches the page/volume already used in the PS 2.4 solutions key.

2. **`book/weeks/week-01/mentor1-what-is-a-finding.md`** (line 134) — Added the issue
   number to the Gao/Han/Li/Zhou (2018) JFE citation. Was `129, 394–414`; now
   `129(2), 394–414`. Verified against the ScienceDirect record and EconPapers
   (`v129y2018i2p394-414`). Pure precision; no substantive change.

No other in-scope file required edits.

---

## (b) Remaining genuine [CHECK] / human-decision items

The Weeks-1–2 corpus contains **zero `[CHECK]` markers** in the prose or notebooks.
All open `[CHECK]` items in the project (per `book/TOC.md` and the running ledger) are
in Weeks 4–6 (Russell-RD critiques; HMDA snapshot vintages; Petersen § survey counts;
KPSS/HP/LM/BMSW citations; etc.) and are out of this audit's scope.

Two human-verify items surfaced during the audit but were **not fixed unilaterally** —
the author (Prof. Gao) is the ground truth:

- **`book/weeks/week-02/mentor2-standard-error-ballgame.md` line 130–131.** The book
  cites *Gao, Han, Kim & Pan (2024), "Common Ownership Along the Supply Chain and
  Management Disclosure," Journal of Corporate Finance.* WebFetch on Lei Gao's CV/
  research page lists a closely related paper *"Common Ownership along the Supply Chain
  and Corporate Earnings Management"* (3rd-round under review per the CV page), and a
  WebSearch surfaces a published JCF 84, 102520 (2024) titled *"Overlapping institutional
  ownership along the supply chain and earnings management of supplier firms"* by the
  same author quartet. The cited *Management Disclosure* version may be a different
  working paper; either way, the issue/volume/pages are not given in the book, so a human
  with author knowledge should confirm the exact title and venue before publication.
  This is **author-disambiguation, not a math/correctness issue.**

- **Various "illustrative" numbers labeled as such in the prose.** Ch 2.4 §6 has a
  hand-typed Maya panel SE table (lines 565–570) that is explicitly tagged "illustrative,
  not the output of a committed script; the live numbers are in nb2.4" — and nb2.4 does
  reproduce comparable magnitudes (β̂ = 0.30, classical t = 5.70 → firm-clustered t =
  2.19). The two are mutually consistent. No change needed; this is legitimate "in the
  spirit of Petersen's tables" pedagogy with the live-number anchor named.

The nb1.1 "Your Turn" cell contains a `print("TODO: change the regime probabilities …")`
string. This is **intentional student-exercise scaffolding**, not unfinished agent work
(confirmed in `w1-nb11.md`). No change.

---

## (c) PASS / FAIL by dimension

| Dimension | Verdict | Notes |
|---|---|---|
| **1. [CHECK]-flag resolution** | **PASS** | Zero `[CHECK]` markers in scope. One latent bibliographic gap (the dangling `[^cgm2011]` footnote in ch24) was fixed in place with a WebSearch-verified citation. Mentor1's JFE issue number tightened. |
| **2. Math / answer-key correctness** | **PASS** | Every numerical claim in the ten solution keys was re-derived in `python3` (numpy / scipy / statsmodels / fractions). LIE/LTV (ch11/nb1.1/PS1.1) lands E[X]=0.300, within=1.710, across=0.400, total=2.110 exactly. PS1.1 P4 (Sam's three-value table) gives within=3.987, across=2.3814, Var(X)=6.3684. PS1.2 P6(b) min-var weight w★=0.0588, sd=0.0485. PS1.3 Bessel/MSE derivations sound. PS1.4 Chebyshev N≥4500, P3(b) N=348, P5 NW L≈4.17 (rounded to L=4). PS1.5 t=1.6933, p_one≈0.0458 (matches "t=1.69, p≈0.046"); B(20,0.05) ⇒ 1-0.95²⁰=0.642; N=1764 ⇒ 7 years; k=14 for >50% chance of ≥1 false positive. PS2.1 by-hand OLS β̂=(-0.5, 1.6), det=20, leverages (3/5, 3/10, 1/5, 3/10, 3/5) summing to 2, R²=64/65≈0.985, κ(0.98)=99. PS2.2 SE 0.003802 vs. 0.006001. PS2.3 Maya long β̂_x=2, short=37/26≈1.423, δ=15/26≈0.577, OVB identity exact. PS2.4 classical SE 0.462, HC0 SE 0.548, Moulton bracket 8.96 ⇒ honest t≈2.0. PS2.5 OVB and λ algebra correct. Assessment 1: |μ| < 3699 for shrinkage win; MC SE 0.00154 at R=20k. Assessment 2: Moulton factor 50.95, SE inflation √50.95≈7.14. **No genuine math errors found.** |
| **3. Cross-file consistency** | **PASS** | ch11 ↔ nb1.1 ↔ PS1.1: Sam's 0.300/1.710/0.400/2.110 reproduce to ≥3 decimals on the chapter's two-regime table (1.7095 rounds to 1.710; 0.4005 to 0.400); PS1.1 P4 deliberately uses a *different* three-value table (per writer-handoff `w1-ps11.md`), so no contradiction. ch15 ↔ nb1.5 ↔ PS1.5: Sam's t=1.69 (precisely 1.6933) with se=0.0473, critical 1.65, p_one≈0.046, recovered identically wherever invoked. ch23 ↔ nb2.3: Leah's β̂₁=1.838 (= 68/37 exact) matches; PS2.3 reuses the Maya literacy/income dataset (β̂₁=2 exact) intentionally, not the Leah dataset — call-out in the PS makes this explicit. ch24 ↔ nb2.4 ↔ PS2.4: the chapter's 5.70 → 2.19 t-stat collapse is *the* nb2.4 simulated panel headline (β̂=0.30, 120-firm × 20-year, seed=42); the chapter's §6 illustrative Maya table (classical t=5.0 → two-way t=1.7) is *typed-by-hand* with explicit "illustrative" disclosure and points the reader at nb2.4 for live numbers — consistent. PS2.4 P6 uses a *fresh* illustrative panel table (classical t=5.0, firm-clustered t=2.0, two-way t=1.96), distinct from but in the same spirit as the chapter's. Assessment 2 keeps the same vocabulary. **No contradictions detected.** |
| **4. Voice / style** | **PASS** | Zero real Unicode-emoji codepoints in any in-scope `.md` or `.ipynb` (programmatic scan over U+1F300–1F9FF and U+2700–27BF, with math arrows/checkmarks `→ ✓ ✗` exempted because they're legitimate notation). Zero hits on "in today's fast-paced world" and "controls for endogeneity" (the latter is only used in PS2.5 and ch25 as the *flagged anti-pattern* — "I controlled for it" vs. "I controlled for it well" — exactly per CONVENTIONS). Cast (Maya, Devon, Priya, Sam, Leah) used consistently per CONVENTIONS §2: Maya runs the fair-lending / OLS-by-hand thread; Devon does heavy-tails / OVB; Priya does insurance/variance/PSD; Sam does momentum/t-test/Fama–MacBeth; Leah does R&D / FWL. Reveal-the-trick structure observed in every chapter's §2. Ch 2.4 illustrative tables and PS2.4 P6 table both label each row by SE flavor and disclose the clustering choice in the surrounding note (Appendix D §D2 conformant). The chapter's stipulated lack of stars on illustrative tables is consistent with D2's "stars only where they carry information." |

---

## (d) Chapter ↔ notebook contradictions found

**None.** Every load-bearing numeric claim that recurs across the chapter–notebook–PS triad
was reproduced to the digits shown:

- ch11 §5 `E[X]=0.300` ↔ nb1.1 cell 6 `0.300` ↔ PS1.1 P4 uses a different (declared) table.
- ch11 §7 `within=1.710, across=0.400, total=2.110` ↔ nb1.1 cells 6/10/14 confirm to ±MC tol.
- ch15 §4 `se=0.0473, t=1.69, p≈0.046, critical=1.65` ↔ nb1.5 cells 4/8 confirm; PS1.5 P1 and P3 reuse the same numbers.
- ch23 §2 `β̂₁=1.838, β̂₂=3.676, β̂₀=1.919, short β̂=4.65` ↔ nb2.3 cell 6 asserts to atol=1e-6; PS2.3 uses a separate (declared) Maya literacy dataset.
- ch24 §6 narrative `5.70 → 2.19 firm-clustered` ↔ nb2.4 cell 15 prose `classical ≈ 5.7 … cluster ≈ 2.2` — agree to one decimal.
- Petersen 2009 RFS 22(1):435–480; White 1980 *Econometrica* 48(4):817–838; Newey–West 1987 *Econometrica* 55(3):703–708; Frisch–Waugh 1933 *Econometrica* 1(4):387–401; Lovell 1963 *JASA* 58(304):993–1010; CGM 2008 *RES* 90(3):414–427; CGM 2011 *JBES* 29(2):238–249; Fama–MacBeth 1973 *JPE* 81(3):607–636; Gao, Han, Li & Zhou 2018 *JFE* 129(2):394–414 — all verified bibliographically. The last one was tightened to add the issue number; the rest were already correct.

---

## One-line bottom line

Weeks 1–2 are publication-ready as far as `[CHECK]` flags, math correctness, cross-file
numerical consistency, and voice/style are concerned. One latent bibliographic gap and one
missing issue number were repaired in place. The only outstanding human-verify item — the
exact title/venue of the Gao/Han/Kim/Pan 2024 JCF working paper cited in mentor2 — is an
author-disambiguation question, not a math or pedagogy issue, and belongs to Prof. Gao to
settle from his own CV.
