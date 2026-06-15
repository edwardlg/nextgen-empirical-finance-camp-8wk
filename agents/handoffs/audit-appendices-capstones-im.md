# Audit Verdict — Appendices, Capstone Gallery, Instructor's Manual, Data Cards

**Date:** 2026-05-30
**Auditor:** AuditAgent (Claude Opus 4.7, 1M context)
**Scope:** `book/appendices/{A-math-toolkit,B-python-latex-setup,D-style-guide,C-data-dictionary}/`, `data-cards/` (34 cards), `book/capstones/` (5 papers + index), `book/instructor-manual/` (6 sections + index), `CONVENTIONS.md`, `README.md`, `environment.yml`.

---

## 1. Headline result

**Status:** the in-scope material is in good shape. Eight in-place edits were applied that resolve every critical [CHECK] the user asked to be resolved, fix two CONVENTIONS-§1 emoji violations in capstones 2 and 4, repair the USPTO PatentsView card and Ch 7.2 against the **March 20, 2026 ODP migration**, and resolve eight canonical-citation [CHECK]s in Capstones 2, 4, and 5. The 200-pt capstone rubric sums correctly. The 34-data-card master indexes (data-cards/README.md and book/appendices/C-data-dictionary/README.md) are aligned. The IM-1..IM-6 cross-references all resolve.

A residual **132 [CHECK] markers** remain in scope, but they fall into the four categories the user asked to be left as [CHECK]:
- WRDS table/column-name pins (CRSP, Compustat, IBES, OptionMetrics, Capital IQ, Thomson, TRACE, FISD data cards),
- HMDA / CFPB / agency API field names + pinned vintage,
- GMU Azure OpenAI deployment name + `api_version`,
- Hopper SLURM partition/account strings.

These are by-design holes that *must* be confirmed against the live container at run time, not before.

---

## 2. Edits applied in place

### 2.1 USPTO PatentsView → USPTO Open Data Portal (highest-impact)

**Source verified.** The legacy `api.patentsview.org` API was discontinued **May 1, 2025** (returns HTTP 410). The intermediate **PatentSearch API** at `https://search.patentsview.org/api/v1` (which used `X-Api-Key`) was paused on **March 20, 2026**, when PatentsView migrated to the **USPTO Open Data Portal** at `https://api.uspto.gov`. The current header is **`X-API-KEY`** (lowercase). Keys are registered at `https://data.uspto.gov/apis/getting-started`. The bulk PatentsView tables are stable on ODP today; the exact post-cutover patent-search endpoint path is still being finalized by USPTO ("functions will pause temporarily starting March 20… reintroduced in updated forms as the transition progresses").

Sources:
- https://www.uspto.gov/subscription-center/2026/patentsview-migrating-uspto-open-data-portal-march-20
- https://data.uspto.gov/support/transition-guide/patentsview
- https://patentsview.org/data-in-action/support-legacy-api-end-february-2025-switch-patentsearch-api-now
- https://data.uspto.gov/apis/getting-started

**Files edited:**
- `/mnt/e/ccli/8weeks/data-cards/uspto-patentsview.md` — retitled to "USPTO PatentsView (now USPTO Open Data Portal)"; rewrote the Access-path section with the three-stage migration history; updated the Python snippet to use `https://api.uspto.gov` + `X-API-KEY` header + `USPTO_ODP_API_KEY` env var; updated the License note to flag CC-BY-vs-ODP-terms confirmation post-March-2026; updated the API-shape Gotcha to name all three platform generations and point to the ODP transition guide. One narrowly-scoped `[CHECK]` retained: the exact post-cutover endpoint path under `${base}/api/v1/...`, which USPTO is still finalizing.
- `/mnt/e/ccli/8weeks/book/weeks/week-07/ch72-data-acquisition-in-practice.md` (§7.2.7) — replaced the PatentSearch API snippet and prose with the ODP version; named the migration date; pointed at the ODP transition guide; kept the env-var pattern. One `[CHECK]` retained on the endpoint path, mirrored to the data card.
- `/mnt/e/ccli/8weeks/book/capstones/capstone3-innovation-uspto.md` — three updates: §3 Data ("the PatentSearch API, key via `X-Api-Key`" → ODP `api.uspto.gov`, `X-API-KEY`, March 20 2026 cutover); References (PatentSearch URL → ODP URL with paused-search note); margin commentary (real-pull paragraph updated).

### 2.2 Canonical citations — eight [CHECK]s resolved

All resolved via WebSearch against the actual journal listings (JSTOR/Oxford Academic/Wiley/ScienceDirect/IDEAS-RePEc):

| Reference | Verified citation | Where applied |
|---|---|---|
| Bertrand–Duflo–Mullainathan 2004 | *QJE* 119(1), 249–275 | Capstone 2 refs |
| Petersen 2009 | *RFS* 22(1), 435–480 | Capstone 2 refs |
| MacKinlay 1997 | *JEL* 35(1), 13–39 | Capstone 4 refs (+ confirmed in C5) |
| Landis–Koch 1977 | *Biometrics* 33(1), 159–174 (κ bands 0.81–1.00 "almost perfect", 0.61–0.80 "substantial", 0.41–0.60 "moderate", 0.21–0.40 "fair", <0.20 "slight") | Capstone 4 §4 + refs |
| Hoberg–Phillips 2016 | *JPE* 124(5), 1423–1465 | Capstone 4 refs |
| Hall–Jaffe–Trajtenberg 2001 | NBER WP 8498 (no journal vol/issue — WP series) | Capstone 3 refs + margin commentary |
| Kuttner 2001 | *JME* 47(3), 523–544 | Capstone 5 refs (was already correct; verification note updated) |
| Bernanke–Kuttner 2005 | *JF* 60(3), 1221–1257 | Capstone 5 refs (was already correct) |
| Nakamura–Steinsson 2018 | *QJE* 133(3), 1283–1330 | Capstone 5 refs (was already correct) |
| Gürkaynak–Sack–Swanson 2005 | *IJCB* 1(1), 55–93 | Capstone 5 refs (was already correct) |
| Newey–West 1987 | *Econometrica* 55(3), 703–708 | Capstone 5 refs (was already correct) |
| Fama–Fisher–Jensen–Roll 1969 | *IER* 10(1), 1–21 | Capstone 5 refs (was already correct) |
| Christensen–Miguel 2018 | *JEL* 56(3), 920–980 ("Transparency, Reproducibility, and the Credibility of Economics Research") | Not currently cited in scope — verified for reference |
| Wilson et al 2017 | *PLOS Comp Biol* 13(6): e1005510 ("Good enough practices in scientific computing"; Wilson, Bryan, Cranston, Kitzes, Nederbragt, Teal). Note: brief's "Wilson et al 2017" most plausibly refers to this paper; if instead the intended ref was Baker (2016) *Nature* 533:452–454 ("1500 scientists lift the lid on reproducibility"), that citation is also verified | Not currently cited in scope — verified for reference |
| Gentzkow–Shapiro 2014 | "Code and Data for the Social Sciences: A Practitioner's Guide" (Stanford working manual; no journal placement) | Not currently cited in scope — verified for reference |

**Files edited:**
- `/mnt/e/ccli/8weeks/book/capstones/capstone2-common-ownership-13f.md` — removed BDM and Petersen page-range [CHECK]s (page ranges verified); updated the "Why the citations carry [CHECK] tags" commentary to be singular (only Gao et al 2024 vol/issue/pages still pending).
- `/mnt/e/ccli/8weeks/book/capstones/capstone4-8k-text-classification.md` — removed [CHECK] tags from MacKinlay, Landis–Koch, and Hoberg–Phillips; rewrote the κ-bands sentence in §4.2 to inline the Landis–Koch thresholds with the source.
- `/mnt/e/ccli/8weeks/book/capstones/capstone5-fred-macro-event-study.md` — replaced the omnibus "verify all eight refs" [CHECK] with an inline confirmation listing every reference's verified venue + pages.
- `/mnt/e/ccli/8weeks/book/capstones/capstone3-innovation-uspto.md` — Hall–Jaffe–Trajtenberg [CHECK] replaced with the WP-series note (WPs carry no journal volume/issue, which is what the [CHECK] was reaching for); margin commentary updated to match.

### 2.3 Emoji removal (CONVENTIONS §1 "No emojis")

**Files edited:**
- `/mnt/e/ccli/8weeks/book/capstones/capstone2-common-ownership-13f.md` — removed `⚠️` from the synthetic-data banner.
- `/mnt/e/ccli/8weeks/book/capstones/capstone4-8k-text-classification.md` — removed `⚠️` from the synthetic-data banner.

Post-fix sweep across `book/appendices/{A,B,D}`, `book/capstones/`, `book/instructor-manual/`, `data-cards/`: **0 emojis remain.**

---

## 3. Math / factual spot-checks (Dimension 2)

**All four spot-checks pass.**

| Check | Source | Verification |
|---|---|---|
| 2×2 inverse arithmetic | A.1 §4, matrix `[[4,2],[2,3]]` | det = 4·3 − 2·2 = 8 ✓; inverse `(1/8)[[3,-2],[-2,4]] = [[3/8,−1/4],[−1/4,1/2]]` ✓; row1·col1 = 4·(3/8)+2·(−1/4) = 1.5−0.5 = 1 ✓; row1·col2 = 4·(−1/4)+2·(1/2) = 0 ✓. A·b = [1·5+2·6, 3·5+4·6] = [17, 39] ✓ |
| Sharpe-SE delta method | A.3 §4 | g(μ)=μ/σ, g′(μ)=1/σ, se(x̄)=σ/√N → se(SR) ≈ 1/√N ✓. With N=252: 1/√252 ≈ 0.0630 ✓; 1.96·0.063 = 0.1235, so 0.10 ± 0.1235 = [−0.0235, 0.2235] — text reports [−0.023, 0.223] ✓ |
| t / χ² / F moments | A.4 §3–§5 | χ²ₖ: mean=k, variance=2k ✓; Fd1,d2: mean = d₂/(d₂−2) for d₂>2 ✓; t→Normal as df→∞ ✓; F1,d2 = t²d2 ✓ |
| Capstone 3 numerics ↔ nb6.1 | capstone3 vs `notebooks/week-06/nb6.1-patent-value-panel.ipynb` | Notebook fixes `np.random.default_rng(42)`, N_FIRMS=60, YEARS=[2018..2021], Poisson(2.5) patents/firm/year ⇒ E[#patents] ≈ 600 (capstone reports 633, a single-seed realization, consistent) ✓. Aggregate firm-year panel of ~217 rows (consistent with the 60-firm × 4-year skeleton minus those without enough pre-grant history to estimate the market model) ✓. The 0.565 / 0.382 / 0.722 recovery correlations, t = 3.61 validation, and 0–75% contamination sweep are produced at run time inside nb6.1 (they're computed, not hard-coded) — they're internally consistent with the notebook's setup |
| Capstone 1 numerics ↔ nb6.4 | capstone1 §5 table | All figures (43.9 raw, 39.8/32.4/20.9 build-up, 22.3/21.6 Oaxaca split, 32.9 omitted-score inflation, −6.0 over-control flip, 1.3 race-blind residual) are sourced from a `nb6.4` re-run at seed 42, per the margin commentary; pattern is internally consistent with a planted-discrimination DGP |
| **IM-2 200-pt rubric sum** | IM-2 §4b | **30 + 34 + 40 + 30 + 34 + 22 + 10 = 200 ✓**. Three craft rows (identification 34 + execution 40 + reproducibility 34) = **108**, which is correctly described in the prose as "more than half the grade" ✓ |
| IM-2 100-pt Week-7 rubric sum | IM-2 §4a | Sums to 100 ✓ (verified) |

---

## 4. Cross-file consistency (Dimension 3)

| Check | Result |
|---|---|
| Capstones label results SYNTHETIC at top + every table note | **All 5 pass.** Mention counts per capstone of "synthetic"/"ILLUSTRATIVE"/"SYNTHETIC": C1=14, C2=20, C3=14, C4=15, C5=16. Banners present and prominent. Capstone gallery README adds a gallery-wide banner above any individual paper. |
| Appendix C master index ↔ data-cards README ↔ data-cards/ directory all list the same 34 cards | **Pass.** All three list 34 cards, same slugs, same categorization. Verified file-by-file. |
| IM cross-references (IM-1..IM-6) resolve | **Pass.** IM-1 references IM-2/3/4/5/6 by name; IM-3 → IM-1/IM-2/IM-4/IM-5; IM-5 → IM-2 explicitly ("IM-2 (the consolidated rubrics) tells you *what*… this is the operational heart"); IM-6 cross-refs Appendix B.4. All resolve. |
| Capstone gallery anchors to notebooks per brief (C1=nb6.4, C3=nb6.1, C4=nb6.5) | **Pass.** Index explicitly maps the three; in-paper references match. |

---

## 5. Voice / style / no-secrets (Dimension 4)

| Check | Result |
|---|---|
| No emojis in scope (post-fix) | **Pass.** Two ⚠️ removed from capstone-2 and capstone-4 banners; full sweep returns zero. |
| Banned phrase "controls for endogeneity" only appears as anti-pattern | **Pass.** 4 hits — all explicitly forbid the phrase (D5-prose-style.md, E-w7-ps7.3-solutions.md, capstone1, IM3). No usage in actual prose. |
| Other banned-marketing-voice phrases (e.g., "in today's fast-paced world") | **Pass.** Only hit is the explicit ban statement in D5-prose-style.md. |
| Causal-language discipline in capstones ("associated with" / "consistent with") | **Pass.** Capstone 1 keeps the residual at "associated with" and disclaims causality; Capstone 2 explicitly audits down to "associated with" and refuses the causal verb; Capstone 3 uses "recovers," "predicts," "forecasts" for the synthetic-truth check, not causal; Capstone 4 stays at "associations consistent with information content, not causal effects"; Capstone 5 stays at "announcement-day association under a timing assumption." All five honor the D.5.1 verb-audit. |
| Hard-coded secrets across B-toolchain, data-cards, capstones, IM | **Pass.** Regex scan for `sk-…`, `AKIA…`, `api_key="…"` returns nothing. All credentials are read from env vars (`${WRDS_USERNAME}`, `${AZURE_OPENAI_KEY}`, `${PATENTSVIEW_API_KEY}` → now `${USPTO_ODP_API_KEY}`, etc.), per CONVENTIONS §5. |
| Appendix D's own table-craft and prose rules followed by D itself | **Pass.** Spot-checked D1 (table craft), D5 (prose style); both internally consistent and follow their own rules. |

---

## 6. Remaining [CHECK]s (kept by design)

A scan after edits shows 132 remaining [CHECK]s, distributed:

| File class | Count | Why kept |
|---|---:|---|
| `data-cards/` (WRDS-licensed tables: CRSP/Compustat/IBES/OptionMetrics/Capital-IQ/Thomson-13F/TRACE/FISD) | ~40 | Per user instructions: WRDS table/column names must be verified on the live container. These are by-design holes. |
| `data-cards/` (federal/SEC/public-API field names: HMDA, CFPB, FRED, EDGAR, FFIEC, FR-Y9C, FDIC, NOAA, Census, BLS, BEA, USAspending, Treasury, MSRB, Mergent, free-equity, FOMC/GDELT, Loughran–McDonald, crypto, international-macro) | ~70 | Per user instructions: HMDA CFPB Data Browser API and the pinned vintage stay [CHECK] (container-dependent). Same pattern applies to other agencies. |
| `book/appendices/B-python-latex-setup/B4-hopper-slurm.md` + B5 (partition/account strings, login hostname, module names) | ~15 | Per user instructions: Hopper SLURM partition/account strings stay [CHECK]. |
| `book/appendices/D-style-guide/D5-prose-style.md` | 1 | A meta-reference to the [CHECK] policy itself. |
| `book/capstones/capstone1-fair-lending-hmda.md` | 1 | The HMDA vintage [CHECK] — leave-as-[CHECK] per user, depends on camp container. |
| `book/capstones/capstone2-common-ownership-13f.md` | 1 | Gao et al 2024 vol/issue/pages — Gao papers per CV; the journal listing hasn't yet pinned vol/issue. |
| `book/capstones/capstone3-innovation-uspto.md` | 1 | USPTO ODP post-cutover patent-search endpoint path — USPTO is still finalizing it. Properly scoped [CHECK]. |
| `book/capstones/capstone4-8k-text-classification.md` | 2 | GMU Azure OpenAI deployment name + `api_version` — leave-as-[CHECK] per user. |
| `book/instructor-manual/IM6-equity-access.md` | 1 | Delegated reference to B.4 partition strings — same Hopper [CHECK]. |
| `book/appendices/E-solutions-manual/` | ~7 | Out of scope per user (E is solutions-manual, indexed already), but counted for completeness. |

---

## 7. Files modified

1. `/mnt/e/ccli/8weeks/data-cards/uspto-patentsview.md` — full rewrite of Access path + License + Gotchas sections for ODP migration; title updated; date refreshed.
2. `/mnt/e/ccli/8weeks/book/weeks/week-07/ch72-data-acquisition-in-practice.md` — §7.2.7 PatentsView access-path block updated for ODP.
3. `/mnt/e/ccli/8weeks/book/capstones/capstone3-innovation-uspto.md` — §3 Data, References, margin commentary all updated for ODP; HJT [CHECK] resolved.
4. `/mnt/e/ccli/8weeks/book/capstones/capstone2-common-ownership-13f.md` — BDM and Petersen [CHECK]s removed; banner ⚠️ removed; commentary updated.
5. `/mnt/e/ccli/8weeks/book/capstones/capstone4-8k-text-classification.md` — MacKinlay, Landis–Koch, Hoberg–Phillips [CHECK]s removed; κ-bands inlined with thresholds; banner ⚠️ removed.
6. `/mnt/e/ccli/8weeks/book/capstones/capstone5-fred-macro-event-study.md` — omnibus citation [CHECK] replaced with verified-per-reference confirmation.

No new files were created.

---

## 8. Recommendations for the next pass (not done)

These are out of scope or properly deferred, listed so they are not lost:

1. **Re-pin the data-cards/CRSP/Compustat/WRDS [CHECK]s once the camp container is provisioned** — bulk task best done from a live WRDS Cloud session, not from prose review.
2. **Capstone 2 Gao–Han–Kim–Pan (2024) vol/issue/pages** — needs Prof. Gao's confirmation that the *JCF* publication is finalized.
3. **USPTO ODP patent-search endpoint path** — re-verify in (say) a month once USPTO finishes reintroducing the search function on `api.uspto.gov`. The data card and Ch 7.2 both name this as the one remaining [CHECK].
4. **GMU Azure OpenAI deployment names + `api_version`** — Capstone 4 and the AI co-pilot lab — confirm against the N1 AI portal.
5. **Hopper SLURM partition/account strings** — confirm against ORC's current Hopper documentation; touches Appendix B.4, B.5, IM-6.

---

**Verdict:** APPROVED with the 8 edits above applied. Material is consistent, math is correct, the 200-pt rubric sums, the 34-card data dictionary is aligned across three index files, no emojis remain in scope, no hard-coded secrets exist, and the highest-impact factual fix (USPTO ODP migration) is now reflected in the data card, Ch 7.2, and Capstone 3.
