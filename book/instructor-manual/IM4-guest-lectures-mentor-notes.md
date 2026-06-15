# IM-4 — Suggested Guest Lectures & Mentor-Session Facilitation

This section is about the two kinds of outside-the-chapter voice the camp leans on. The first is the
**guest lecture**: a working professional who visits once and gives the students a forty-five-minute
window into how the methods they are grinding through actually get used, fought over, and paid for.
The second is the **Lei Gao mentor session**, the sixty-minute weekly fixture that is *not* optional
color but a graded, structured part of the arc — one per week, each anchored to one of Prof. Gao's
own papers and tied forward to the capstone. IM-1 (pacing) tells you *where* in the week these land;
this section tells you how to run them so they earn their place. Read it alongside IM-2 (rubrics),
because the mentor sessions feed directly into the research-design and capstone rubrics, and IM-3
(pitfalls), because a guest who contradicts a chapter's framing creates exactly the kind of confusion
IM-3 warns about.

A standing principle for both: an outside voice is most valuable when it is *concrete and slightly
uncomfortable* — a real result that did not replicate, a regulator's question the data could not
answer, a deadline that forced a shortcut. Curate for honesty over polish.

---

## Part A — Suggested guest lectures and how to slot them

You do not need a guest every week, and you should not try; four across the eight weeks is plenty,
and each should land where the chapters have just made the visitor's world legible. The four profiles
below are chosen so that, together, they cover the arc from regulation to practice to product to
public communication — the full life of an empirical finding once it leaves the notebook. Treat the
week assignments as defaults; the binding constraint is always "what have the students just learned
enough to *interrogate* this person about?"

**A fair-lending regulator or SEC economist — slot in Week 4.** This is the most important guest of
the four because it lands the week the students study identification for discrimination detection,
the same week Mentor Session 4 anchors on Gao & Sun (2019) and Lab 4 builds a DiD on HMDA. A
practicing economist from a fair-lending unit, the CFPB, HUD, or an SEC division can do something a
chapter cannot: explain what it means for a coefficient to become *evidence in a proceeding* — the
documentation a regulator demands, the adversarial scrutiny a number survives in litigation, why
"residual disparity after controls" is necessary but not sufficient in a courtroom exactly as the
mentor pre-read argues it is not sufficient in a journal. Brief the guest to bring one real case where
the *design*, not the size of the gap, was what decided whether the agency could act. This guest also
de-abstracts the entire fair-lending thread that runs Weeks 4 through 6 and Capstone 1.

**A quant practitioner — slot in Week 2 or Week 5.** Someone from a hedge fund, an asset manager, or
a bank's modeling desk grounds the standard-error and replication material in money. Week 2 (the OLS
engine, the SE flavors) is the natural home if you want them to drive home Mentor Session 2's thesis
that "your standard error is the whole ballgame" — a practitioner who has watched a strategy die
because its backtest used the wrong inference is the most persuasive possible witness for clustered
and HAC errors. Week 5 (reading the classics — Fama–French, Jegadeesh–Titman momentum) works if you
want them to talk about how a published anomaly does or does not survive contact with transaction
costs and capacity. Either way, ask them to bring one *failed* idea: students hear plenty about
successes, and the failure is where the inference lesson lives.

**A FinTech founder — slot in Week 6.** Week 6 is text-as-data and the AI co-pilot module, and it is
where the camp's subject most clearly touches the world the students are about to enter through
NextGen's FinTech framing (see the Articulation Matrix). A founder of a lending, payments, or
alt-data startup can speak to algorithmic underwriting from the inside — which ties straight to the
Bartlett et al. (2022) fair-lending-in-the-FinTech-era reader's guide and to Ch 6.5's warnings about
look-ahead leakage and validating model outputs. Brief them to be candid about the tension between
moving fast and the disparate-impact exposure the students just learned to measure; that tension *is*
the lecture.

**A data journalist — slot in Week 7 or Week 8.** As students turn their own analysis into a paper
and an eight-minute defense, a journalist who covers finance or does data-driven investigations
teaches the skill the chapters undersell: communicating a quantitative finding to someone who will
not read the appendix. They can critique a student's headline sentence, show how a chart either
clarifies or misleads, and explain how they themselves decide a result is solid enough to publish —
a real-world echo of the capstone's "would this survive a hostile referee" standard, extended to "and
a skeptical editor." This guest pairs naturally with Ch 8.3 (writing) and Ch 8.5 (the talk).

**Logistics that make a guest worth the disruption.** Send the guest a one-paragraph brief naming the
exact chapters the students have finished and the two or three things you want them to *not* re-teach
(the students already have the mechanics; you want the war stories). Ask for thirty to thirty-five
minutes of talk and a hard fifteen of questions, and seed the Q&A: assign each student to arrive with
one written question tied to a specific chapter, collected beforehand so a quiet room never stalls.
Record with permission so absent or future cohorts benefit. If a profile is unavailable, a recorded
talk or a published interview plus a live student-led discussion is a fully acceptable substitute —
the goal is the outside perspective, not the logistics of a calendar.

**Protect the chapter framing.** The one failure mode to guard against is the guest who, in good
faith, contradicts something the chapters established — tells the room that "stars are all that
matter" the week after Mentor Session 2 argued the standard error is the whole ballgame, or waves
away identification with "we just control for everything." This is the confusion IM-3 catalogs, and
the fix is a thirty-second framing in your introduction and a one-sentence debrief after the guest
leaves: name explicitly where the practitioner's shortcut is a *reasonable working compromise under a
deadline* versus where it is the exact mistake the camp is teaching students to avoid. A guest who
disagrees with a chapter is a teaching opportunity, not a problem, *provided you surface the
disagreement* rather than leaving students to silently resolve it the wrong way.

**Tie each guest forward.** Like the mentor sessions, a guest lecture should close with one sentence
connecting the visit to the students' own work: the regulator's documentation standard is the
threats-table discipline of their capstone; the quant's failed backtest is why their robustness
section must include the check that *failed*; the founder's disparate-impact exposure is what
Capstone 1 measures; the journalist's headline test is the eight-minute defense. The guest gives the
war story; you draw the line back to the deliverable.

---

## Part B — Facilitating the eight Lei Gao mentor sessions

Each mentor session ships with the same five parts, and they are designed to be run in order: a
**one-page pre-read packet**, **three Socratic warm-up questions**, a **five-slide speak-from deck**,
**three stretch questions** tied to one of Prof. Gao's papers, and a **post-session reflection
prompt**. The Week 4 session (`mentor4-detecting-discrimination.md`) is the template to study before
you run any of them; it shows the intended register exactly. Below are the facilitation notes that
turn those materials into a good sixty minutes.

**Use the pre-read packet as a gate, not a suggestion.** The packet is one page and is meant to be
read slowly *before* the session. The single highest-leverage move you make all week is enforcing
that it was read — not by quizzing, but by opening the session with the warm-ups, which are
unanswerable by a student who skipped it. Collect the written warm-up answers at the door (the
materials ask students to "come with a written sentence or two for each"). This does two things: it
guarantees preparation, and it gives you a thirty-second read of where the room is before you have
said a word. Tell students explicitly, in Week 1, that the packets are short *because* they are dense
and that skimming them defeats the design.

**Run the warm-ups Socratically — they are framed to have no clean answer.** The three warm-ups are
written, per the Week 4 example, as questions with "honest answers and dishonest ones, not right and
wrong ones." Resist the instinct to confirm a correct answer and move on. The move is to take a
student's sentence and press it: *what would have to be true for that to fail?* When a student gives
the textbook answer ("control for creditworthiness, read the residual"), your job is to be the
unobserved confounder — to name the soft variable the underwriter saw that the regression did not.
Aim to spend ten to twelve minutes here and to have surfaced the session's central tension before any
slide appears. The deck then *resolves* a tension the warm-ups *opened*, which is the right
pedagogical order.

**Time the five-slide deck to leave room for the stretch questions.** The deck is a speak-from
scaffold, not a read-aloud — each slide is bullets you talk *around*. Budget roughly four minutes a
slide, twenty minutes total, and protect that ceiling ruthlessly: the deck is the *least* important
of the five parts because it is the one closest to the chapters the students already read. If you run
long, cut from the deck, never from the stretch questions or the discussion. A practical rhythm for
the sixty minutes: pre-read assumed done (0 min), warm-ups and discussion (12), deck (20), stretch
questions worked in pairs then aloud (22), reflection prompt assigned (6). Adjust, but keep the deck
from eating the back half.

**Work the stretch questions in pairs, then surface to the room.** The three stretch questions are
the intellectual core: they take the week's identification ideas and point them at one of Prof. Gao's
papers, and they are deliberately *reason-about-method-only* — the Week 4 set says plainly, "Do not
invent or recite any specific reported magnitudes from the paper." Honor that instruction when you
facilitate; if a student reaches for a number, redirect to the judgment ("never mind what they found
— what comparison makes the finding believable?"). Have students work them in pairs for six or seven
minutes first. Pair work is the mechanism that gets quiet students talking, because a student who will
not address the room will tell a partner what they think, and you can then invite the *partner* to
report it — which brings me to the next point.

**Draw out quiet students by structure, not by cold-calling.** Cold-calling a quiet student on a hard
identification question usually produces silence and embarrassment, not learning. Three structural
moves work better. First, the **pair-and-report** above: ask a student to relay their partner's idea,
which lowers the stakes because they are not defending their own view. Second, the **written-first**
habit — because the warm-ups and stretch questions ask for written sentences, every student has
something on paper to read from, so "read me what you wrote for the second warm-up" is always a fair
ask. Third, **route by interest**: the recurring student cast (Maya–fair lending, Devon–crypto,
Priya–climate/insurance, Sam–markets, Leah–patents/text) maps onto the sessions, and a quiet student
is far more likely to speak when the question is steered toward their hook. The Week 4 fair-lending
session is Maya's; the Week 6 text session is Leah's; use that. Keep a private tally of who has spoken
each week and close the gap deliberately over the eight weeks.

**Connect every session explicitly to the capstone.** This is the thread that makes the eight
sessions a sequence rather than eight disconnected talks, and it is the easiest thing to forget under
time pressure. Every session should end with one sentence naming what this week's identification
lesson will demand of the student's own capstone. The materials already do this — the Week 4 deck's
final slide says outright, "Your Lab 4 and Capstone 1 are exactly this." Say it out loud every week:
Week 2's SE discipline is the wild-cluster-bootstrap row of the capstone rubric; Week 3's
selection-on-observables failure is why the capstone needs a *design*, not a longer control list;
Week 5's "anatomy of a paper" is the structure of the paper they will write; Week 7's "how I pick a
project — and kill one" is the literal task of their Week 7 deliverable. The reflection prompt is the
hand-off: it asks students to take an outside claim and run it through the week's discipline, which
is rehearsal for running it on their own result. Read a few reflections aloud at the start of the
*following* session — the materials invite this — so the students see that the prompt was real work,
not a formality, and so the sessions chain.

**A word on register.** Prof. Gao's sessions are written in the first person and trade on the
authority of someone who took a question "all the way from a dataset to Congressional testimony." If
you are facilitating a session you did not write — covering for a guest mentor, or running the camp
without Prof. Gao present — do not impersonate that voice. Present the paper as a worked case from the
literature, keep the Socratic structure exactly, and be honest that you are relaying the design lesson
rather than the war story. The structure carries the learning; the personal stakes are a bonus, not
the load-bearing element.
