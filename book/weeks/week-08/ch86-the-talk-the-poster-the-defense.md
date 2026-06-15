# Ch 8.6 — The Talk, the Poster, the Defense

In Chapter 8.4 you defended a result across a desk: a peer wearing a referee hat found your draft's weak points, and you answered in writing, with time to think. That was the private defense. This chapter is the public one. At the camp's conference presentation — Phase 2 of NextGen, around mid-August — you have eight minutes to make one argument land in front of a room of strangers, then nine more where the room finds out whether you *believe* the result or merely recited it. Then you stand by a poster for ninety minutes, defending in a different medium. Then you go home and, in twenty-four hours, catch everything the room gave back before it perishes.

Here is the move the chapter turns on. **The talk, the poster, and the question period are three projections of the same argument you already built — and the defense is not improvisation, it is reading from a script you authored in private, when you were calm.** The questions you will get were written down weeks ago: they are the rows of your threats-and-responses table from Chapter 7.5. The figures on your slides and poster are the ones your code already generated for the packet (Chapter 8.5). What is new here is *delivery under live pressure*. This chapter goes deep on those crafts; for the slide-by-slide table-to-figure catalog and the `make all` one-click replication packet, see Chapter 8.5, which this chapter complements rather than repeats.

---

## 1. The 72-hour pre-flight: stop improving, start hardening

Three days out, the nervous instinct is to keep *improving the paper* — one more robustness check, one more sentence in the abstract. That instinct is wrong. **At T-minus-seventy-two hours, you stop improving and start hardening.** The work is done; the job now is to make sure nothing about it *breaks* between now and the moment the room sees it. The pre-flight is how you find out, in private with three days to fix it, what would otherwise ambush you in public with zero seconds. Pilots have a saying that applies verbatim: *the airplane does not care how many hours you have on the type.* Run the checklist.

**Freeze the deck.** Cut the slides into a file you commit, tag `deck-freeze`, and do not edit again; the deck is locked, the delivery is open. The reason is not superstition: **a talk is a motor skill, and motor skills compile slowly.** Every time you change a slide, the times you rehearsed the transition into it become noise — your mouth has memorized a sentence that no longer matches what your eyes will see. Sam rewrote his momentum talk's headline slide three times in the last week and on the day stumbled into a version-one sentence while looking at version three; the stumble cost eight seconds, two slides' worth.

```bash
# 72-hour mark: freeze the deck.
make slides                                  # rebuild PDF from the same env as Lab 8
sha256sum paper/talk.pdf > paper/talk.pdf.sha256
git add paper/talk.pdf paper/talk.pdf.sha256
git commit -m "deck-freeze: 8-minute talk, conference vN.0"
git tag -a deck-freeze -m "Frozen for $(date -I) conference"
git push origin main deck-freeze
cp paper/talk.pdf ~/conf-backup/talk.pdf     # USB + cloud copy live OUTSIDE the repo
```

The freeze is a discipline of *visibility*, not silence: if you find an actual error in the paper between freeze and conference, you fix the paper, rebuild the deck, retag `deck-freeze-v2`, and announce the change at the start — you do not silently slip it in. The `deck-freeze` tag is the same kind of commitment device as the `pap-filed` tag of Chapter 7.3, applied to delivery instead of analysis.

**Audit the packet on a machine that is not yours.** The standard is from Chapter 8.5 and Appendix D — a stranger runs one command and watches every table and figure regenerate. In the pre-flight you become that stranger, in three steps: (1) `make clean && make all` on your own machine, daily, to catch the dumb stuff; (2) a clean `git clone` into `/tmp` with a fresh env from `environment.lock.yml`, to catch the "worked because of something my machine had" failures; (3) a peer on a different OS who runs `make all` and reports back. Maya did step three and found her `make all` had silently skipped the data pull for a month, relying on a stale cached parquet — at T-minus-sixty hours, not under the spotlights. If the fresh-clone PDF's SHA does not match your committed one, you have non-determinism (a missing seed, a timestamp in a caption, a `latexmk` version skew), and you kill it before Saturday.

**Build the anticipated-questions matrix.** This is the load-bearing pre-flight artifact: it turns Chapter 7.5's threats table into a script you read under pressure. The reveal — **the questions you will get on Saturday were already written down, by you, weeks ago.** One row per anticipated question, columns in the order you say the answer:

| col | name | what goes here |
|-----|------|----------------|
| 1 | **Question (surface form)** | As a hostile-but-competent attendee would phrase it. |
| 2 | **What it's attacking** | The threat-ID from your Ch 7.5 table, so the lookup is mechanical. |
| 3 | **Why it's fair** | One sentence granting the worry. *"Yes — that's the natural worry, because…"* |
| 4 | **What you did** | The specific test or design choice. *"We checked X; slide N / Appendix B.2."* |
| 5 | **The residual concern** | What you still cannot rule out, with a bound. |
| 6 | **The retreat position** | If the critique is fatal and right, what claim survives? |

Write twelve to twenty rows: one per threat, plus the predictable methodological questions (clustering, sample, fixed effects, bootstrap seed), three "so what" and three "generalization" questions, and one *trap* — the question that, if the attacker is right, is *fatal*. Tag each `survivable` or `fatal-if-right`, and **rehearse the fatal rows out loud**, because column 6 — the retreat sentence — is the hardest thing in the talk to say. Finish with a twenty-minute pre-mortem in prose: *imagine the talk went badly; name three causes and which you can still fix.* The usual culprits each have a pre-day fix — the projector mangled the PDF (bring it on USB and cloud, arrive early, test the aspect ratio); you ran out of time (pre-decide the cut line); the adapter did not fit (bring the charger and both video adapters). Then sleep: five rehearsals on seven hours' sleep beat ten on four.

---

## 2. The eight-minute talk, decomposed

Eight minutes is not a short version of a long talk; it is a different object with room for *one* idea, defended cleanly. The arc is six beats, one slide each, roughly a minute per slide: **hook → question → design → identification punchline → headline result → threats → contribution + ask.** Two rules govern each slide — *one idea per slide* (if you cannot write the idea as the slide's full-sentence title, it is doing too much) and *translate every table into a figure* (a table is for a reader who can stop and study; a figure is for a listener who cannot; the catalog is in Chapter 8.5 §2). Here we walk the *microstructure* of each beat.

**The hook is the first sentence, not a slide title** — the most expensive sentence of the talk, since the room decides whether to listen in the first ten seconds. It contains a number or a name, not an abstraction, because the room *cannot argue with a number yet*; it is a fact to be explained. Sam's, after three drafts: *"If the first thirty minutes of trading on the NYSE go up, the last thirty minutes go up — and you can bet on this, every day, for thirty years, and it still pays. We don't know why."* Twelve to twenty words, one breath; write it for the nervous mouth, which produces short sentences.

**The design slide names the comparison in one sentence, with one small picture — never the regression equation.** Nobody reads $\Delta\mathrm{Vol}_{i,t} = \alpha_i + \gamma_t + \beta\,\mathrm{ETF}_{i,t} + \mathbf{X}_{i,t}'\boldsymbol{\delta} + \varepsilon_{i,t}$ from row eight in sixty seconds. The slide gets the *sentence version* — Devon's: *"We compare a token's volatility before and after its spot ETF launches, against similar tokens that never got an ETF, in the same month."* That sentence *is* the equation, decoded for ears. The one piece of notation that travels is the *unit of variation* — a small grid showing what is compared to what, with what absorbed by fixed effects.

**The identification punchline is the load-bearing slide.** It is not a *statement* that the design works; it is *evidence*, compressed to nine seconds: flat pre-trends on the event-study plot for a DiD; the first-stage F plus a one-sentence exclusion argument for an IV; the visible jump plus a manipulation check for an RD; the institutional detail that gives you variation for a natural experiment. The common failure is *hedging* — "we *hope* parallel trends hold." The honest verb is *to demonstrate* what you can demonstrate and *to argue* what you can argue, but never *hope*, which the room hears as a confession. Test it: ask a peer who has not read the paper to listen to the design and punchline slides and tell you what assumption your estimate depends on. If they cannot, the question period fails at exactly that point.

**The headline result is one picture, not a table** — one estimate, one interval, one sentence in human units ("1.8 percentage points, about a fifth of the baseline gap," not "$\hat\beta = 0.018$, $t = 3.2$"). The interval is part of the result, not an afterthought. The figure is the *same file* the paper's LaTeX includes — generated by the analysis code, seeded from `config.py`, never re-drawn, because drawing it twice risks the divergence Chapter 8.5 forbids. After you say the number, let it sit: five to ten seconds of silence, which is hard the first time, so rehearse it.

**The threats slide is a strike, not a defense.** A defensive slide ("here are some specs, in case you ask") projects anxiety. A strike says: *"we tested the three choices a careful reader would worry about — clustering, sample restriction, fixed effects — and the result moves by less than half a standard error in every case."* The room hears that you thought about it harder than the questioner is about to, and you have *used up* a hostile question with a slide. The mechanism is the specification curve from Chapter 8.1 — a tight cluster the eye reads as a shape. The rule: **do not show robustness checks the result does not pass**; a failed check belongs in the paper and in your honest answer to "did you check $X$?", not on a slide.

**The contribution slide closes with what the room now knows, what you are *not* claiming, and one specific *ask*** — one concrete italic sentence: a methodological ask ("if you've worked with HMDA on fair-lending questions, I'd like to ask in Q&A whether the within-credit-band matching is doing what I think it's doing"), a data ask, or a collaboration ask. It is not "I'd love any feedback," which the room hears as a pleasantry; it is named at the granularity where the right person self-identifies.

Behind the six live slides you carry *backup slides* — the full table, the full event-study plot, the placebo — labeled so you can navigate to them. **The backup is what you *could* say, not what you *will* say.** Rehearse three times against a timer, target 7:30, cut from the motivation (where the time leak always is), never from the design. Record yourself and watch three tells: rising intonation at the end of declaratives, "um" more than once per slide, and looking at the slides instead of the camera.

---

## 3. Handling Q&A: the three honest answers

The talk tests whether you can *present* the work; the questions test whether you can *defend* it, by a harder standard. The reveal: **there are three honest answers to any hostile question, and a great defender uses all three on the same afternoon — "I don't know," "It's in the paper," and "Good point — and here's what I did about it."** The amateur uses only the third because it sounds smartest; the professional knows the first two are how you *budget your credibility* across nine minutes of incoming fire.

Before the answer comes the *read*. Questions arrive with a half-second of pre-context: *who* is asking (a data person asks about construction, a methods person about threats, an outsider about generalizability); the *tone* before the content (curious rising, skeptical descending, hostile flat); and **the first noun**, the question's target — "*Your standard errors…*" attacks inference, "*Your control group…*" attacks the comparison, "*The mechanism…*" asks for the story. By the time the sentence ends, your hand should be at the backup slide. Then *pause* — one breath, invisible to the room, long enough to classify the question into a bucket:

| bucket | kind | the script |
|--------|------|-----------|
| A | number-choice | robustness pointer + spec curve ("within 0.3 SE; Appendix B.2") |
| B | identification-attack | "Good point": fair-did-residual, possibly retreat |
| C | mechanism / why | mechanism sentence + two pieces of evidence |
| D | generalization | what data / what mechanism / what prediction / what test |
| E | motivation / so-what | stakes sentence + concrete consequence |
| F | data construction | technical precision: merge rate, drop reasons, appendix pointer |
| G | methodology-curiosity | parsimony: the simplest estimator the design supports |
| H | out-of-scope | gracious redirection to the hallway |

Classification takes one second; the answer follows from the script. Now the three honest answers, matched to their buckets.

**"I don't know" — the *located* version.** Admitting ignorance *honestly* — located, structured, useful — *builds* credibility, because the rest of your "I know"s become trustworthy by contrast. Four parts, in order: the admission plain ("I don't know," first, before any qualifier); the locator ("that's outside what my design can speak to, because…"); the directional guess, flagged as a guess; the resolution ("what I'd want to do is…"). Maya, asked whether her fair-lending effect extends to auto loans: *"I don't know. My data is mortgages, and auto lending falls under a different supervisor, so I have no reason to think the mechanism applies the same way. My guess is the effect is smaller because scrutiny is lower — but I wouldn't bet on the sign without re-running it on the comparable CFPB auto-loan data."* More credible than a confident "yes" (a bluff) or "I can't comment" (a refusal). Use for Buckets D and H.

**"It's in the paper" — the gracious deflection.** For fair questions that would eat your Q&A budget if answered live. Three parts: a one-sentence partial answer; a *specific* pointer ("Appendix B.3, Table B6," never just "it's in the paper"); the open door ("happy to walk through it afterward"). Sam, asked "why don't you control for size?": *"We do — column 3 of Table 3 includes size as a Fama-MacBeth control, and the coefficient is unchanged. Happy to walk through it afterward."* The room is satisfied; the deep dive is offered to the questioner. Use for Buckets A, F, G — **never** for B, D, or E, which must be defended live.

**"Good point" — the prepared sub-answer.** What you say for a Bucket B identification attack when you have the matrix row ready. It is a marker — *I am about to read from the matrix* — not a pleasantry. The structure is *fair-did-residual-retreat*. Priya, on whether her wildfire result is just declining property values: *"Fair question — exposed counties are also disproportionately rural and losing population. We tested it two ways: the pre-treatment coefficients on slide 5 are flat for four years before the rating shock, and dropping counties with population decline above 5% per year (Appendix Table A.4) leaves the coefficient within 0.2 standard errors of the headline. What I can't rule out is a county-specific shock coincident with the re-rating. If you don't buy the causal reading, the descriptive finding — insurer concentration collapses within two years of a re-rating in high-fire counties — is robust on its own and worth the policy attention."* The visible structure is itself a credibility move: the room hears that you have a system for exactly this question.

And the hardest case: **when a fatal critique lands and the attacker is right.** A fatal critique breaks the *identifying assumption itself* (Chapter 8.5 §4), not a choice in the analysis. The wrong response is to defend harder — the room can *see* it, and what they see is a speaker who knew the critique was right and chose to obscure it; the cost is not the broken design but the broken integrity. The right response is to concede at the level of the critique and retreat to what survives: *"You're right that I can't rule that out with this design, so the honest reading is a strong association consistent with the mechanism, not the clean causal estimate I'd hoped for."* That is the single most credibility-building thing you can say in the question period, and you retreat not to nothing but to the *descriptive* claim — which column 6 pre-wrote so you read it under fire rather than invent it.

After the last question, do *not* sit down and check your phone. Stay near the front; the three people who approach in the next ninety seconds — a technical follow-up, a paper to recommend, a contact — are the proximate ROI of the talk. Write what you heard on the paper notebook in your pocket; the feedback ledger (§5) starts now, not at 5 p.m.

---

## 4. The poster and the hallway conversation

The poster session opens after lunch and you stand by a 36-by-48-inch sheet for ninety minutes. The temptation is to treat the poster as *the talk, printed.* It is not. **The talk is a sequence; the poster is a layout.** The talk runs on rails. The poster is *spatial*: a viewer scans the whole surface in three seconds, decides whether to engage, and if they engage they *choose their own path*. The viewer-chosen path is the design constraint — every component must make sense *without* the others being read first. Design for the grazer; serve the reader only if asked.

The layout is six zones. The **headline band** (top six inches, full width, 60–72-point type) holds *one sentence* — the result, concrete before abstract — plus your name, the institution, and a QR code; this does 80% of the poster's work before the viewer steps closer. The **center figure** is the single largest object — the same slide-five picture, legible from six feet. Around them, four self-contained zones: **question** (top-left), **design** (middle-left, the identification sentence plus the unit-of-variation grid), **robustness** (bottom-left, the specification curve, the strike in spatial form), and **implications + ask** (right column). The rule: **a poster is not where you put more — it is where you put *less*, with bigger type and more white space.** Every failed poster fails by density; the eye reads density as "I do not have time for this" and walks on.

Because the viewer chooses the path, design so *any plausible eye path produces a coherent argument*: the methods reader jumps straight to robustness, so the spec-curve caption must stand alone (*"144 plausible specifications; the headline is within 30% of −1.8 pp; none crosses zero"*); the impact reader jumps to the right column, so the implication sentence must stand alone too. Borrow the *five-second test*: show the poster to five people for five seconds, take it away, ask what it was about; if three name the *result* — not the topic — the headline band works, else rewrite, then print. The figures are composed from the same `paper/figures/` files as the talk and paper, so the poster is a reformatting exercise, not new content.

The poster session is a *series of conversations*, and each viewer cues you, within five seconds, to which length they want. **The elevator pitch (15 seconds)** is question-finding-so-what, one sentence each, for "what's it about?" — the filter, not the conversation. **The one-minute version** is the six-beat arc compressed, for "tell me more," where you *point* through the poster beat by beat and it carries the argument while your voice narrates. **The five-minute version** extends the design and robustness zones, for the genuinely engaged. The discipline: *do not give a version the viewer did not ask for* — the elevator-pitch viewer is lost in the design zone ten seconds in. Read the cue; match the version; *pause* and let the viewer set the pace. Three physical habits make the poster work *for* you: *point* at the specific feature you describe so the conversation happens *in* the poster; *stand to the side*, about ninety degrees to its plane, so the viewer sees both you and the layout; and *make eye contact with the viewer, not the poster* — the hardest, because the poster is the natural place for your eyes to go.

The success metric is not foot traffic; it is *what conversations did I have, and what next steps did they generate?* Five people stopping with three named next steps beats fifty glancing past. The three useful conversations are the substantive critique (write it in the notebook with a next step), the lead (a paper, dataset, or person — Devon was pointed to a 2022 ether-liquidity working paper he had missed), and the collaboration thread (rarest, highest-value). Before sleep that night, send three follow-up emails; the conversion from contact to working relationship is roughly an order of magnitude higher inside forty-eight hours. The principle ties talk and poster together: **every artifact of the project is a different projection of the same argument, and the discipline is to keep the projections consistent.** The slide-five figure becomes the center figure; the threats slide becomes the robustness zone; the punchline becomes the design zone; the ask stays the ask. Get the *spine* right and every projection gets easier.

---

## 5. After the conference: the feedback ledger and triage

It is Saturday evening, you are depleted from performing credibility for nine hours, and you want to sleep. This is what you do *first*, and it decides whether the conference produced an outcome or merely an experience. The reveal: **conference feedback is a perishable asset.** Within twenty-four hours fifty percent is gone — *what* the critique was you may remember, *who* said it you probably do not, and *the exact framing that made it sting* you certainly do not, and the framing is where the operational signal lived.

There are two artifacts. The **feedback ledger** is a single CSV — one row per piece of feedback — with eight columns: `fid`, `when`, `who`, `verbatim`, `translation`, `bucket` (A–H, the same taxonomy as §3), `severity`, and `next_step`. The split that makes it survivable: **fill the first four columns Saturday evening, the last four Sunday morning.** Saturday's pass is pure transcription — quantity, not quality; a half-remembered verbatim beats no row — walking memory through the talk Q&A, the post-talk approaches, the poster conversations (from your pocket notebook), the overheard-and-indirect, and — the part most students skip — your own self-reflections. Devon's, that evening: *"I rushed the design slide and skipped the unit-of-variation grid I had practiced; the two questions that followed would not have been asked if it had landed."* That row names a delivery failure, connects it to a consequence, and seeds a rehearsal correction. By morning the verbatim column is half noise, so you *transcribe* tonight and *think* tomorrow.

Sunday morning, rested, you fill the last four columns. **Translation** decodes each row into the language of your paper; **bucket** tags it A–H; **severity** runs on decision rules, not vibes:

> **Critical** — if this critic is right, the headline doesn't survive. *A row is Critical if and only if the threat, confirmed, would break the identifying assumption itself* (the fatal/survivable line from Chapter 8.5 §4). Rare; most talks produce zero or one. Your post-symposium revision begins here.
>
> **Important** — the paper is meaningfully better if you address it, but it is not blocking. Most Bucket B rows already covered by robustness but not surfaced clearly, and most Bucket F documentation gaps.
>
> **Minor** — a single sentence, a citation, a paragraph to tighten. The polish layer.
>
> **Courtesy** — served a relational, not a paper-improvement, purpose. A personal thank-you, no paper-side action.

The rubric exists *precisely* to short-circuit the emotional reaction. The sequencing is cognitive: **severity reactions are biased by fatigue in the wrong direction.** A row that feels Critical at 9 p.m. Saturday becomes Important by 10 a.m. Sunday and Minor by Wednesday — *that cooling is not denial, it is calibration*, produced by the overnight memory-consolidation gap. Collapse Sunday into Saturday and you bake the over-reaction into your revision plan. A healthy distribution: 0–1 Critical, 4–8 Important, 8–15 Minor, 5–10 Courtesy. Every row's `next_step` is a verb, an object, and a *date*.

```python
# triage emitter: filter the feedback ledger to a four-week task board
import pandas as pd
df = pd.read_csv("paper/qa/feedback_ledger.csv")
todo = df[df.severity.isin(["Critical", "Important"])].sort_values(["severity", "next_step"])
todo.to_csv("paper/qa/revision_taskboard.csv", index=False)          # the input to your post-symposium revision
df[df.severity == "Courtesy"].to_csv("paper/qa/followups.csv", index=False)   # the Monday emails
```

The taskboard CSV is the bridge to revision: you walk into your first mentor meeting with *one document* and say *here is what I heard, here is how I sorted it, here is what I'm going to do* — turning the mentor's job from discovery into calibration. The deeper claim, the one the whole NextGen arc is pointed at: **research is a chain of hand-offs, and the artifact that survives each one determines the quality of the next.** This ledger is the survival of the feedback — the one hand-off that depends entirely on what you do *after* the event, in private, under fatigue, before the asset perishes.

---

## Your Turn

Three live-delivery deliverables, the same project seen from its three surfaces.

- **The pre-flight, run for real.** Three days before any talk: freeze the deck (tag, SHA, USB + cloud backup); run the three-step packet audit (own machine, fresh clone, peer); author the anticipated-questions matrix with at least one row per row of your Ch 7.5 threats table, severity-tagged, fatal rows rehearsed out loud; and write the twenty-minute pre-mortem in prose. The deliverable is the *audit trail*, not the talk.
- **The six-slide arc + a 7:30 dry-run video.** Build the six live slides plus title, acknowledgments, and backup; rehearse three times against a timer; record pass three. Grade it on the six beats — hook earns attention, design names the comparison, punchline lands, result picture replaces a table, threats slide strikes rather than defends, ask exists — not on production value. Then run a recorded ten-question adversarial Q&A and, for each, write the *bucket*, the *answer* (100–150 words), and which of the three honest answers you used.
- **The poster + the feedback ledger.** Compose the poster from the figures already in `paper/figures/` (reuse, not new figures), and pass the five-second test before you print. After the talk, build the ledger Saturday evening (first four columns) and run the triage Sunday morning (last four), emitting `feedback_ledger.csv`, `week_10_12_taskboard.csv`, and `followups.csv`. Send three follow-up emails inside forty-eight hours.

### A closing reflection

Pick the single Bucket B question you are most afraid of — the one where the attacker, if right, is *fatal* and not merely survivable. Write the full fair-did-residual-retreat answer in long form, on a piece of paper you keep in your pocket, and ask: *if I had to deliver the retreat sentence live, could I do it?* Then ask the question this whole camp was pointed at: *if a stranger clones my repo and types one command, do they get the number I just defended?* If yes, you are ready to walk into the room. If "almost," you now know exactly what "done" means — and the next chapter (Ch 8.7, submitting your paper to the Schar Young Scholars Journal and the GMU MARS repository) is where the conference's feedback ledger becomes the agenda of the final revision.

---

### References

- Gawande, A. (2009). *The Checklist Manifesto: How to Get Things Right.* Metropolitan Books. (Aviation pre-flight as a template for high-stakes delivery.)
- Klein, G. (2007). Performing a Project Premortem. *Harvard Business Review*, 85(9), 18–19. (The pre-mortem method.)
- McConnell, P. (2009). *The Craft of Scientific Presentations.* Springer. (The single-idea-per-slide rule; the title-as-sentence discipline.)
- Doumont, J.-L. (2009). *Trees, Maps, and Theorems: Effective Communication for Rational Minds.* Principiæ. (The structural arc of a technical talk.)
- Tufte, E. R. (2006). *The Cognitive Style of PowerPoint: Pitching Out Corrupts Within* (2nd ed.). Graphics Press. (The table-to-figure argument.)
- Hess, G. R., Tosney, K. W., & Liegel, L. H. (2009). Creating effective poster presentations. *American Biology Teacher*, 71(1), 9–17. (The five-second test; zone-based layouts; the grazer model.)
- Krug, S. (2014). *Don't Make Me Think, Revisited* (3rd ed.). New Riders. (The three-second decision model, applied to poster scanning.)
- Gao, L., & Sun, Y. (2019). Subjective Performance Evaluation, Influence Activities, and Bureaucratic Work Behavior: Evidence from China. *Proceedings of the National Academy of Sciences*, 116(19), 9293–9302. (Prof. Gao's policy testimony on this paper informs Mentor Session 8 — what changes when the room is policy, not academic.)
- Walker, M. P. (2017). *Why We Sleep.* Scribner. (Memory consolidation across the overnight gap; why the Saturday/Sunday split is cognitive, not merely logistical.)
- Cialdini, R. B. (2008). *Influence: Science and Practice* (5th ed.). Pearson. (Why the follow-up email within forty-eight hours converts at an order of magnitude higher rate.)
- See also Chapter 8.5 (the eight-minute talk and the one-click replication packet — the work this chapter's delivery protects), Chapter 8.1 (the specification curve — the threats slide and robustness zone), Chapter 8.2 (the robustness battery — the source of "we checked" answers), Chapter 7.5 (the identification memo and threats table — the seed of the anticipated-questions matrix), Chapter 7.3 (the `pap-filed` tag — the commitment device the `deck-freeze` tag generalizes), Chapter 8.7 (submission — where the feedback ledger becomes the final-revision agenda), and Appendix D (style guide and packet standard).
