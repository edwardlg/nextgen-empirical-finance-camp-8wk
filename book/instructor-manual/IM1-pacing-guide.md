# IM-1 — Pacing Guide

> **Companion documents.** This guide governs the *clock*. For how each deliverable is scored,
> see **IM-2 (Grading Rubrics)**; for the errors to watch as the clock runs, see **IM-3 (Common
> Student Pitfalls)**; for slotting outside speakers into the week, see **IM-4 (Guest Lectures &
> Mentor-Session Facilitation)**; for anchor work and answer keys, **IM-5**; for the access,
> compute, and licensing constraints that shape any schedule you build, **IM-6 (Equity/Access)**.
> All cross-references are by name so this manual survives reordering.

This camp has two clocks running at once, and the first job of any instructor is to keep them
straight. The first clock is the **8-week program clock** the textbook is built around — an intensive
edition that runs the whole arc, from a defensible number to a submitted paper, in eight weeks. The
second clock is the **NextGen calendar** the camp feeds (CONVENTIONS §8), whose Phase 1 is **seven**
weekly Friday meetings 12:00–2:00pm EST, with a conference presentation in mid-August and a
paper-refinement window after it. Two hours a week for seven weeks is roughly fourteen contact hours;
the camp's curriculum was built for closer to two hundred. The Friday schedule is therefore not a
trim of the curriculum — it is a re-architecture against an asynchronous backbone, and §3 below is
the concrete mapping that makes it work. Read §1 and §2 first so you know what you are compressing
*from*, and what the final two weeks are *for*.

---

## 1. The 8-week clock

The eight weeks are not eight parallel modules; they are one load-bearing arc, and the order is not
negotiable. The book that lives at `book/weeks/week-NN/` reflects this directly: Weeks 1–4 build the
toolkit and the robustness frontier, Weeks 5–6 teach reading the frontier, and Weeks 7–8 carry the
project build, the manuscript, the presentation, and the submission. The intensive edition folds what
a slower program would spread across a symposium week and a four-week revise-and-resubmit tail into
this eight-week spine; the long-form treatment of each compressed piece is **available on demand in
office hours**. An instructor who paces the back half as if it were the front half will exhaust the
cohort before submission; one who paces the front half too gently will arrive at Week 8 with nothing
to present.

### Weeks 1–4 — the toolkit and the robustness frontier

Weeks 1–4 build the toolkit from the ground up: probability and the logic of inference (W1), the OLS
engine and its standard-error pathologies (W2), causal inference from potential outcomes through
matching and instruments (W3), and the modern quasi-experimental toolkit — DiD, RD, synthetic
control, shift-share — together with the recent literature showing the obvious estimators are often
wrong (W4). Week 4 also carries the **robustness frontier** (ch46-robustness-v2): multiple-testing
corrections, heterogeneity/CATE, mechanism analysis, and external validity — the material a slower
program would defer to a post-conference revision, brought forward so robustness is a habit by the
time the student runs their own design.

### Weeks 5–6 — reading the frontier

Weeks 5–6 turn from *running* estimators to *reading* them: one empirical paper per day under a fixed
Reader's-Guide anatomy (W5), then text-as-data and a full module on using LLMs as a research co-pilot
without fooling yourself (W6). Each week earns the next. You cannot read a weak-instrument disaster
in Week 5 if you did not derive 2SLS in Week 3; you cannot stress-test your own DiD in Week 8 if you
did not meet the staggered-adoption crisis in Week 4. When you compress (see §3), you may *thin* a
week but you may not *reorder* the dependencies.

### Weeks 7–8 — build, write, present, submit

Weeks 7–8 are the build and the finish. **W7 — question to pre-analysis plan, then the manuscript
build**: the seven-slot specification, the pre-registered PAP, and — folded in via
ch76-manuscript-build — the paper itself drafted to publication standard (abstract, tables, figures,
introduction, literature review). **W8 — execution, robustness, presentation, and submission**: run
and break the pre-registered spec, then present it and ship it. Week 8 carries
ch86-the-talk-the-poster-the-defense (the conference talk, poster, and Q&A defense) and
ch87-submission-and-the-long-arc (the *Young Scholars Journal* portal, MARS deposit, SSRN preprint,
the conference circuit, and the post-submission long arc). A useful way to hold the arc in mind:
Weeks 1–4 answer "what is a defensible number?", Weeks 5–6 answer "how do I judge someone else's
number, including a machine's?", and Weeks 7–8 answer "can I produce, defend, and submit a defensible
number of my own?"

### The within-week rhythm — Weeks 1–7

Within a full curriculum week, the book assumes a five-day cycle with a stable **daily block
structure**. The block is the unit of pacing, and it repeats:

1. **Read the chapter** (one of the week's five), which follows the mandated reveal-the-trick
   structure from CONVENTIONS §1: state the result in a plain sentence, show *why* it works
   (intuition → worked numerical example → algebra), show *when it fails* (the assumption that
   breaks and what you see when it does), then show the runnable code. A chapter is roughly a
   60–90 minute read for the target student.
2. **Work the notebook** that mirrors the chapter (nbN.k), which re-derives the chapter's central
   move in code and ends with a "Your Turn" extension. Budget 45–75 minutes.
3. **Do the problem set** (psN.k, ~6 problems). Solutions live in Appendix E; students should
   attempt before consulting. Budget 60–90 minutes.
4. **Lab / mentor as the week's spine.** The lab (Weeks 1–4, 7–8) or reading pack (Weeks 5–6) is the
   integrative artifact that ties the five chapters into one build. The 60-minute **Lei Gao mentor
   session** sits late in the week as the intellectual capstone, tied to one of his papers (see
   IM-4 for facilitation).

A typical curriculum day runs: morning chapter + notebook, afternoon problem set, with the lab
threaded across the back half of the week and the mentor session on day four or five. The end-of-week
**assessment** closes the loop on day five, graded against the week's rubric (IM-2). For a student
hitting the daily block, a curriculum week costs on the order of 25–35 contact-and-homework hours —
five chapters (≈6 h reading), five notebooks (≈5 h), five problem sets (≈6 h), the lab (≈4–6 h), the
mentor session (1 h + a pre-read), and the assessment (≈3–4 h). This is **full-time effective load**;
the residential cohort treats it as a workday. Week 7 carries an extra load — the manuscript build
(ch76) sits on top of the question-to-PAP arc — and instructors should budget toward the high end of
that range, or push the manuscript draft into the Week 7→8 weekend.

### The within-week rhythm — Week 8 (present and submit)

Week 8 dissolves the chapter-a-day rhythm. Its presentation and submission chapters
(ch86-the-talk-the-poster-the-defense, ch87-submission-and-the-long-arc) are best read in two
evenings, not five mornings, alongside the execution-and-robustness chapters. The week then runs as
**three rehearsal cycles** layered over the final analysis: a private dry-run (Mon), a peer dry-run
with the threats-table question bank (Wed), and a final dry-run with at least one faculty critic
(Thu), with the live presentation late in the week and the submission packet — `Young Scholars
Journal` portal, MARS deposit, SSRN preprint — assembled at the close. Mentor 8 sits Tuesday, when
students have just enough self-awareness about their own talk to learn from a professional's. The
presentation/defense load is weighted toward rehearsal and slide iteration; the submission load is
weighted toward the reproducibility rebuild (`make clean && make all`) and the deposit checklist.

The longer-form version of this stretch — a full symposium week and a four-week revise-and-resubmit
cycle (referee triage, second-round robustness, journal-style rewriting, a staged submission) — is
**available on demand in office hours** for students who want to keep refining after the eight weeks
close. It is not part of the graded eight-week clock.

---

## 2. How a week's pieces fit together

The seven components of a curriculum week are not interchangeable; each does a distinct pedagogical
job, and the compression in §3 works only if you know which jobs are sacrificable and which are
not. The opening narrative (~1,300 w) sets the week's question in a recurring-cast frame (Maya,
Devon, Priya, Sam, Leah) — Maya's loan-approval puzzle for causal inference, Priya's
climate-insurance shock for DiD; it is motivational and *cuttable to a paragraph* under compression.
The **chapters** carry the conceptual spine and are the least sacrificable content; under
compression you assign them as *pre-reading*, not as in-session reading. The **notebooks** are
where understanding is verified by production — highly compressible in session (demo one, assign the
rest) but never skippable entirely. The **problem sets** are graded practice; under compression
they collapse to one or two core problems per week (IM-5 flags which). The **lab / reading pack** is
the integrative artifact and the natural home of the week's deliverable, the piece most worth
protecting. The **mentor session** is the camp's signature, mapping cleanly onto a guest slot or a
live Friday segment (IM-4). The **assessment** is the graded checkpoint.

Week 8's pieces are different: the execution-and-robustness chapters plus the
present-and-submit chapters (ch86 talk/poster/defense, ch87 submission-and-the-long-arc), one mentor
(Mentor 8), three rehearsal cycles, a live defense, and the submission packet. The dependency that
most constrains scheduling across all eight weeks is **data access**: the labs in Weeks 2, 4, 5, and
the capstone all touch CRSP/Compustat/HMDA, which under CONVENTIONS §5 stay read-only on GMU
infrastructure. WRDS seats and Hopper compute must be provisioned *before* the week that needs them,
not during it — an IM-6 concern that drives pacing, because a week whose lab cannot run is a week
half-lost, and a Week-8 submission whose data pipeline breaks is a submission missed.

---

## 3. The 8 weeks against NextGen's Friday-only calendar

NextGen Phase 1 is **seven** weekly Fridays, 12:00–2:00pm EST, Jun 26–Aug 7. The book is
**eight** weeks of curriculum, so something has to give — but the give is on the *Friday* side, not
the curriculum side. The governing principle: **the seven Fridays cover eight weeks of
material by merging the two reading weeks (W5+W6) into one Friday and pushing all chapter reading
and most notebook execution to async between-Friday self-study**. The synchronous two hours buy what
recordings cannot: the mentor's live reasoning, a critique that responds to *this* student's error,
and the experience of watching a result break in real time.

### The seven curriculum Fridays

Each Friday is two hours. A reliable internal shape is: **0:00–0:20** debrief on the week's pre-work
and prior deliverable; **0:20–1:10** the mentor/teaching segment; **1:10–1:50** live work — a
critique circle, pair-coding clinic, or design workshop; **1:50–2:00** assign the coming week's
pre-work and deliverable. The seven Fridays map to the eight book-weeks like this:

| Friday | Date (2026) | Book content | Synchronous focus | Async between Fridays | Deliverable due |
|---|---|---|---|---|---|
| **F1** | Jun 26 | W1 — Probability & Inference | Mentor 1, *"What is a finding?"*; live size/power demo (Lab 1 core) | W1 chapters + notebooks | W1 mini-sim (size of a t-test) |
| **F2** | Jul 3 | W2 — OLS Engine | Mentor 2, *"Why your SE is the whole ballgame"*; FWL + clustered-SE clinic | W2 chapters + notebooks | W2 SE-choice mini-replication |
| **F3** | Jul 10 | W3 — Causal I (PO, matching, IV) | Mentor 3, *"Natural experiments"* (Deng, Gao & Kim 2020); weak-IV demo | W3 chapters + notebooks; Lab 3 | W3 weak-IV / AR-coverage task |
| **F4** | Jul 17 | W4 — Causal II (DiD/RD/SC) | Mentor 4, *"Detecting discrimination with a clean design"* (Gao & Sun 2019) | W4 chapters + notebooks; Lab 4 (HMDA DiD) | W4 staggered-DiD task; **first capstone idea memo** |
| **F5** | Jul 24 | W5 + W6 merged | Mentor 5 *"Anatomy of a JF paper"* + Mentor 6 *"AI without fooling yourself"*; Reader's-Guide walk | W5 reading pack + W6 Ch 6.5 + nb6.5 | Reader's Guide on unseen paper + one classifier OOS-validation report |
| **F6** | Jul 31 | W7 — Question → PAP → manuscript build | Mentor 7, *"How I pick a project — and kill one"*; identification-memo workshop | W7 chapters + notebooks; Lab 7 (stand up repo); ch76 manuscript draft | **PAP filed at the end of W7 — a `pap-filed` tagged commit** + manuscript draft |
| **F7** | Aug 7 | W8 — Execution, robustness, present & submit | Mentor 8, *"Defending a result"*; robustness clinic; **presentation-rubric introduced** | W8 chapters + notebooks; run pre-registered spec once; ch86 talk/poster; ch87 submission | **First-look frozen at W8 (tagged commit)** + robustness battery + draft talk |

Two structural moves make this fit. First, **W5 and W6 merge into one Friday (F5)**: both are
"reading the frontier," both replace a lab with a reading/AI pack, and both end-of-week assessments
are "produce one artifact and validate it." Second, **W7 carries the manuscript build and W8 carries
the present-and-submit work**: F6 freezes the design (PAP tag) and seeds the manuscript draft, and F7
freezes the first-look (results tag) and runs the talk, poster, defense, and submission. The
slower-paced symposium-and-revision arc is available on demand in office hours, not on the graded
Friday calendar.

Two critical milestones live inside this table and are worth restating, because everything
downstream depends on them holding. The **PAP is filed at the end of W7 (F6, Jul 31)**
as a `pap-filed` tagged commit; this is non-negotiable, because breaking the pre-registration freeze
voids the meaning of every Week-8 p-value. The **first-look results are frozen at W8 — at F7
(Aug 7)** with a `first-look-frozen` tag; everything after the freeze is robustness and write-up,
not exploration.

### The presentation-and-submission Friday (Week 8)

Book Week 8 is the public test and the finish. The **dry-run is Mon Aug 10**, the peer dry-run is
Wed Aug 12, the final dry-run with a faculty critic is Thu Aug 13, and the **live presentation is
Fri Aug 14**; the submission packet (`Young Scholars Journal` portal, MARS deposit, SSRN preprint)
is assembled across the back of the week, with the final journal submission landing by **Sep 11**.
Mentor 8 was delivered Tuesday Aug 11 asynchronously or in a 30-minute live drop-in. Grading uses
the **presentation rubric from Mentor 8** (introduced at F7) plus the Week-8 defense rubric in
IM-2 §5; weights are **40% talk craft, 30% defense, 20% slide architecture, 10% reproducibility of
the demo result**. The content-mastery rubric is **paused** for the presentation block — the cohort
is not learning new methods, they are defending the ones they have.

### Beyond Week 8 — the on-demand refinement arc

A slower-paced program would carry the cohort through a dedicated symposium week and a four-week
revise-and-resubmit tail (referee triage, second-round robustness, journal-style rewriting, a staged
submission with the final journal deposit landing Sep 11). In the intensive eight-week edition this
material is **folded into Weeks 4, 7, and 8** — the robustness frontier into Week 4, the manuscript
build into Week 7, the talk/poster/defense and the submission packet into Week 8 — and the long-form
treatment is offered **on demand in office hours** for students who want to keep refining after the
camp closes. It is not part of the graded eight-week clock, so no separate Friday calendar is
required for it; an instructor running the camp against NextGen simply supports interested students
into NextGen's own post-conference window.

### Grading rubric weights shift across the eight weeks

The rubric is not constant across the eight weeks, and instructors should make this shift explicit
to the cohort *before* it happens, not after a student is surprised by a presentation score. **Weeks
1–6 weight methods mastery and the produced artifacts** (the per-week assessments in IM-2 §3).
**Week 7 weights the design half** — the PAP, the identification memo, and the manuscript draft (the
research-design rubric, IM-2 §4a). **Week 8 weights the terminal capstone rubric** — execution,
robustness, write-up, reproducibility, and the presentation/defense (IM-2 §4b and §5). The terminal
grade leans on the capstone and presentation, because those are the only things the camp was
building toward; the IM-2 §1 weighting table states the blend.

### What gets cut, and what never does

Be explicit with yourself about the trade. Under the seven-Friday compression you will
**cut**: most in-session chapter reading (async), three of every five problem sets per week (keep
the highest-signal two — IM-5 flags them), and most in-session notebook execution (demo one, assign
the rest). You will **protect, without exception**: the dependency order of Weeks 1→4; at least one
mentor session per Friday; the W7 PAP-and-tag discipline (the pre-registration freeze is
non-negotiable); the W8 first-look freeze; and the W8 reproducibility standard (`make clean && make
all`). A compression that sacrifices any of these has not compressed the camp — it has cancelled it.

---

## 4. A pacing checklist for the instructor

Before the program starts: provision WRDS seats and Hopper/A100 compute for the weeks that need
them (W2 Fama–MacBeth, W4 HMDA DiD, W5 portfolio sorts, W6 AI module, W7–W8 capstone); run FM-4
(Prerequisite Self-Test) and triage; pin the environment (`python=3.11` + the CONVENTIONS §5 stack)
and confirm every notebook runs on a fresh env; stand up GitHub Classroom and the repo template so
the `pap-filed` and `first-look-frozen` tag workflows are ready by F6 and F7.

Each curriculum Friday: confirm the prior deliverable arrived and was graded against its rubric
(IM-2) before the session, so the debrief is specific; keep the synchronous two hours for what
cannot happen alone; assign the next deliverable explicitly with its rubric attached.

Week 8 (present and submit): schedule the **Mon Aug 10 dry-run** as a hard calendar event; confirm
the threats-table question bank is built before the peer dry-run on Aug 12; book the presentation
logistics (room, A/V, faculty critics) by F6 at the latest; protect the reproducibility check (it is
the single most objective row in the terminal rubric, and it is the MARS deposit standard); reserve
the close of the week for the actual submission, not for more analysis.

The clock is unforgiving precisely because the deliverable is real: these papers are headed for an
actual journal and an actual repository. Pace the camp so that the freezes hold (the W7 PAP, the W8
first-look), the presentation gets a real defense (Aug 14), the code rebuilds for submission, the
journal deposit lands by Sep 11, and every student can say the sentence the whole program exists to
let them say truthfully — *here is what I found, here is why you should believe it, and here is the
command that lets you check me.*
