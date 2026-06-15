# Ch 4.6 — Robustness v2: Multiple Testing, Heterogeneity, Mechanism, and External Validity

You have a result. Priya's wildfire regulation moved premiums by \$70 (Chapter 4.1); Maya's staggered fair-lending examinations cut the minority denial gap by 1.4 percentage points (Chapter 4.2); somebody's discontinuity, synthetic control, or Bartik instrument landed a clean coefficient with a star next to it. The design is honest, the parallel-trends plot is flat, the standard errors are clustered. You think you are done. You are not done, because the moment you present that number to a room of economists, four questions come back, and they come back *every single time*. *Did you torture the data until one of your many tests confessed? Is the effect the same for everyone, or are you averaging over something interesting? Through what channel does it actually work? And would any of this replicate somewhere — or sometime — else?*

This chapter is the set of robustness checks a referee will demand of any quasi-experimental result, and it is deliberately placed at the end of Week 4 because it applies to *all* of the designs you just learned — DiD, RD, synthetic control, shift-share alike. We compress what could be a whole unit into four tools, each answering one of the four questions: (1) **multiple-testing corrections** that keep your p-values honest when you run many tests; (2) **heterogeneous-treatment-effect** estimation that finds *for whom* the effect is large; (3) **mechanism analysis without bad controls**, which finds *through what channel* without committing the most common identification mistake in empirical work; and (4) **external validity and transportability**, which asks whether the number travels. Maya, whose staggered DiD on county-year denial gaps is the running example from Chapter 4.2, will carry us through all four. The unifying lesson is the one Week 4 has been teaching all along: for every check, *state the assumption, then say exactly what you would see if it broke*.

---

## 4.6.1 Multiple testing: why "p < 0.05" lies when you run many tests

Run your headline regression once and a p-value of 0.05 means what you think it means. Run a heterogeneity table with twelve rows, an outcome table with eight columns, and a battery of twelve placebos, and you have quietly run thirty-some tests — and *several of your "significant" findings are false, and you cannot tell which*. Here is the arithmetic. If every null is true and you run $m$ independent tests each at level $\alpha = 0.05$, the probability that *at least one* rejects falsely is

$$ \Pr(\text{at least one false rejection}) = 1 - (1-\alpha)^m. $$

For $m=1$ this is 0.05; for $m=5$, 0.226; for $m=14$ it crosses 0.50; for $m=100$ it is 0.994. At one hundred tests you are essentially *guaranteed* a spurious "discovery." The quantity $1-(1-\alpha)^m$ is the **family-wise error rate (FWER)** — the probability that *any* test in your family is a false positive. The fixes re-define what "significant" means once you are in a family; which fix you want depends on *which error rate* you control and *across what family*.

**Bonferroni** is the blunt, oldest tool: to hold FWER at $\alpha$ across $m$ tests, declare each significant only if its raw p is below $\alpha/m$ (equivalently, multiply each p by $m$). The one-line proof is the union bound,
$$ \Pr\!\left(\bigcup_{j=1}^m \{p_j < \alpha/m\}\ \Big|\ \text{all } H_0\right) \le \sum_{j=1}^m \Pr(p_j < \alpha/m) = m\cdot\frac{\alpha}{m} = \alpha, $$
which needs *no* assumption about dependence — its superpower and its weakness, since it must assume the worst case and is needlessly conservative when tests are correlated.

**Holm (1979)** is Bonferroni's strictly better sequential cousin. Sort the p-values ascending, $p_{(1)}\le\cdots\le p_{(m)}$; compare $p_{(1)}$ to $\alpha/m$, then $p_{(2)}$ to $\alpha/(m-1)$, and so on, *stopping at the first non-rejection*. Once you reject a few hypotheses the remaining family is smaller, so the threshold loosens. Holm rejects everything Bonferroni does and sometimes more, at the same FWER guarantee — there is never a reason to prefer Bonferroni except that referees recognize the name.

**Benjamini–Hochberg (1995)** changes the *question*. Instead of FWER it controls the **false-discovery rate (FDR)** — the expected fraction of false positives *among* your rejections, $\text{FDR}=\mathbb{E}[V/\max(R,1)]$, with $R$ rejections and $V$ false ones. Sort ascending, find the largest $k$ with $p_{(k)}\le \frac{k}{m}\,\alpha$, and reject the smallest $k$. FDR is a weaker guarantee than FWER and therefore lets through many more rejections; it is the right framework when you are *screening* an exploratory family and can tolerate a few false positives, not for the paper's headline. **The headline lives or dies under FWER.**

**Romano–Wolf (2005, 2016)** is the tool a sharp discussant will name. It is a bootstrap step-down that controls FWER but *credits the correlation* among your tests. Bonferroni assumes worst-case independence; when your twelve heterogeneity cuts share the same panel they move together, the *effective* number of independent tests is below twelve, and you can afford a looser threshold. Romano–Wolf operationalizes this by bootstrapping the *joint* distribution of the test statistics — recentered so the null is imposed — and reading the quantile of the *maximum* statistic across the still-active hypotheses. When the statistics co-move, that maximum is barely larger than any one of them, so the procedure is permissive; when they are independent, it recovers Bonferroni-like behavior. Anderson (2008) popularized it for multi-outcome tests, and it is now standard for the primary heterogeneity or multi-outcome table.[^anderson2008][^rw2016]

The ordering, when tests are positively correlated, is **Holm $<$ Romano–Wolf $<$ BH** in number of rejections: Romano–Wolf beats Holm by crediting correlation, BH beats Romano–Wolf by controlling the weaker error rate. For Maya's twelve heterogeneity cuts, Holm might reject 3, Romano–Wolf 5, BH (at 10%) 6.

```python
# nb4.6 cell 1 — Holm step-down and Benjamini-Hochberg FDR on a vector of p-values.
import numpy as np

def holm_adjust(pvals, alpha=0.05):
    pvals = np.asarray(pvals, float); m = pvals.size
    order = np.argsort(pvals); running = 0.0; adj_sorted = np.empty(m)
    for k in range(m):                                   # ascending: easiest first
        running = max(running, (m - k) * pvals[order][k])
        adj_sorted[k] = min(running, 1.0)
    adj = np.empty(m); adj[order] = adj_sorted
    return adj, adj < alpha

def bh_fdr(pvals, alpha=0.10):
    pvals = np.asarray(pvals, float); m = pvals.size
    order = np.argsort(pvals); running = 1.0; adj_sorted = np.empty(m)
    for k in range(m - 1, -1, -1):                       # descending: running min
        running = min(running, m / (k + 1) * pvals[order][k])
        adj_sorted[k] = running
    adj = np.empty(m); adj[order] = adj_sorted
    return adj, adj < alpha

raw = np.array([0.008, 0.042, 0.018, 0.035])            # Maya's four heterogeneity cuts
print("Holm:", holm_adjust(raw)[0].round(3))
print("BH  :", bh_fdr(raw)[0].round(3))
```

The decision tree is small. *Single test?* No adjustment. *Family, and any false positive is a problem?* FWER — Holm for small $m$ or unknown correlation, Romano–Wolf for $m\gtrsim 10$ correlated tests. *Family, exploratory, some false positives tolerable?* FDR via BH. The load-bearing two-word phrase of the whole section is **"after adjustment"**: report what survived the correction, not what was significant before it. And remember the families that hide — multiple *outcomes* on the same regression, multiple *placebo* subsamples, subgroups *discovered by looking at the data* (the garden of forking paths; Gelman & Loken 2014) — each contributes to the count.[^gl2014]

---

## 4.6.2 Heterogeneous treatment effects: for whom is the effect large?

The average effect is one number, and "is the effect the same for everyone?" is the most-asked question after every talk. Done well, heterogeneity turns "the policy cut denial gaps by 1.4pp on average" into "by 3.1pp where examinations are intense and 0.4pp elsewhere — and *that* is where the policy story lives." Done badly, it is fishing for whichever subgroup has a small p-value. The discipline that separates the two is **pre-registration**: a heterogeneity claim is honest only to the extent the subgroups or moderators were committed *before* the data spoke.

Three estimands, in increasing resolution. The **ATE** $=\mathbb{E}[Y(1)-Y(0)]$ and the **ATT** $=\mathbb{E}[Y(1)-Y(0)\mid D=1]$ are single numbers — Maya's Callaway–Sant'Anna estimate from Chapter 4.2 is an ATT. The **conditional average treatment effect**, $\tau(\mathbf{x}) = \mathbb{E}[Y(1)-Y(0)\mid \mathbf{X}=\mathbf{x}]$, is *the* object of heterogeneity analysis: a *function* of covariates. Its coarsening to a finite list of subgroups, $\tau_g = \mathbb{E}[\tau(\mathbf{X})\mid \mathbf{X}\in g]$, is the **group average treatment effect (GATE)** — a number. CATE is a function; GATE is a number, and the function is what makes the argument legible.

The **cautious tool** is the subgroup regression: split by a pre-specified discrete moderator and re-estimate. Maya runs Callaway–Sant'Anna separately on light-, moderate-, and intense-examination states and finds $-0.4$, $-1.5$, and $-3.1$pp — the headline $-1.4$ was the average of three meaningfully different numbers. A *monotone dose-response* like this is not just a robustness check; it is positive identification evidence, because a confounder would have to vary in lockstep with intensity to mimic it. The **one-model tool** is the interaction regression, which keeps the whole sample and lets the effect slope with a continuous moderator $M_i$:

$$ Y_{it} = \alpha_i + \gamma_t + \beta_0\, D_{it} + \beta_1\, D_{it}\cdot M_i + \boldsymbol{x}_{it}'\boldsymbol{\delta} + \varepsilon_{it}, $$

so the implied effect is the line $\tau(M)=\beta_0+\beta_1 M$. Two traps: the linearity in $M$ is an *assumption* (check it by binning $M$ into terciles and seeing whether the three coefficients line up); and with unit fixed effects you *cannot* identify the main effect of a time-invariant $M_i$ — it is absorbed by $\alpha_i$ — only its interaction with treatment.

The **modern tool** is the **causal forest** (Wager & Athey 2018; Athey, Tibshirani & Wager 2019), which estimates $\tau(\mathbf{x})$ over a high-dimensional $\mathbf{x}$ with a random-forest architecture adapted for causal inference.[^wa2018][^atw2019] Two ideas make it legitimate rather than a fishing expedition: it splits on *heterogeneity in the treatment effect* (not in $Y$), and it enforces **honesty** — the half-sample that *selects* the splits is disjoint from the half that *estimates* the leaf effects, which is what delivers valid pointwise confidence intervals. Crucially, the *list* of candidate features is pre-registered even though the forest decides which matter. You report the **CATE histogram** (how spread out are the per-unit effects?), the **feature-importance** plot (which covariates drive the spread?), and a formal **Best-Linear-Predictor test** (Chernozhukov, Demirer, Duflo & Fernández-Val 2018) that asks whether the forest found *any* real heterogeneity: a BLP p $<0.05$ is the green light to discuss disaggregated predictions; p $>0.10$ is a red flag for over-fitting.[^cddfv2018]

```python
# nb4.6 cell 2 — causal forest CATE on Maya's panel. Residualize Y and treatment on
# (county, year) FE first (Robinson/DML), then feed residuals to an honest forest.
import numpy as np
from econml.grf import CausalForest

X = df[["intensity_bin", "min_share", "msa", "cra_overlap", "pop", "median_inc"]].to_numpy()
forest = CausalForest(n_estimators=2000, honest=True, min_samples_leaf=50, random_state=42)
forest.fit(X=X, T=df["exam_resid"].to_numpy(), y=df["gap_resid"].to_numpy())
tau_hat = forest.predict(X)                      # per-unit CATE; aggregate to GATEs for reporting
```

The pre-registration discipline comes in three levels: **Level 1**, the PAP names the exact subgroups (gold standard, run with Romano–Wolf across the family); **Level 2**, the PAP names the *feature list* and a causal forest decides what matters (the modern compromise, made honest by the forest's sample-splitting); **Level 3**, split the sample into a discovery half (explore freely) and a confirmation half (report only this), manufacturing a pre-registration ex post at the cost of half your power. The wrong move is claiming Level 1 when you had Level 3. And every heterogeneity claim is a *separate* causal claim that inherits the headline's identification only when the moderator is **pre-treatment** — check the event-study leads *within* each subgroup, and worry about composition and reverse causality in the moderator. Finally: if heterogeneity *vanishes*, report it as a finding, and **quantify the null** ("the CIs rule out per-subgroup effects more than 1pp from the average") rather than letting wide intervals masquerade as uniformity.

---

## 4.6.3 Mechanism analysis without bad controls

Chapter 4.6.2 told you *who*; this section asks *why* — through what channel. The single most common identification mistake in empirical work hides here, so state it plainly: **conditioning on a post-treatment variable to "isolate" a mechanism introduces selection bias and produces coefficients that do not estimate what their authors claim.** The Acharya–Blackwell–Sen (2016) critique formalizes why, and tells you what you *can* honestly do.[^abs2016]

The seductive-but-wrong recipe is **Baron–Kenny (1986)** mediation: regress $Y$ on $D$, then on $D$ *and* the candidate mediator $M$, and read the shrinkage in $D$'s coefficient as "the part that flows through $M$."[^bk1986] The problem is that $M$ is *post-treatment* and almost always shares unobserved determinants with $Y$. Draw the structural DAG: $D$ randomized, an unobserved $U$ causing both $M$ and $Y$, with $M$ a child of $D$ and $U$, and $Y$ a child of $D$, $M$, and $U$. Conditioning on $M$ does two things — it correctly blocks the indirect path $D\to M\to Y$ (good), but it *opens a collider path* $D\to M\leftarrow U\to Y$ (bad), because $M$ is a collider and conditioning on a collider unblocks it. A one-cell simulation makes it visceral: with a true direct effect of 1.0 and a true indirect effect of 0.5 (total 1.5), even with $D$ randomized and $N=20{,}000$ the Baron–Kenny "direct effect" comes back near 0.66 and the implied mediation share near 56% against a truth of 33% — off by nearly a factor of two. *And in observational finance, $M$ always shares unobserved determinants with $Y$.*

```python
# nb4.6 cell 3 — the bad-controls collider, in eight lines.
import numpy as np, statsmodels.api as sm
rng = np.random.default_rng(7); N = 20_000
U = rng.standard_normal(N); D = rng.binomial(1, 0.5, N)
M = 0.5*D + 0.8*U + 0.3*rng.standard_normal(N)          # mediator: child of D and U
Y = 1.0*D + 1.0*M + 0.8*U + 0.3*rng.standard_normal(N)  # true direct effect of D = 1.0
print("total :", sm.OLS(Y, sm.add_constant(D)).fit().params[1].round(3))            # ~1.5  (clean)
print("BK dir:", sm.OLS(Y, sm.add_constant(np.c_[D, M])).fit().params[1].round(3))  # ~0.66 (biased!)
```

ABS offer a way out. Their estimand is the **controlled direct effect**, $\text{CDE}(m)=\mathbb{E}[Y(d{=}1,m)-Y(d{=}0,m)]$ — the effect of $D$ if the mediator were *held fixed* at $m$ for everyone — because it needs the weakest assumptions of the mediation estimands and is what most policy questions want. Their **sequential-g estimator** avoids the collider by *de-mediating* the outcome rather than conditioning on $M$. Step 1: estimate $\hat\delta$, the effect of $M$ on $Y$, from $Y_i=\alpha+\gamma D_i+\delta M_i+\boldsymbol{x}_i'\boldsymbol\theta+\boldsymbol{z}_i'\boldsymbol\phi+u_i$, where $\boldsymbol{z}$ are post-treatment intermediates on other channels. Step 2: form the de-mediated outcome $\tilde Y_i = Y_i - \hat\delta\,(M_i - m^{*})$ and regress it on $D$ and *pre-treatment* controls only, $\tilde Y_i = \alpha+\tau D_i + \boldsymbol{x}_i'\boldsymbol\theta+e_i$, reading $\hat\tau$ as the CDE. The mediator is never on the right-hand side of the $D$-coefficient equation, so the collider is never opened; inference is by cluster bootstrap. The price is **sequential ignorability** — no unobserved confounder of $M$ and $Y$ once you condition on $(\boldsymbol{x},\boldsymbol{z})$ — which is *very* strong in observational finance (lenders who adopt training likely adopt other unobserved practices too). So the honest report names the assumption and pairs the CDE with a sensitivity analysis (Oster $\delta$; Cinelli–Hazlett): *"Sequential-g delivers the CDE under sequential ignorability; we show the implied mediation share stays in the 40–60% range across plausible violations."*

Three more-defensible modes often beat sequential-g because they ask less. **Heterogeneity-as-mechanism** (the cleanest): if a *pre-treatment* moderator that proxies the channel predicts the largest effects, the heterogeneity *is* the mechanism evidence — Maya's intensity dose-response is exactly this, identified under the *same* assumption as the headline, no extra machinery. **Instrument-for-mediator**: an instrument $Z$ that shifts $M$ but is excluded from $Y$ gives a causal $M\to Y$ effect via 2SLS — clean but the exclusion restriction is rarely credible. **Design-based mechanism**: a separate quasi-experiment that moves only one channel — the gold standard, rarely available. The real-world template is **Gao, Han, Kim & Pan (2024)** on overlapping institutional ownership, whose mechanism section leans on heterogeneity-as-mechanism (the coordination effect is stronger when the overlapping owner holds a board seat at both firms, and when it is an active rather than a passive investor) and never runs a Baron–Kenny regression.[^ghkp2024] The mature judgment is that many papers are stronger doing *no* mechanism analysis: if no channel can be honestly identified, **"we leave the channel question to future work" beats a Baron–Kenny regression that pretends.**

---

## 4.6.4 External validity and transportability: would this replicate?

Your estimate is internally valid — it survived the placebo battery, the multiple-testing adjustment, the heterogeneity and mechanism checks. The number is real *in the data you have*. The last question — "would this replicate in 2030?" — is not about internal validity at all. **External validity is identification all over again: different threats, different remedies, the same discipline of stating an assumption and saying what you would see if it broke.**

The first thing to kill is the instinct that a huge $N$ settles it. **External validity is about *transport* error, not *sampling* error.** With $N=47{,}000$ county-years your standard error around the average effect *for this population* is tiny, and a billion observations would shrink it further — but none of that closes the gap between the *source* population (U.S. county-years, 2008–2024) and the *target* population a policymaker cares about (county-years in 2030, in a different demographic, technological, and regulatory regime). That gap is structural. This is the Lalonde (1986) gap and the Manski (1990) partial-identification problem: the right answer to "would this replicate?" is never "yes, my SE is small," but *"conditional on these specific features of the 2026 setting also holding in 2030, yes; if they change in the following ways, the effect changes as follows."*[^lalonde1986][^manski1990]

Four threats, paralleling the internal-validity threats of the identification memo. **Sample–population mismatch**: the source covariate distribution differs from the target's (diagnose with a standardized-mean-difference table). **Period non-stationarity**: the effect is a function of the era — examinations in post-crisis 2010 may differ from 2030 under AI underwriting (diagnose by checking whether the per-year ATT is stable across your window). **Institutional drift**: the institutions that *mediate* the effect change — if federal pre-emption weakens state examination authority, the channel the effect runs through may not exist in 2030 (diagnose by listing the load-bearing institutions from your mechanism analysis and asking which survive). **Non-transporting heterogeneity**: even if average effects coincide by luck, the *distribution* of effects across subgroups may not transport if source and target differ on your moderator.

The formal organizer is the **transportability diagram** (Pearl & Bareinboim 2014; Bareinboim & Pearl 2016): a DAG with a selection node $S$ ($S{=}1$ in source, $0$ in target) that points into every variable whose distribution *differs* across populations.[^pb2014][^bp2016] **The effect transports if every path from $D$ to $Y$ avoids the selection-dependent variables — or can be closed by conditioning on them.** Maya's diagram has $S$ pointing into pre-treatment covariates $X$ and the institutional ecosystem $I$; the effect transports if she can re-weight the source so its $X$ and $I$ distributions match the target's. The common failure is **outside-the-support**: if 2030 features AI underwriting at scale, absent from the 2008–2024 source, *no re-weighting can manufacture an estimate there*, and the honest report says exactly that — which is a strength, telling the reader precisely where you cannot speak.

The workhorse tool is **inverse-probability transport re-weighting**. Stack source and target, fit a logit of $S$ on covariates to get the source-membership probability $\hat\pi(\mathbf{X}_i)$, and weight each source unit by

$$ w_i = \frac{1-\hat\pi(\mathbf{X}_i)}{\hat\pi(\mathbf{X}_i)}, \qquad \widehat{\text{ATT}}_{\text{target}} = \frac{\sum_{i\in\text{src-treated}} w_i\,\hat\tau_i}{\sum_{i\in\text{src-treated}} w_i}, $$

with $\hat\tau_i$ the per-unit CATE from the causal forest of §4.6.2. The catch is that you need target data on $\mathbf{X}$; for a *known* near-target — "states not yet adopted as of 2024" — that distribution is observable and the re-weighting is clean, which is why most papers transport to a near-target rather than to a hypothetical 2030. Watch the **positivity** diagnostic: if a few transport weights exceed ~10, a handful of source units are carrying the whole target and the estimator's variance explodes; the honest fix is to *truncate* to common support and state which region of the target you cannot reach.

```python
# nb4.6 cell 4 — transport ATT from adopting states (source) to not-yet-adopted (near target).
import numpy as np, statsmodels.api as sm
stacked = pd.concat([source.assign(S=1), target.assign(S=0)], ignore_index=True)
prop = sm.Logit(stacked["S"], sm.add_constant(stacked[Xcols])).fit(disp=0)
source["pi"] = prop.predict(sm.add_constant(source[Xcols]))
source["w"]  = (1 - source["pi"]) / source["pi"]          # up-weight source units resembling target
src_t = source[source["exam"] == 1]
print("source ATT :", src_t["tau_hat"].mean().round(3))
print("target ATT :", np.average(src_t["tau_hat"], weights=src_t["w"]).round(3))
```

The full **"would this replicate in 2030?" stress test** is four steps: *specify the target precisely* (not "2030" but "U.S. county-years under the Census medium-fertility projection with the 2024 institutional status quo"); *diagnose the covariate gap*; *re-weight* and report the transported estimate beside the source estimate with the positivity diagnostic; and *add institutional drift* as a substantive, non-statistical judgment ("by 2030 this institution is expected to be weakened because…; if weakened, the effect shrinks"). The deliverable is a *paragraph*, not a number — the external-validity equivalent of the threats-and-responses table. Aim for **Class 2 — bounded transport** ("we transport to a near-target with explicit re-weighting and decline to extrapolate to far-targets where the institutional ecosystem is unstable"): Class 1 (local validity only) looks like flinching, Class 3 (a full structural model) is overreach for a reduced-form paper. The most rigorous papers even **pre-register a replication prediction** for an identifiable future population (Camerer et al. 2018), which someone can test by re-running your code in 2030 — high cost, high credibility.[^camerer2018]

---

## 4.6.5 The thread through all four

Step back and the four tools are one discipline wearing four costumes. Multiple testing keeps you honest about *how many bets you placed*. Heterogeneity asks *for whom* the bet paid off, with pre-registration as the guard against fishing. Mechanism asks *through what channel*, with the bad-controls collider as the trap to avoid and sequential ignorability as the assumption to name. External validity asks *where the bet still pays*, with the transportability diagram naming exactly which features must carry over. Every one of them follows the Week 4 reflex you have now drilled five chapters deep: **name the threat, name the assumption that addresses it, separate what the data can test from what you must argue, and bring the diagnostic that shows where the result is fragile.** A referee who asks all four questions is not being hostile; they are reading your paper the way you should have read it first.

---

## Your Turn

In a fresh notebook — reusing the staggered-DiD panel you built for Week 4 (`nb4.2`) — work Maya's robustness suite end-to-end. Working from her staggered fair-lending panel, you will (1) **count the family** — assemble every test in a mock results section (heterogeneity rows, multiple outcomes, placebos), then apply Holm, BH, and a hand-rolled Romano–Wolf step-down, and watch how many "significant" findings survive *after adjustment*; (2) **estimate heterogeneity three ways** — pre-specified subgroup regressions, a continuous-moderator interaction with a delta-method band, and an honest causal forest, reading the CATE histogram and the BLP test, then deliberately under-power the BLP test to see a false "no heterogeneity"; (3) **break a mediation regression on purpose** — run the §4.6.3 bad-controls simulation, confirm the Baron–Kenny "direct effect" is biased, then recover the controlled direct effect with sequential-g and a cluster bootstrap; and (4) **transport the estimate** — re-weight Maya's source ATT to the not-yet-adopted states, inspect the distribution of transport weights for a positivity violation, truncate to common support, and write the four-sentence external-validity paragraph.

Before you start, make sure you can answer these:

1. **Multiple testing.** Maya reports twelve positively-correlated heterogeneity cuts. (a) Why is $1-(0.95)^{12}$ the wrong literal FWER here, and in which direction does the true family-wise error differ? (b) Order Holm, Romano–Wolf, and BH by number of rejections, and explain *why* that ordering holds when the tests are correlated. (c) Which of the four procedures would you put on the paper's *headline* test, and why is BH the wrong choice there?

2. **Heterogeneity & mechanism.** (a) Maya finds effects of $-0.4$, $-1.5$, $-3.1$pp across light/moderate/intense examinations. Why is this monotone dose-response stronger identification evidence than a single subgroup contrast? (b) A classmate "isolates the training channel" by adding training intensity to the outcome regression and reporting the shrunken $D$ coefficient. Draw the DAG, name the path that conditioning opens, and state which estimand sequential-g recovers instead. (c) Why does heterogeneity-as-mechanism need *no* assumption beyond the headline's, while sequential-g needs sequential ignorability?

3. **External validity.** (a) Maya's $N$ is 47,000 county-years. Explain in one sentence why that does *not* answer "would this replicate in 2030?" (b) In her transportability diagram, the selection node $S$ points into the institutional ecosystem $I$, and 2030 features AI underwriting absent from her source. Why can no re-weighting produce an estimate for that target, and what is the honest report? (c) Distinguish a Class 2 from a Class 3 external-validity claim, and say which she should make.

---

**Cross-references.** This chapter compresses the Week-4 robustness suite that earlier editions spread across a standalone week; it builds on the DiD/event-study foundation of Chapter 4.1 (parallel trends, clustered inference, pre-trend tests) and the staggered-adoption ATT of Chapter 4.2 (the Callaway–Sant'Anna estimate that is Maya's headline), and applies equally to the RD (4.3), synthetic-control (4.4), and shift-share (4.5) designs. For the write-up, see **Appendix D, Style Guide — §D3 "Robustness sections"** (`book/appendices/D-style-guide/D3-robustness-sections.md`) for how to table adjusted p-values, report a heterogeneity figure, frame a sequential-g sensitivity analysis, and write the external-validity paragraph; and **§D2 "Reporting regressions"** for the adjusted-p-value column convention. The lab `lab4-hmda-did.md` and the Week-4 notebooks carry Maya's panel end-to-end.

---

[^anderson2008]: Anderson, M. L. (2008). "Multiple Inference and Gender Differences in the Effects of Early Intervention: A Reevaluation of the Abecedarian, Perry Preschool, and Early Training Projects." *Journal of the American Statistical Association* 103(484):1481–1495.

[^rw2016]: Romano, J. P. and Wolf, M. (2016). "Efficient computation of adjusted p-values for resampling-based stepdown multiple testing." *Statistics and Probability Letters* 113:38–40.

[^gl2014]: Gelman, A. and Loken, E. (2014). "The Statistical Crisis in Science." *American Scientist* 102(6):460.

[^wa2018]: Wager, S. and Athey, S. (2018). "Estimation and Inference of Heterogeneous Treatment Effects Using Random Forests." *Journal of the American Statistical Association* 113(523):1228–1242.

[^atw2019]: Athey, S., Tibshirani, J., and Wager, S. (2019). "Generalized Random Forests." *Annals of Statistics* 47(2):1148–1178.

[^cddfv2018]: Chernozhukov, V., Demirer, M., Duflo, E., and Fernández-Val, I. (2018). "Generic Machine Learning Inference on Heterogeneous Treatment Effects in Randomized Experiments." *NBER Working Paper* 24678.

[^abs2016]: Acharya, A., Blackwell, M., and Sen, M. (2016). "Explaining Causal Findings Without Bias: Detecting and Assessing Direct Effects." *American Political Science Review* 110(3):512–529.

[^bk1986]: Baron, R. M. and Kenny, D. A. (1986). "The moderator-mediator variable distinction in social psychological research: Conceptual, strategic, and statistical considerations." *Journal of Personality and Social Psychology* 51(6):1173–1182.

[^ghkp2024]: Gao, L., Han, M., Kim, S. T., and Pan, X. (2024). "Overlapping institutional ownership, common ownership, and corporate strategy." *Journal of Corporate Finance* 84:102520.

[^pb2014]: Pearl, J. and Bareinboim, E. (2014). "External Validity: From Do-Calculus to Transportability Across Populations." *Statistical Science* 29(4):579–595.

[^bp2016]: Bareinboim, E. and Pearl, J. (2016). "Causal inference and the data-fusion problem." *Proceedings of the National Academy of Sciences* 113(27):7345–7352.

[^lalonde1986]: LaLonde, R. J. (1986). "Evaluating the Econometric Evaluations of Training Programs with Experimental Data." *American Economic Review* 76(4):604–620.

[^manski1990]: Manski, C. F. (1990). "Nonparametric Bounds on Treatment Effects." *American Economic Review* 80(2):319–323.

[^camerer2018]: Camerer, C. F. et al. (2018). "Evaluating the replicability of social science experiments in Nature and Science between 2010 and 2015." *Nature Human Behaviour* 2:637–644.
