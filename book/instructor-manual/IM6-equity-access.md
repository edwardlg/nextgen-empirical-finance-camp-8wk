# IM-6 — Equity, Access & Compliance

This section is about the parts of running the camp that decide whether *every* admitted student can
actually do the work — not just the ones who arrive with a fast laptop, a paid API key, or no
accommodation needs — and about the one rule whose violation is not a pedagogical problem but a legal
one. The camp was designed so that none of the access constraints below is a hard barrier: it is
**completable end to end with no commercial API key, no personal data subscription, and no hardware
beyond a basic laptop**, because the licensed-data and compute paths all route through GMU
infrastructure that the camp provides. Your job as instructor is to make sure that design actually
reaches each student. IM-1 (pacing) tells you when the access-dependent weeks land; this section tells
you how to keep them open to everyone.

---

## WRDS seats: a scarce resource to schedule, not assume

WRDS access is a **seat under GMU's institutional subscription** (Appendix B.3), not something a
student buys. Seats are limited and access is institutional, so treat them as a managed resource from
day one.

**Request accounts in the first days of the camp, in a batch.** Approval is not instant — a WRDS
administrator at GMU (or you, acting on the students' behalf) must approve each request, and it
triggers a credential email. Have every student register with their **GMU email address** (a personal
Gmail is rejected, because the GMU address is what ties them to the subscription) on day one, so seats
are live by Week 2's first CRSP work and not scrambling the morning a lab needs them. Confirm each
student can log in to the WRDS web interface once before they need it in a notebook.

**Schedule heavy use; do not let the cohort hammer the shared server at once.** The expensive moments
are the CRSP/Compustat-touching labs — Lab 2 (Fama–MacBeth), the Week 5 portfolio sorts, Week 7 data
acquisition. If seat or server contention is a real constraint for your cohort size, stagger the
class: have students prototype query *logic* on the small illustrative `permno` lists the appendix
uses (which barely touch the server) and run the full-universe pulls on a rota or as queued Hopper
jobs rather than forty simultaneous interactive `SELECT`s. Appendix B.3 already teaches the etiquette
that makes this work — bound every query by date and identifier, prototype small before scaling — and
B.4 gives the batch-job path that takes load off interactive seats entirely. A student seat is
**read-only** and **non-shareable**; sharing a login is both a security problem and a license
violation, so the answer to "I can't get a seat in time" is never "use a classmate's" — it is "pull a
small extract on the available seat, or run it on Hopper," never shared credentials.

**A student who cannot get a WRDS seat can still complete the camp.** Every WRDS-dependent deliverable
has a public-data path. The capstone gallery includes papers built entirely on public sources (the
USPTO PatentsView innovation paper, the FRED macro event study), and the data dictionary (Appendix C)
documents the public alternatives — FRED, SEC EDGAR, HMDA, USPTO, yfinance — that carry the fair-lending,
text, and macro threads without a CRSP seat. If a seat is delayed or unavailable for a given student,
route their project to a public-data question. No student is blocked from the capstone by a seat
queue.

---

## Compute access: Hopper, the AI-module API budget, and the local fallback

The camp's compute needs are met by GMU infrastructure, with deliberate fallbacks so that lack of
money or hardware is never the barrier.

**GMU Hopper accounts for the heavy and the licensed work.** Hopper is GMU's research-computing
cluster (Appendix B.4), and it is where two kinds of work belong: anything that touches licensed data
(because that data must stay on GMU infrastructure — see Compliance below) and anything that needs a
GPU or more memory and time than a laptop has. Getting a student onto Hopper is paperwork, not coding:
a Hopper account tied to their GMU NetID, membership in the camp's allocation/account string, and the
GMU VPN if they are off-campus. Start that paperwork early, the same week as the WRDS requests, because
the AI module in Week 6 and the licensed-data steps in Weeks 7–8 both assume it is in place. The exact
partition names, allocation string, and module names are cluster-specific and are tagged `[CHECK]` in
B.4 — confirm them against ORC's current documentation before the cohort's first submission, because a
wrong partition string is the most common reason a first job never runs.

**The AI-module API budget, and how to ration it.** Week 6 uses LLMs as a research co-pilot, and the
camp provides access two ways (Ch 6.5): the Anthropic Messages API directly, and OpenAI-family models
(GPT-5.4 / 4.1 / 4o) through **GMU's Azure OpenAI gateway** (`apim-n1ai-use-gmun1.azure-api.net`), with
the key read from `${AZURE_OPENAI_KEY}` and never hard-coded. API calls cost money against a shared
budget, so ration them: have students develop and debug their prompts and pipelines on a *tiny* sample
(a dozen filings) before they run a classifier over a full corpus, and prefer the local path for the
bulk classification job. The two-provider setup is also a robustness check, not just redundancy — a
result that survives re-labeling with a different vendor's model is stronger evidence — so spending a
little budget to cross-check a headline classification is good practice, not waste.

**The local-Ollama fallback, and the promise that no key is required.** This is the equity guarantee
that matters most for the AI module: **a student with no API key and no budget can complete the entire
camp**, because the local path runs an open model with **Ollama** on the student's own machine (an
~8B model serving on `localhost:11434`, data never leaving the laptop) or, for larger models, on a
Hopper A100 node where licensed text already lives and stays. The local path carries *no API key and
no secret of any kind*, because there is no remote service to authenticate to — which makes CONVENTIONS
§5's "secrets via env vars only" rule trivially satisfied and, more to the point, makes the module
free. Tell students this explicitly in Week 6: the API path is a convenience and a capability boost,
not a requirement, and the camp's design assumes some students will run entirely local. The one cost
is capability — an open 8B model is weaker than a frontier API model — so a locally-labeled classifier
needs the *same* out-of-sample precision/recall/F1 validation against a hand-labeled gold set that any
classifier does (Ch 6.5); validation is what tells a student whether the free, private choice is also
an accurate-enough one, and if it is not, the larger Hopper model is the escalation, still key-free.

---

## Accommodations for students with disabilities

Run the camp's accommodations through **GMU's disability-services process** — registered
accommodations are documented and binding, and you should ask in the first days (privately, not in
front of the cohort) whether any admitted student has accommodations on file, then implement them
without making the student re-explain. Beyond the formal letter, several camp-specific points are
worth anticipating because the format is intensive and unusual:

- **Timed and high-stakes work.** The end-of-week assessments and the eight-minute capstone defense
  are the timed elements. Extended-time accommodations apply to the assessments as they would to any
  exam; for the defense, the binding constraint is the *content arc* (question → design → result →
  robustness → contribution), not the stopwatch — a student with a relevant accommodation can be given
  a longer slot without penalty, since the rubric grades the argument and the defense, not raw speed.
- **The notebooks and code.** Make sure the chapter notebooks and any slide decks meet basic
  accessibility — figures with described content for screen-reader users, sufficient color contrast in
  the plots (the CLT animations, coefficient plots, and event-study charts especially), and
  keyboard-navigable tooling. Prefer materials that read well as plain Markdown and LaTeX, which the
  camp already uses, because that format degrades gracefully to assistive technology.
- **The pace and the mentor sessions.** The daily rhythm is demanding; build in the flexibility a
  documented accommodation requires (recorded mentor sessions and guest lectures, which IM-4 already
  recommends recording, directly serve students who need to revisit material or who cannot attend a
  live block). The mentor sessions' written-first structure (warm-ups and stretch questions answered
  on paper) is itself an accessibility feature — it gives every student, including those for whom live
  verbal participation is hard, a prepared way in. Lean on it.

When in doubt, the principle is the camp's own: the deliverables grade *reasoning and honesty*, so any
accommodation that preserves the student's ability to demonstrate reasoning while removing an
unrelated barrier is consistent with the standard. Coordinate the specifics with GMU's disability
services rather than improvising.

---

## Data-licensing compliance: the §5 rule, restated for instructors

This is the one rule in the camp whose violation is not a learning setback but a breach of GMU's
license agreements, and it is your responsibility to enforce it across the cohort. CONVENTIONS §5
states it; here it is in the form instructors need.

**Licensed data stays on GMU infrastructure, read-only. Never have students pull CRSP, Compustat,
TRACE, IBES, or any other licensed dataset to a personal machine or send it to an external API.** GMU
*leases* these datasets under agreements that forbid copying the raw records off GMU-controlled
systems or redistributing them. The professional pattern, which the camp teaches from Week 2 onward,
is to **send the query to the data, not the data to the laptop**: the filtering runs on the WRDS
Cloud or on Hopper, and only a small *derived* extract — your filtered, often aggregated result —
comes back as a working product. The terabytes stay put.

What this means in practice, as the lines you police:

- **No raw licensed datasets on personal laptops.** A small bounded extract for active analysis is
  normal research practice; mirroring a raw dataset is not. When a pull is large or sensitive, it runs
  on Hopper, where the data lives behind GMU authentication end to end.
- **No licensed data in any repository.** The replication packets (Lab 7, Lab 8) must *not* ship CRSP
  or Compustat bytes. The repo holds *code* — a pinned access script that re-pulls identical bytes for
  a reviewer who has their own seat — never the licensed data itself. The template `.gitignore`
  already blocks `*.dta`, `*.sas7bdat`, and `data/raw/`; pushing a CRSP `.dta` to GitHub is
  redistribution and breaches the license. Audit student repos for this before any capstone is
  deposited.
- **No licensed text to a commercial API.** Sending raw CRSP/Compustat/TRACE-licensed text to an
  external LLM over the public internet may violate the data-use agreement regardless of the vendor's
  privacy policy — the data is not the student's to transmit. When the text to classify is licensed,
  the model runs *locally* (Ollama on the laptop, or an open model on a Hopper A100), so the text
  never crosses the boundary. This is the same local-fallback path that also makes the module free;
  here it is doing double duty as the compliance mechanism.
- **No shared WRDS credentials, ever.** Sharing a login is both a security problem and a license
  violation. The §5 secrets rule — credentials via `.pgpass` or environment variables, never in code,
  never pasted where a teammate can see them — exists precisely so no student ever needs to share a
  password.
- **Pin the snapshot date.** Every notebook that touches licensed data records the date its
  CRSP/Compustat snapshot was pulled, because vendors revise history and a result is only reproducible
  relative to *when* it was pulled. This is reproducibility hygiene and a compliance trail at once.

The clean mental model to teach and to enforce: **the recipe travels; the protected ingredients stay
in the GMU kitchen.** Anyone with a student's repo and their own WRDS seat can rerun the code and
regenerate the extract from the source — that is reproducibility done right. What no one can do, and
must not be able to do, is read GMU's licensed data *out of a student's repository or laptop*, because
it was never there. Make this rule explicit in Week 2 when students first touch WRDS, restate it at
Lab 7 when they build the repo, and verify it at the capstone deposit — a hard-coded secret or shipped
licensed data is the one error that caps the reproducibility rubric row at Missing on its own (Week 8
assessment; CONVENTIONS §5), and the one error that has consequences beyond the gradebook.
