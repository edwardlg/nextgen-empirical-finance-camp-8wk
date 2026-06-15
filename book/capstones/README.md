# Capstone Gallery

This gallery holds five complete, publication-style papers — the kind of thing your own capstone is reaching toward. Each one runs from abstract to honest limits the way a real empirical-finance paper does: a question worth asking, a literature it sits inside, a dataset, a stated identifying assumption, a headline table, robustness organized around the threats, and a conclusion that says plainly what the design cannot establish. They exist so that before you build your own paper, you can study what "done" looks like at full size, instead of inferring it from a rubric.

Each paper is paired with an annotated commentary — a "How this paper was built" section at the end — that narrates every choice against the camp's conventions and the Appendix-D table-and-prose standard: why the standard errors are clustered this way and not that, why the identifying assumption is phrased as one sentence, why a verb was softened from "causes" to "is associated with." The paper shows you the finished object; the commentary shows you the decisions that produced it. Read both.

## Read this first: every result here is synthetic

**All quantitative results in these five papers are synthetic and illustrative. They were constructed for instruction — built to be consistent with the relevant Week-6 notebooks and seeded so they regenerate bit-for-bit — and they are NOT empirical findings.** No coefficient, standard error, sample size, correlation, or figure in this gallery describes a real lender, a real firm, a real patent, a real filing, or a real market. Several of the datasets are built with a *known, planted* truth precisely so the method can be checked against a ground truth that no real dataset gives you — that is the whole point of a synthetic study (Lab 4's slogan: when you are unsure whether a method does what it claims, build a universe where you know the truth and check).

Read these papers for their *craft* — the structure, the table design, the calibrated verbs, the disciplined hedging, the honest limits — and copy the craft, never the numbers. Where a real paper would name a data vintage, a journal, or a statute, an exemplar says so and stops. Where a citation could not be verified, it is tagged `[CHECK]` rather than fabricated, exactly as the conventions require. Every paper carries its own synthetic-data notice at the top; this one governs the whole gallery.

## The five capstones

Each paper is tied to a dataset track and to a Gao or anchor paper that defines its question, so you can trace the line from a published study to a camp-scale reproduction.

1. **[Fair Lending on HMDA](./capstone1-fair-lending-hmda.md)** — *Track:* mortgage-denial decomposition · *Anchor:* Gao & Sun (2019) · *Notebook:* `nb6.4`.
   How much of the minority/non-minority mortgage-denial gap survives the legitimate underwriting controls? A Blinder–Oaxaca decomposition on a synthetic, HMDA-like panel with a *planted* amount of discrimination, used to show exactly how an omitted credit score inflates the residual and how over-controlling on a pricing channel collapses it. The model paper for the selection-on-observables discipline: a residual is a bounded, fragile number, not a verdict.

2. **[Common Ownership from 13F](./capstone2-common-ownership-13f.md)** — *Track:* institutional-holdings overlap · *Anchor:* Gao, Han, Kim & Pan (2024), "Overlapping institutional ownership along the supply chain and earnings management of supplier firms," *Journal of Corporate Finance*, 84, 102520.
   When the same institutions hold large stakes in competing firms, does it change how much those firms voluntarily disclose? Builds a firm-pair common-ownership measure from synthetic 13F-like holdings and runs a panel fixed-effects regression of a disclosure index on peer common ownership. The exemplar for disciplined observational work: the association survives the most demanding specification, and the paper refuses to call it causal. *Note: this is a **student-track variant** — the anchor paper's actual outcome is supplier-firm earnings management (discretionary accruals); the capstone substitutes voluntary disclosure as a sibling outcome that is easier to build at camp scale. See the variant notice at the top of the capstone.*

3. **[Innovation from USPTO PatentsView](./capstone3-innovation-uspto.md)** — *Track:* market-based innovation measurement · *Anchor:* Kogan, Papanikolaou, Seru & Stoffman (2017) · *Notebook:* `nb6.1`.
   What is a single patent worth? Reconstructs the KPSS measure — the abnormal stock return in a three-day window around a patent's grant, scaled by market cap — on a seeded synthetic panel where the true patent value is known. Recovers the planted value, validates it against next-year profitability, and shows how contaminating news in the grant window degrades the measure. The model for turning an unobservable into an observable and checking the recovery.

4. **[SEC 8-K Text Classification](./capstone4-8k-text-classification.md)** — *Track:* AI / text-as-data module · *Notebook:* `nb6.5` · *Out-of-sample validated.*
   Can a classifier recover the *type* of event an 8-K reports from its text, and does the short-window return reaction differ by type? Treats the classifier as a measurement instrument, not an oracle: hand-labels a gold set, freezes the rubric against a held-out test split *before* any returns are examined, and reports out-of-sample precision/recall/F1 and Cohen's κ. The exemplar for using an LLM in research honestly — with the training-data-leakage audit and full prompt disclosure that applied work too often skips.

5. **[FRED Macro Event Study](./capstone5-fred-macro-event-study.md)** — *Track:* macro-announcement event study · *Anchor:* Kuttner (2001); Bernanke & Kuttner (2005).
   Do FOMC surprises move the market? Separates the *expected* component of each rate decision from its *surprise* and regresses the announcement-day equity return on the surprise, with HAC (Newey–West) standard errors, placebo dates, and split-sample checks. The model for the full event-study scaffolding from free data — and for reporting an "announcement-day association" under a tightly defended timing assumption rather than overclaiming a causal effect.

## How to use the gallery

These papers are working documents, not display pieces. Three ways to put them to work:

**Read them like a referee.** Bring the Week-5 anatomy of a journal paper to bear: take each one apart section by section — does the introduction state a contribution, is the identifying assumption a single defensible sentence, does the robustness section answer the threats it names, does the conclusion concede what the design cannot reach? Practice the skeptical read before you ask anyone to give your own paper that read. (See [Week 5: Anatomy of a Journal Paper](../weeks/week-05/mentor5-anatomy-of-a-jf-paper.md).)

**Model your own paper's structure on them.** When you build your capstone, use the matching exemplar as a structural template: the same section order, the same one-sentence identifying assumption, the same threats-then-robustness rhythm, the same Appendix-D table craft. You are not copying a result — you are inheriting a proven shape for the argument. The "How this paper was built" commentary at the end of each paper is the map for doing that translation.

**Know the standard you are graded against.** These exemplars are written to the same rubric your capstone will be assessed on. Read the [Week-8 capstone rubric](../weeks/week-08/assessment8.md) alongside the paper and ask, criterion by criterion, where this exemplar would score and why — then hold your own draft to the same bar.

**A note for instructors.** These five papers double as the instructor's anchor papers: they are the worked references in [IM-5: Answer Keys and Anchor Work](../instructor-manual/IM5-answer-keys-anchor-work.md), where each is mapped to the rubric and to its source notebook for use in grading calibration and class discussion.
