# Chapter 7.6 — The Manuscript Build

Maya filed her pre-analysis plan as a tagged commit, built the analysis dataset the plan refers to, and wrote the one-page identification memo that defends the assumption her $\beta_1$ rests on. The design half of her project is closed. The number is in hand — say it came out the way the PAP allowed it to: a 1.4-percentage-point reduction in the conditional Black–White denial gap, a result a hostile referee can trust because the forks were nailed down before she could see it. Now comes the part nobody warns you about: that number does not present itself. A correct result buried in a sloppy paragraph, a nineteen-column table, and a default-`matplotlib` figure will be skimmed and forgotten. The carrying of the result is a separate craft from the producing of it, and this chapter is that craft.

We compress the whole manuscript build into one chapter because the discipline is the same five times over. **Every artifact in a paper — abstract, tables, figures, introduction, literature review — is engineered for the same reader and against the same diagnostic: can a busy expert, reading cold, extract your contribution and decide to believe it in the time they will actually give you?** That time is brutally short — ninety seconds for an editor on the abstract, fifteen for a referee on a table. We take the five artifacts in the order a reader meets them and give the skeleton or template for each. Maya carries the worked example; Devon, Priya, Sam, and Leah appear where their designs differ.

---

## 7.6.1 The 250-word abstract: the five-sentence skeleton

The folk theory says an abstract summarizes the paper. That theory produces the worst student abstracts — the ones that read like a table of contents ("This paper studies X. We use data Z. We employ DiD. We find an effect."). The reveal is sharper: **the abstract is not a summary, it is a contract with three readers you will never meet, and the contract has five clauses.** The *editor* reads it in ninety seconds asking "in scope? credible enough to send out?" The *referee* reads it in three minutes asking "is the design strong enough that I won't have to write a fifteen-page rejection?" The *SSRN searcher* reads the last sentence asking "does this matter to the literature I'm building on?" The five-sentence skeleton satisfies all three without taxing any.

The five sentences, each roughly fifty words, in order:

1. **Question** — the relationship studied, with a verb that *matches your design*. "Estimates the causal effect" demands a causal design in sentence 3; "documents," "measures," "quantifies" suit descriptive or forecasting work. Mismatch between sentence 1's verb and sentence 3's design is the most common reason a referee marks an abstract "oversold."
2. **Setting** — the data by its *proper noun*, the years, the unit, the universe. "Detailed administrative data" is content-free; "HMDA loan-level data for 2014–2020 covering 47 million originations" is a credential.
3. **Design** — the identification strategy *and the assumption that makes it work*, in one sentence. This is the sentence the referee weights most and students underweight. "We use a difference-in-differences design" is half a sentence; name the variant, name the assumption, name the evidence for it.
4. **Finding** — the headline, with sign, magnitude, and precision *in a unit a non-specialist can parse*. "A statistically significant effect" is empty; "1.4 percentage points (95% CI: 0.8 to 2.0), a 23% reduction" is a headline someone can quote.
5. **Why-It-Matters** — a *what-now*, not a what-next. "Future research should explore X" is the most forgettable ending in empirical economics. Name a consequence: a number a regulator can act on, a limit on what the estimate supports.

Here is Maya's revised abstract, the five sentences stitched together (with one bonus heterogeneity sentence between Finding and Why-It-Matters — the only place heterogeneity belongs in an abstract), at 248 words:

> *This paper estimates the causal effect of the 2017 intensified fair-lending examination program on the Black–White denial-rate gap in covered U.S. counties. Using HMDA loan-level data for 2014–2020 covering 47 million originations, we exploit the staggered rollout of intensified examinations across 614 Office-of-the-Comptroller-of-the-Currency-supervised counties. We implement a staggered difference-in-differences estimator with cohort-specific event-study leads and lags, identifying the average treatment effect on the treated under the assumption that pre-treatment denial-rate trends in covered and uncovered counties were parallel — an assumption supported by F-tests on pre-period leads and by Oster's δ bound of 1.7 against unobservable selection. We find that intensified examinations reduce the Black–White denial-rate gap by 1.4 percentage points (95% CI: 0.8 to 2.0), a 23% reduction relative to the pre-treatment gap of 6.1 points and roughly one-quarter of the persistent national gap documented in Gao and Sun (2019). Effects are concentrated in counties with above-median minority-population shares and in lenders with above-median examination-finding histories, consistent with intensified supervision acting through targeted scrutiny rather than generalized deterrence. Our estimates imply that the 2017 examination reform closed roughly one-quarter of the persistent Black–White denial-rate gap in covered counties, suggesting that targeted supervisory intensity can complement — though not substitute for — broader reforms to lending practices.*

Four failure modes recur, each with a one-line diagnostic. The **Disappearing Finding** (sentence 4 has no number — *can a reader tell a friend what you found, with a number?*). The **Method-Heavy Abstract** (more than one sentence mentions an estimator — *how many of five sentences name a method?*). The **Literature-Review Opening** (sentence 1's subject is "a growing literature" rather than the question — *what is the subject of the first sentence?*). The **Diffuse Conclusion** (sentence 5 is boilerplate — *if I read only the last sentence, do I know what changes about the world?*). The skeleton survives across topics: Sam's sentence 1 uses "documents" and "estimates," not "estimates the causal effect," because Sam is making a forecasting-and-profitability claim, and the abstract is honest about which kind of claim it is.

---

## 7.6.2 Publication-grade tables: `etable`, stars discipline, panels

The abstract promises a number; the *table* is the artifact the referee turns to first to verify it. The reveal: **a publication-grade table is not a regression output, it is a re-engineered presentation of selected output, built so the reader can answer four questions in under a minute** — what is the headline coefficient (sign, magnitude, units), what is its precision, what is the sample (N, clusters, span), and what controls and fixed effects were in? Call it the **one-glance test**. Most student tables fail it because they were produced *by* the software (the defaults dump everything) rather than *for* the reader. Referees infer rigor from typography; you may resent that this is true and you must still produce the typography.

The workhorse is `pyfixest`'s `etable()`, whose LaTeX output is journal-clean by default (`booktabs` rules, no vertical lines, sensible rounding). The single short code snippet for this chapter is Maya's headline four-column DiD table:

```python
# nb7.6 cell 1 — Maya's headline DiD table to LaTeX via pyfixest etable()
import pyfixest as pf

# m1..m4: progressively saturated fitted pf.feols() models, county-clustered SEs.
table2 = pf.etable(
    [m1, m2, m3, m4],
    type="tex",
    coef_fmt="b (se)",                 # estimate above, SE below in parens, with stars
    keep=["treated_post"],             # show ONLY the headline coefficient
    labels={"treated_post": r"Treated $\times$ Post"},
    digits=3,
    digits_stats=0,                    # no "47123481.000" in the N row
    model_heads=["(1) Baseline", "(2) +Lender FE", "(3) Preferred", "(4) OCC sample"],
    signif_code=[0.01, 0.05, 0.10],    # three-tier finance-journal default
    notes=(
        "Standard errors clustered at the county level in parentheses. "
        "*** p<0.01, ** p<0.05, * p<0.10. Outcome: Black--White denial-rate gap (pp)."
    ),
    caption="Effect of intensified fair-lending examinations on the denial-rate gap.",
    label="tab:maya_did_headline",
)
```

Three flags do most of the work. `keep=["treated_post"]` is the most important typographic decision: it suppresses the forty rows of control coefficients (income deciles, loan amount) that distract from the one row the abstract is about — those go in an online appendix, shown on the main table only as Yes/No fixed-effects flags. `coef_fmt` fixes the *precision convention*: `"b (se)"` (estimate, SE in parens, stars) is the finance-journal default; the confidence-interval convention shows magnitude and precision but loses the at-a-glance star scan. Pick one and use it across every table in the paper. `signif_code` and `notes` together govern **stars discipline**, which has three rules: stars belong on the *headline row*, not control rows (a table starred everywhere conveys no information); stars must be *calibrated to the inference you would defend* — cluster-robust for panel/DiD, never classical-SE stars in one column and cluster-SE stars in the next; and stars *never substitute for magnitude* — a 0.0001 coefficient starred at 1% is significant and trivial.

Below the coefficient block sits the **fixed-effects indicator block** (Yes/No rows per FE, *including a Sample row* — a restricted-sample column is a different population and the reader must see it), then the sample footer. Always report the **cluster count** when SEs are clustered: 47 million observations with 614 county clusters has an effective sample size closer to 614, and reviewers know it. Use thousands separators ("47,123,481" reads in half a second; "47123481" does not).

When you present *one specification across multiple outcomes or sub-samples* — rather than alternative specifications, which go in columns — break the table into **panels**: vertical blocks (Panel A, Panel B) sharing column headers, each with its own coefficient row, so the reader scans down the preferred column and reads the heterogeneity story in two seconds. Three tables every empirical paper needs: the **headline** table (Table 2, progressively saturated columns), the **robustness** table (many columns, one row, the eye scans left-to-right confirming the coefficient does not move), and the **heterogeneity** table (panels). Plus Table 1, summary statistics with treated/control **balance** if the design is causal — the table students most often phone in and the reader's first encounter with your variables.

---

## 7.6.3 Publication-grade figures: event study, specification curve, forest

Many students treat figures as decoration. In modern empirical finance this is backward: the event-study plot *is* the parallel-trends argument; the specification curve *is* your robustness disclosure; the forest plot *is* the heterogeneity result. The reveal: **a publication-grade figure is engineered to make one comparison salient, with everything not in service of that comparison removed or demoted.** This is Tufte's **data-ink ratio** turned into a checklist — remove the top and right spines, suppress the grid (or set it very light gray), one accent color for data and gray for context, axis labels with units, no internal title (the caption does that). The figure passes its one-glance test when a referee can name the *x*-axis, the *y*-axis, the comparison, and the bottom-line claim in fifteen seconds. Design backward from the claim: write the one-sentence caption first, then engineer the plot so the claim is visible without reading it.

Three figures every paper needs, with their anatomy:

**The event-study plot** is the visual evidence for parallel trends in a DiD design. The horizontal axis is *event time*, not calendar time (in a staggered design the same calendar year is $\tau=-2$ for one cohort and $\tau=+1$ for another). The vertical axis is the outcome's natural unit (percentage points, matching the abstract's headline). A thin dashed vertical at $\tau=0$ separates pre from post; a thin gray horizontal at $y=0$ lets the reader judge whether bands cover the null. Point estimates are filled circles with error bars of $\hat{\delta}_\tau \pm 1.96\,\hat{\sigma}_\tau$, no whisker caps. The reference period $\tau=-1$ returns 0 by construction — show it as an open circle or drop it; never plot it as a normal point with a zero-width bar, which fakes a magically precise estimate. Pre-period coefficients flat around zero, post-period falling to $-1.4$: the entire parallel-trends-plus-dynamics claim, visible without the caption.

**The specification-curve plot** (Simonsohn, Simmons, and Nelson, 2020) shows the headline coefficient across the menu of plausible specifications, *sorted ascending* (an unsorted curve is noise), in an upper panel — color the dot black if its 95% CI excludes zero, gray otherwise. A *lower panel* of choice indicators (which sample, outcome variant, FE saturation, clustering, winsorization each spec used) lets the reader scan the rightmost, most-positive specs and read down to see which choice drives them. Without the lower panel the figure is half-useful. Keep the choice rows under twelve; if your menu has thirty free dimensions, some of them were forks the PAP should have committed.

**The heterogeneity forest plot**, borrowed from clinical trials, shows each subgroup as a dot with a horizontal 95% CI line, ordered meaningfully (by moderator, then by quantile within moderator — never alphabetically), with the overall ATT at the top and a dashed vertical reference at the overall ATT so the reader sees which subgroups beat the average. Maya's forest tells the whole story in five seconds: a monotone minority-share gradient from $-0.2$ (Q1) to $-2.3$ (Q4), a parallel exam-history gradient — a twelve-row table cannot do that.

```python
# nb7.6 cell 2 — publication-grade event-study plot (the Tufte styling block)
import numpy as np
import matplotlib
matplotlib.use("Agg")                       # reproducible, headless
import matplotlib.pyplot as plt

tau   = np.arange(-4, 5)
delta = np.array([0.10, -0.05, 0.02, 0.00, 0.00, -0.85, -1.20, -1.35, -1.40])  # pp
sigma = np.array([0.35, 0.32, 0.28, 0.25, 0.00, 0.30, 0.32, 0.35, 0.38])

fig, ax = plt.subplots(figsize=(6.5, 4.0))
ax.axvspan(tau.min() - 0.5, -0.5, color="0.92")          # shade pre-period
ax.axhline(0, color="0.5", lw=0.8)                        # null reference
ax.axvline(0, color="0.2", lw=0.8, ls="--")              # event line
ax.errorbar(tau, delta, yerr=1.96 * sigma, fmt="o", color="black",
            ecolor="black", elinewidth=1.2, capsize=0, markersize=6)
ax.set_xlabel("Event time (years from intensified examination)")
ax.set_ylabel("Black–White denial-rate gap (percentage points)")
ax.set_xticks(tau)
ax.spines["top"].set_visible(False)                       # Tufte: demote frame
ax.spines["right"].set_visible(False)
fig.tight_layout()
fig.savefig("fig_maya_event_study.pdf", dpi=300)          # vector, not raster
```

Production discipline finishes the figure: save **vector** (PDF/SVG), produce at the size it will display (do not render `figsize=(12,8)` then shrink — the fonts come out wrong), and name files after the manuscript element (`fig_maya_event_study.pdf`) so a reviewer's "re-render figure 3 with a wider band" is a one-parameter change. Captions run two to four lines: what the figure is, the source/sample, the inference convention, and an optional bottom-line-claim line. Use color-blind-safe palettes; never red-green together.

---

## 7.6.4 The introduction: five paragraphs, one job each

You have the abstract, the tables, the figures. The introduction restates the contract at one degree more detail. The reveal: **a publication-grade introduction is exactly five paragraphs, and each paragraph has exactly one job.** The five paragraphs form a *funnel* — broad institutional fact, narrowing to the question, the design, the numbers, then widening back out to the literature. Each depends on the one before: the hook motivates the question, the question motivates the design, the design produces the findings, the findings define the contribution. About 1,200–1,600 words total.

- **Paragraph 1 — the Hook** (100–180 words). One concrete fact that locates the reader in the world, *not* a literature review and *not* a definition. "Mortgage denial rates differ across racial groups" is generic; "In 2019, a Black applicant for a conventional mortgage in the Atlanta MSA was 1.7 times as likely to be denied as a White applicant with the same income, credit score, debt-to-income ratio, and loan-to-value ratio (Gao and Sun, 2019)" is specific — a year, a city, a comparison, a magnitude the reader can picture. The hook's last sentence bridges to the question.
- **Paragraph 2 — the Question** (150–250 words). The first sentence names what is *known*; the middle sentences name what is *not* known (the gap); the last states the question with precision and *matching the abstract's sentence 1*. Cite three to seven papers, no more — the reader cannot hold more in working memory, and the gap statement must be defensible (failing to acknowledge a close prior is how a referee comes to write "the author appears unaware of …", a near-fatal review).
- **Paragraph 3 — the What-We-Do** (200–350 words). Data, design, identifying assumption, evidence for the assumption. This is where the referee decides revise-and-resubmit versus reject, so it earns the most effort. Name the data by name, the design variant (not just "DiD" but "staggered DiD with cohort-specific leads and lags following Callaway and Sant'Anna, 2021"), the assumption (parallel trends), and the assumption's defenses (flat pre-period leads in Figure 1; Oster δ of 1.7; the Sun and Abraham heterogeneity-robust re-estimate).
- **Paragraph 4 — the Findings** (180–280 words). Not a tour of the tables — a curated four or five numbers: the headline with CI, the temporal structure, the key heterogeneity, the placebo reassurance, and an optional external-validity caveat. Map directly to the abstract's sentence 4 and Table 2.
- **Paragraph 5 — the Contribution** (150–250 words). Three strands, one literature each, naming what the paper *adds* to each. Three matches the cognitive granularity of most papers' literatures; two is thin, four is too many. Use precise descriptors ("the first credibly identified estimate of …") not open superlatives ("the first …", "the most important …"), because a superlative invites the reader to find the counterexample, and the counterexample becomes the referee's hook.

Diagnosing a **sick intro** is the negative template. Tag each paragraph with its job (Hook / Question / What-We-Do / Findings / Contribution) or "Other"; any "Other," or two paragraphs sharing a job, is the **Wandering Funnel** (the eleven-paragraph, 4,200-word February draft). The **Methodological Detour**: paragraph 3 becomes a methods tutorial (more than a quarter of its sentences defend the method rather than describe it — move the defense to Section 3). The **Diffuse Hook**: fewer than three numbers-or-proper-nouns in the first three sentences. The **Findings-Free Findings Paragraph**: fewer than four numbers in paragraph 4. The **Conscripted Question Paragraph**: more than seven citations in paragraph 2. The **Contribution Inflation**: more than two superlatives in paragraph 5. Length correlates with sickness; the worst introductions are the longest. Maya's revised intro is 1,150 words — 73% shorter than her draft — and a reader who reads only those five paragraphs knows the setting, question, design, headline, and contribution.

---

## 7.6.5 The literature review: from bullet list to a three-strand argument

The contribution paragraph names three literatures and claims one contribution to each. Section 2 has to *earn* those claims. The reveal: **a publication-grade literature review is not a list of papers, it is an argument that, by its end, has earned the contribution sentences in the introduction** — so completely that a reader could recite the contribution paragraph without having read the introduction. The load-bearing structure is a **three-strand braid**: each strand is one literature, each has a beginning (foundational work), a middle (recent extensions), and an end (the gap your paper fills), and the three are interwoven so the reader sees them *converge* on your question rather than reading three stacked mini-reviews.

For Maya, the strands are: **A**, fair-lending disparities (Munnell, Tootell, Browne, and McEneaney, 1996 → Bayer, Ferreira, and Ross, 2018 → Gao and Sun, 2019 → Bhutta, Hizmo, and Ringo, 2024; gap: no credibly identified estimate of a *regulatory intervention*'s effect); **B**, real effects of bank supervision (Berger and Bouwman, 2013; Hirtle, Kovner, and Plosser, 2020; gap: focus on safety-and-soundness, not *distributional* outcomes); **C**, the methodological literature on staggered DiD and disciplined heterogeneity (Callaway and Sant'Anna, 2021; Sun and Abraham, 2021; Oster, 2019; Benjamini and Hochberg, 1995). The strands are *unequal* — A is deepest (the substantive contribution), C briefest (the deep methods belong in Section 3) — and their proportions track the contribution each earns. Each strand is **chronological-but-thematic**, grouping citations into *intellectual moves* (documenting the gap → characterizing its mechanisms → probing interventions), not a flat year-by-year list. A synthesis-and-gap closer (Section 2.4) names the three gaps and claims the contribution to each.

Two principles govern every citation. **Cite what the paper *finds*, not just *that* it exists**: "Bayer, Ferreira, and Ross (2018) study mortgage disparities" wastes the citation; "Bayer, Ferreira, and Ross (2018) show that 30–40% of the Black–White origination gap is driven by the geographic concentration of high-cost lenders in minority neighborhoods" makes it contribute to the argument. **Characterize accurately enough that the cited author would recognize it** — for every substantive citation you should be able to point to the table or sentence it comes from; otherwise the neighboring scholar becomes the angry referee writing "the author has mischaracterized …". Six pathologies recur: the **Annotated Bibliography** (sentence-per-paper, no transitions), the **Kitchen-Sink Review** (more than ~60 citations; triage to the moves), **Self-Citation Inflation** (above ~15% of citations), the **Stale Review** (newest citation more than two years pre-submission), **Mischaracterization**, and the **Disconnected Methodological Section** (Strand C as a standalone defense rather than the toolkit the substantive question demands). Maya trimmed from 43 cited papers to 28 strategically chosen ones across the three strands.

The through-line across all five artifacts is one idea: **each artifact does the same job — credibilizing the contribution — at a different level of detail.** The abstract delivers the contract; the tables and figures deliver the evidence; the introduction restates the contract with evidence; the literature review establishes why the contract is credible. The week's discipline is making all four say the same thing in different registers — and, crucially, the same number. The 1.4 in the abstract is the 1.4 in Table 2, the $-1.4$ at event-year $+4$ in Figure 1 (explain any difference between a pooled ATT and a long-run coefficient in the caption), and the 1.4 in paragraph 4. A reader who catches them disagreeing stops trusting all of them.

---

## 7.6.6 Where this goes next

You arrived with a filed PAP, a clean dataset, and an identification memo. You now have the second half of the project: the manuscript that carries the result — an abstract that promises only what the paper delivers, tables and figures engineered for the one-glance test, a five-paragraph introduction, and a three-strand literature review that earns the contribution sentence. In **Week 8** you execute and defend: Chapter 8.2 turns each row of your identification memo's threats-and-responses table into a real robustness result (the placebos, the alternative standard errors, the specification curve you sketched here); peer review hands your manuscript to another student who will read it the way you learned to read papers in Week 5 — tables first, identifying assumption second, prose last and skeptically. Write the manuscript so there is no row they can ambush you on.

---

## Your Turn

In a fresh **manuscript-build** notebook, walk from your own filed results to the four engineered artifacts. Build yourself three auditing helpers as you go — an `abstract_audit()`, an `intro_audit()`, and a table/figure checklist — that flag the obvious failure modes before your mentor does (necessary, not sufficient: a clean audit certifies only that the artifact is not *obviously* sick). You will: (1) draft your 250-word abstract in six passes — skeleton, numbers, verbs, assumption, closing, word count — and run `abstract_audit()` until it returns only `word_count`; (2) render your headline table with `pf.etable(..., keep=[...], digits_stats=0)` and confirm it passes the one-glance test cold; (3) produce your event-study (or, for Sam, an intraday-persistence curve; for Devon, a minute-level de-peg event study), specification-curve, and forest plots with the Tufte styling block, saved as vector PDFs at display size; (4) rewrite your introduction to five paragraphs and run `intro_audit()`; and (5) braid your literature spreadsheet into three strands. The notebook emits a `manuscript/` folder — abstract, tables, figures, intro, Section 2 — that is the spine of the Week-8 paper.

**Reflection prompts to carry into the lab and Mentor Session 7.**

1. *Verb-design match.* Reread your abstract's sentence 1 and sentence 3. Does the verb ("estimates the causal effect" / "documents" / "quantifies") match the design you can actually defend? If you wrote "causal" but your identification memo's residual-concern column is full, downgrade the verb and adjust every claim in the abstract to match.

2. *The one-glance test, run by a stranger.* Hand your headline table and event-study figure — no caption, no surrounding prose — to someone who has not seen your project. Can they name, in under a minute for the table and fifteen seconds for the figure, the headline coefficient, its precision, the sample, and the bottom-line claim? Every question they cannot answer is a design decision (a missing `keep=`, an unlabeled axis, an absent cluster count) you still owe the reader.

3. *Tag your own intro.* Print your introduction and write H / Q / WWD / F / C / O beside each paragraph. Any "Other" paragraph, or any job appearing twice, is a cut or a relocation. Then count the numbers in your Findings paragraph and the superlatives in your Contribution paragraph: fewer than four numbers, or more than two superlatives, and the paragraph is sick.

---

See also **Appendix D (Style Guide)** for the table-craft and reporting-regressions conventions this chapter applies (the `booktabs`/`siunitx` preamble, the stars-and-precision rules, thousands separators, caption-versus-notes division), and **Week 8** for the execution, robustness, and peer-review stages that turn this manuscript into a defended paper.

### Citations

- Benjamini, Y., and Y. Hochberg (1995). "Controlling the False Discovery Rate." *Journal of the Royal Statistical Society B* 57(1): 289–300.
- Bayer, P., F. Ferreira, and S. L. Ross (2018). "What Drives Racial and Ethnic Differences in High-Cost Mortgages? The Role of High-Risk Lenders." *Review of Financial Studies* 31(1): 175–205.
- Berger, A. N., and C. H. S. Bouwman (2013). "How Does Capital Affect Bank Performance during Financial Crises?" *Journal of Financial Economics* 109(1): 146–176.
- Bhutta, N., A. Hizmo, and D. Ringo (2024). "How Much Does Racial Bias Affect Mortgage Lending? Evidence from Human and Algorithmic Credit Decisions." *Journal of Finance* (forthcoming). [CHECK]
- Callaway, B., and P. H. C. Sant'Anna (2021). "Difference-in-Differences with Multiple Time Periods." *Journal of Econometrics* 225(2): 200–230.
- Gao, L., and J. Sun (2019). "Lender-Borrower Race-Match Effects on Mortgage Approval." *Proceedings of the National Academy of Sciences* 116(19): 9293–9302.
- Hirtle, B., A. Kovner, and M. Plosser (2020). "The Impact of Supervision on Bank Performance." *Journal of Finance* 75(5): 2765–2808.
- Munnell, A. H., G. M. B. Tootell, L. E. Browne, and J. McEneaney (1996). "Mortgage Lending in Boston: Interpreting HMDA Data." *American Economic Review* 86(1): 25–53.
- Oster, E. (2019). "Unobservable Selection and Coefficient Stability: Theory and Evidence." *Journal of Business & Economic Statistics* 37(2): 187–204.
- Simonsohn, U., J. P. Simmons, and L. D. Nelson (2020). "Specification Curve Analysis." *Nature Human Behaviour* 4(11): 1208–1214.
- Sun, L., and S. Abraham (2021). "Estimating Dynamic Treatment Effects in Event Studies with Heterogeneous Treatment Effects." *Journal of Econometrics* 225(2): 175–199.
- Tufte, E. R. (1983). *The Visual Display of Quantitative Information*. Cheshire, CT: Graphics Press.
