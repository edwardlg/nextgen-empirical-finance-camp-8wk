`[ReviewerAgent -> Week 8 verdict]`

Reviewed all of Week 8: ch81–ch85, ps8.1–ps8.5, lab8, mentor8, assessment8; solutions E-w8-ps8.1..8.5; notebooks nb8.1–nb8.5 (read JSON, sanity-checked, not executed). Arithmetic verified in python3 (numpy).

## (a) Fixes APPLIED

1. **[Citations — PART 1] Added full bib for Cameron, Gelbach & Miller (2008) and (2011) in ch82.** Both were cited by name inline (and in ps8.2 + solutions) but had no bibliographic entry — only Gao&Sun, Oster, and BH had footnotes. Added `[^cgm2008]` (Bootstrap-Based Improvements for Inference with Clustered Errors, *REStat* 90(3):414–427) and `[^cgm2011]` (Robust Inference with Multiway Clustering, *JBES* 29(2):238–249), with inline markers placed at first mention. Footnote markers now pair 1:1 with definitions.
2. **[Citations] ch81 Simonsohn–Simmons–Nelson (2020) — added issue number 4(11)** to match the verified reference (was "4, 1208–1214").
3. **[Citations] Wilson et al. (2017) journal name "PLOS" → "PLoS"** in ch85 and lab8 reference lists, to match the verified bib exactly (was internally consistent but in the post-2012 all-caps form; aligned to the verified-list spelling).
4. **[Improvement] Tightened the surviving ch81 `[CHECK]`** (the Simonsohn permutation-test pinpoint cite) — reworded from a redundant page-range repetition into a precise "confirm the exact page where the joint permutation-inference procedure is stated" note, now that the full bib is confirmed.

## (b) Remaining [CHECK] / human items (all legitimate, left in place)

- **ch81 §8.1.3** — pinpoint page within Simonsohn et al. (2020) for the joint permutation-inference procedure. Not in the verified-reference set (only the page range 1208–1214 is verified), so kept per instructions.
- **lab8 (2 items)** — exact AEA `\documentclass`/`aea.bst` filenames, and the `book/capstones/` gallery stub. Genuine human/asset-dependent verifications; correctly flagged.
- The other `[CHECK]` occurrences (ch83, ps8.3, assessment8, mentor8) are **instructional references to the CONVENTIONS §6 honesty norm or to "confirm against the actual paper"** — not unresolved citations. Mentor8's Elnahas, Gao, Hossain & Kim (2024, *JFQA*) is fully cited; its `[CHECK]`s are "verify against the paper" prompts for the live session, appropriate to leave.

## (c) PASS / FAIL

- **Answer-key / arithmetic: PASS.** Verified in python3:
  - Oster δ: oster_delta(β0=−2.2, R0=0.08, β1=−1.4, R1=0.42) = **4.72** at default R_max=1.3·R1=0.546 (chapter says ~4.72 / ~4.7 ✓). At harshest R_max=1.0, δ=**1.03** — still ≥1, so the "robust case, δ≥1" claim holds across the whole sweep, matching nb8.2's sweep figure and ps8.2/solutions. Internally consistent.
  - BH m=8 family: largest k with p(k) ≤ (k/8)(0.05) is **k=2** → denial gap (0.004) + approval-rate gap (0.011) survive; rate-spread (0.030) does NOT (chapter, ps8.2, solutions, and nb8.2's `benjamini_hochberg` step-up function all agree). Bonferroni bar 0.00625 keeps only the primary. ✓
  - Spec-curve headline: ch81 / nb8.1 / ps8.1 / solutions all carry **ATT ≈ −1.89pp** (planted truth −1.80, recovered −1.89), 4×2×3×3×2 = **144** specs, median ≈ −1.8/−1.85, primary near 46th pct. Consistent. ✓
  - Note: ch82/nb8.2/ps8.2 use a **different illustrative ATT (−1.4pp)** for the robustness running example. This is INTENTIONAL and explicitly documented in ps8.2's "standing rule on numbers" and both solutions — two distinct stylized examples, not a drift error.
- **Citations: PASS** (after fixes). Every verified reference cited in Week 8 now carries full bib in-file; no fabricated vols; Gao&Sun (2019, *PNAS* 116(19):9293–9302) and Elnahas et al. (2024, *JFQA*) per the verified list.
- **No-secrets: PASS.** No hard-coded keys/tokens/passwords in lab8/nb8.x/ps8.x; keys read from `os.environ`; SEED constants only (seed=8, seed=42). `.gitignore`/no-licensed-data discipline reiterated in lab8.
- **Voice / no-overclaiming: PASS.** No emojis (full pictograph sweep clean). Causal-language discipline is strong and self-aware — ch83 explicitly teaches "we exploit plausibly exogenous variation," not "we prove discrimination"; verbs bounded by parallel-trends throughout; cast (Maya/Sam) consistent.
- **Consistency: PASS.** Chapter↔notebook headline numbers agree. The 8.1 (analytic-choice multiplicity / spec curve) vs 8.2 (inference validity / robustness battery) distinction is maintained explicitly and repeatedly — ch81 opening boundary, ch82 opening, nb8.2 cell 0, ps8.1, ps8.2. nb8.1 plants/recovers the spec-curve numbers; nb8.2's BH and Oster functions reproduce the chapter tables.

## (d) Length note
Week 8 prose totals ~59,000 words (5 chapters ~5.3–6.2k each, ps8.1–8.5 ~2.9–4.8k, lab8 6.2k, mentor8 3.5k, assessment8 3.7k) — well-proportioned and on-spec for a capstone week; no chapter is an outlier.
