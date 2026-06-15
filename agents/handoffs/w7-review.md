`[ReviewerAgent -> Week 7 verdict]`

Reviewed all of Week 7: chapters ch71–ch75, problem sets ps7.1–ps7.5, lab7, mentor7, assessment7;
model deliverables E-w7-ps7.1..7.5; notebooks nb7.1–nb7.5 (read JSON, did not execute); plus the
w7-* handoffs. Arithmetic re-run in python3.

## (a) Fixes APPLIED

1. **Emoji removal (CONVENTIONS §1 "No emojis") — 4 fixes across 2 notebooks.** The prose (.md)
   files were clean, but two notebook markdown cells carried pictographic emojis:
   - `nb7.5-first-look-regressions.ipynb`: removed `⛔` from the "PAP-FILED GATE" header (cell
     367b3f97), `✋` from "The confirmatory analysis is now COMPLETE" header, and `⚠️` from the
     "EXPLORATORY ZONE" callout (both cell b8a54e58).
   - `nb7.1-idea-to-spec-template.ipynb`: removed `➡️` from the "Replace this line with your own
     puzzle" prompt (cell 513ad225).
   - Both notebooks re-validated as well-formed nbformat-4 JSON after the edits (21 and 23 cells).

   These were the only true CONVENTIONS violations found in the entire week; the remaining non-ASCII
   characters are legitimate typographic (— – · §) and math (→ ↔ ∈ ≥ × ⋈) symbols.

## (b) Remaining [CHECK] / human items (all legitimate, none are citation gaps)

- ch72 §7.2.7 — PatentsView current API base-URL/auth `[CHECK]` (deliberate; nb7.2 holds verified call).
- ch74 §7.4.1 — CCM `LINKTYPE` recommended code set `[CHECK]` against WRDS snapshot vintage.
- lab7 — environment-lock-file cross-OS solve `[CHECK]`; HMDA Data Browser snapshot-date `[CHECK]`.
- mentor7 / ps7.1 — instructional "verify-before-asserting" `[CHECK]` tags on Prof. Gao's own papers
  and on student AI-leash citation verification. Intentional pedagogy, not unverified citations.

  No fabricated citations and no unresolved citation placeholders. Recommend leaving all as-is.

## (c) PASS / FAIL

- **no-secrets: PASS.** Scanned ch72, lab7, nb7.2, ps7.2, E-w7-ps7.2-solutions, assessment7, and all
  remaining week-07 + solution files. Every key uses `os.environ[...]` / a `require_env(...)` helper /
  `export VAR=` placeholders. nb7.2's source functions (FRED, EDGAR, HMDA, PatentsView, yfinance,
  WRDS) all gate on env vars and fail loudly offline; FRED key stays in PARAMS (never logged),
  PatentsView in a header, SEC `User-Agent` correctly treated as identity not secret. The single
  string literal `api_key=abc123SECRET` (nb7.2 cell b4080875) is the explicitly-labeled anti-pattern
  input fed to the `_redact()` scrubber demo ("NOT a real key — illustrative anti-pattern, then
  scrubbed") — the one acceptable case. No real leak anywhere.
- **citations: PASS.** Olken (2015) JEP 29(3):61–80 — full bib in ch73, ps7.3, lab7, assessment.
  Gao, He & Wu (2023) JFQA — full bib in mentor7. Gao & Sun (2019) PNAS 116(19):9293–9302 — full bib
  in ch75 footnote and referenced in ps7.3 solution. Rainbow of Credits kept as (AEA 2025; target
  *Journal of Finance*) in mentor7 — no fabricated volume. No [CHECK] needed on any of these.
- **arithmetic: PASS.** Re-ran in python3. (i) BH worked example in ch73 §7.3.2, nb7.3 (scrambled
  input order, with internal assert), E-w7-ps7.3-solutions, and assessment7 B4 all give k=2,
  discoveries {0.008, 0.012}; Bonferroni bar 0.05/5=0.01 admits only {0.008}. Correct everywhere.
  (ii) Family-wise figure 1−0.95^20 ≈ 0.64 correct. (iii) nb7.3 power/MDE: residual SD √(.12·.88)
  =0.325; one-sided power at N=8000 =0.81; N for 80% power =7773 (so 8000 adequate, matches
  narrative); two-sided N (9868) > one-sided; MDE at N=8000 =1.97pp ≤ 2pp target; all internal
  asserts (size==α, analytic≈MC, MDE↔power inversion, BH selection) pass.
- **design-consistency: PASS.** The two Maya HMDA designs are deliberately distinct and kept apart
  consistently: (1) PAP/conditional-disparity design — loan-level `denied ~ minority`, lender+tract
  FE, clustered by LENDER, OLS-on-observables reported as a bound (ch73 §7.3.4, ps7.3 solution, nb7.3
  emitter); (2) policy DiD — county-year minority–white gap, staggered state fair-lending exam
  programs, county+year FE, clustered by STATE, Callaway–Sant'Anna (ch75 §6, nb7.5 with planted ATT
  −1.80pp). No file conflates them. The "frozen until PAP filed" discipline is coherent across ch73
  (line 101), ch75 Your Turn, nb7.5's PAP_FILED gate, and the ps7.3/ps7.5/assessment checklists.
  Priya's climate-insurance DiD threads ch71 (worked example B), ps7.1, and is the labeled exemplar
  in E-w7-ps7.5-solutions (footer explicitly notes Priya-memo vs. Maya-chapter as two designs — a
  deliberately-different, labeled exemplar, allowed). All specs in CONVENTIONS §4 seven-slot form.
- **voice: PASS** (after the 4 emoji fixes above). No marketing voice; cast (Maya/Devon/Priya/Sam/
  Leah) stable and on-interest; "controls for endogeneity" / "this controls for endogeneity" appears
  only as explicitly-flagged anti-patterns (ch75 Draft 1, ps7.1, assessment exemplar, ps7.3 solution
  deduction note); no fabricated empirical RESULT magnitudes — the only planted number (−1.80pp) is
  repeatedly labeled "the truth the synthetic generator was built around," never an estimate.

## (d) Length note

Substantial and consistent with prior weeks: five chapters ~30–43 KB each, five problem sets ~16–22 KB,
lab7 ~36 KB, mentor7 ~19 KB, assessment7 ~33 KB, five notebooks ~30–46 KB, five model deliverables —
appropriately scoped for the design-half capstone week; no padding, no thin files.

**Overall: PASS on all five axes** (no-secrets, citations, arithmetic, design-consistency, voice),
with 4 emoji fixes applied and 8 legitimate [CHECK]/human items left in place.
