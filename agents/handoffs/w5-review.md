# [ReviewerAgent -> Week 5 verdict]

Reviewed all of Week 5: five Reader's Guides (ch51–ch55), reading-guide-pack-5, ps5.1–ps5.5,
mentor5, assessment5; five solutions (E-w5-ps5.1–5.5); the five notebooks (nb5.1–nb5.5, JSON
sanity-checked, not executed); and the w5-* handoffs. Date: 2026-05-28.

## (a) Fixes APPLIED

**PART 1 — Citation [CHECK] tags resolved against the verified reference list (8 tags dropped):**
- **ch51 (FF92):** (i) Resolved the FF92 sample window — "1963 to 1990 [CHECK exact endpoints]" →
  "roughly 1963 to 1990 (Fama & French 1992, *JF*, 47(2), 427–465)" (line 65). (ii) Resolved the
  Harvey, Liu & Zhu 2016 [CHECK] — converted the inline `(HLZ 2016 [CHECK])` to a footnote anchor
  `[^hlz]` and added the full bib (*RFS*, 29(1), 5–68) at file end.
- **ch52 (FF93):** Converted the `[^period]` footnote from a [CHECK] caveat to a confirmed note —
  sample July 1963–Dec 1991, 342 months, cited to FF93 *JFE* 33(1), 3–56. (Header bib and the GRS
  footnote `[^grs]` Gibbons–Ross–Shanken 1989 were already correct.)
- **ch53 (JT93):** Dropped 3 bibliographic [CHECK]s now covered by the verified refs — the `[^jt93]`
  footnote page-range caveat (65–91 confirmed); the exchange-filter caveat (NYSE/AMEX confirmed,
  line 49); the sample-window caveat (1965–1989 confirmed, line 51). De Bondt–Thaler `[^dbt]` 1985 and
  Carhart `[^carhart]` 1997 footnotes already present and correct.

**PART 2 — Filename mismatch fixed:**
- **ch55** line 107: `nb5.5-bdm-placebo-law-simulation.ipynb` → `nb5.5-bdm-placebo-law.ipynb` (the real
  file). ps5.5 line 264 already used the correct name; no notebook self-references the wrong name.

**PART 3 + extra concrete improvement:**
- **E-w5-ps5.3-solutions** (Problem 6a): removed a now-stale instruction telling students to "carry the
  `[CHECK]` tags from Ch 5.3 on the exact exchange filter and sample dates" — those tags no longer exist
  (resolved in PART 1). Rewrote to state NYSE/AMEX, 1965–1989 per JT93 while preserving the
  order-of-magnitude-only and licensed-data (pin CRSP snapshot) discipline.

## (b) Remaining [CHECK] / human items (all correctly retained — NOT in the verified ref list)

These are precise table coefficients / sample micro-details / attribution dates that must stay
qualitative per the instruction, plus intentional pedagogical scaffolding:
- ch53: exact skip convention in JT93 vs later work (l.43); exact 6/6 spread decimal (l.63).
- ch52: l.101 is an *instruction to students* to tag carried-forward figures, not a live tag.
- ch54 (Petersen): survey paper counts/journals (l.21); sim baseline grid N/T/S (l.51); which real
  datasets (l.53); precise estimated/true SE ratios (l.74); two-way-clustering attribution dates
  CGM 2011 / Thompson 2011 (l.105). All precise-figure or attribution items to confirm at press.
- ch55 (BDM): exact CPS years & wage variable (l.45); exact ~0.45 headline rejection rate (l.55).
- ps5.5 l.33: instruction about tagging the often-cited BDM number.
- mentor5 l.138/144/169: deliberate template scaffolding for Prof. Gao's in-motion muni-bond paper;
  the [CHECK] is modeled behavior the students are being taught, not a resolvable citation.

## (c) PASS / FAIL

- **Answer-key correctness: PASS.** Independently re-derived in python3 every computed result:
  ps5.1 (size 0.84 / value 1.23 %/mo, ann 10.1 / 14.8; naive beta spread +0.32 → within-size
  ±0.01/±0.01/+0.02; six FM t-stats 1.3/-4.3/4.9/-0.2/-3.9/4.4; inverted slope sd 1.635; 1−0.95^20=0.6415);
  ps5.2 (SMB 0.25, HML 0.80; CAPM α 0.35, FF3 α 0.04, contrib 0.36; 1−0.95^25=0.7226);
  ps5.3 (t_classical 12.05, t_HAC 8.20, ratio 1.47; net(50)=58, net(100)=28; break-even 146.7 bps,
  166.7 with no short premium, Δ20; 0.99×12=11.9);
  ps5.4 (self-diagnostic ratios firm/white 1.07, time/white 2.86);
  ps5.5 (AR(1): T=3 Var 0.8311, ratio 2.49; long-run (1+ρ)/(1−ρ)=9, √9=3; T_eff=20/9≈2.2;
  MC SE 0.0195, z≈15.4). **All exact.** assessment5 is a qualitative rubric (no arithmetic to check).
- **Anatomy-compliance: PASS.** All five guides carry the fixed 7-part anatomy in order
  (research question · identification · data · table-by-table reading order · what's clever ·
  what's vulnerable · three replications), matching reading-guide-pack-5's template. ch52/ch55 add a
  trailing pointer/referee-questions coda after box 7 — additive, not a deviation.
- **Citations: PASS.** All resolvable bib tags now carry full bibliographic info from the verified
  list; GRS, De Bondt–Thaler, Carhart, HLZ, Petersen, FF92, FF93, JT93, BDM all present and correct.
- **No-fabricated-stats: PASS.** No precise paper coefficients presented as fact; every concrete
  number in the PS/solutions is explicitly labeled illustrative; qualitative claims (≈1%/month
  momentum, positive SMB/HML, OLS undercoverage, BDM over-rejection) kept order-of-magnitude.
- **Voice: PASS.** No emojis (the only flags were → arrows and one functional ✓ verify-mark in
  E-w5-ps5.2 l.178, consistent with the established weeks-1–4 solutions house style — left as-is).
  Student cast used per domain (Sam: FF/JT markets; Priya: BDM climate/insurance; Maya/Devon/Priya in
  ps5.4 Petersen). Wooldridge-level prose, full paragraphs, no marketing voice.

## (d) Length note

Five guides run 3,213–4,101 words each (~17.9k total) — squarely in the intended four-page Reader's
Guide band; solutions and assessment are appropriately scoped. No trimming needed.
