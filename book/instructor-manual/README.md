# Instructor's Manual

This is the teaching companion to the student textbook. The textbook is a separate document — the
eight themed weeks of chapters, problem sets, notebooks, labs, and assessments a student works
through. This manual is for the person running the room. It does not re-teach the content; it tells
you how to *deliver* it: how to pace the eight weeks (and compress them when the calendar is shorter),
how to grade every deliverable to one consistent standard, which errors your strongest students will
predictably make and how to catch them, how to run guest lectures and Prof. Gao's weekly mentor
sessions so they earn their place, how to grade fast and in agreement using answer keys and anchor
work, and how to keep the camp open and compliant for every admitted student regardless of hardware,
budget, or accommodation needs.

The six sections are designed to be read in order the first time and consulted by name thereafter.
Every cross-reference inside them is by section name rather than file path, so the manual survives
reordering.

## The six sections

1. **[IM-1 — Pacing Guide](IM1-pacing-guide.md).** The clock. Lays out the 8-week arc and its daily
   block rhythm, then gives the concrete mapping that compresses the eight weeks into NextGen's seven
   weekly Friday sessions (CONVENTIONS §8) — a re-architecture, not a trim, because two hours a week
   for seven weeks is roughly fourteen contact hours against a book built for closer to two hundred.
2. **[IM-2 — Grading Rubrics (Consolidated)](IM2-grading-rubrics.md).** Every deliverable's rubric in
   one place — daily problem sets, weekly assessments, the **200-point capstone rubric**, and the
   presentation/defense — all built on one standard: a modest result honestly stress-tested outscores
   an ambitious one the student cannot defend.
3. **[IM-3 — Common Student Pitfalls, by Week](IM3-common-pitfalls.md).** The predictable
   misconceptions a smart, well-prepared 17-year-old arrives with or develops, week by week, each with
   a diagnostic question that surfaces it fast and a fix that corrects it without re-teaching the
   chapter.
4. **[IM-4 — Suggested Guest Lectures & Mentor-Session Facilitation](IM4-guest-lectures-mentor-notes.md).**
   How to slot outside professional voices into the week, and how to run the sixty-minute weekly Lei
   Gao mentor session — a graded, structured part of the arc, not optional color.
5. **[IM-5 — Answer Keys & Anchor Work](IM5-answer-keys-anchor-work.md).** How to grade *against* the
   IM-2 rubrics quickly and consistently using Appendix E, the per-week assessment keys, and anchor
   papers pinned at the A/B/C boundaries — plus a norming protocol for multiple graders.
6. **[IM-6 — Equity, Access & Compliance](IM6-equity-access.md).** Keeping the access-dependent weeks
   open to everyone — WRDS seats, Hopper compute, the no-API-key local path — and the licensing rule
   whose violation is a legal problem, not a pedagogical one.

## Before you teach

Three things to internalize before the first session.

**The camp is completable with no API key.** Every part of it, including the Week-6 AI module, runs on
a local fallback that needs no commercial API key, no personal data subscription, and no hardware
beyond a basic laptop. A student with no budget is not blocked from any deliverable. IM-6 is the
section that makes sure that design actually reaches each student.

**Licensed data stays on GMU infrastructure.** Per CONVENTIONS §5, CRSP, Compustat, and the other
licensed sources are read-only on GMU's WRDS/Hopper paths and never leave them. Every notebook that
touches licensed data pins its snapshot date. The licensed extract is regenerated from the source, not
copied out — moving licensed data off GMU infrastructure is the one error that caps the
reproducibility rubric row at Missing on its own (IM-6).

**Two integrity checks are non-negotiable.** First, the **pre-analysis plan is filed as a tagged
commit before the student peeks at confirmatory results** — `git tag pap-filed` must predate every
confirmatory regression in the Git history, or the result is a finding fished from data that already
flattered it (IM-3 Pitfall 7.1). Second, **reproducibility**: the whole pipeline must regenerate from
source under `make clean && make all`, with nothing reported from memory (IM-3 Pitfall 5.1). These are
the two things you do not let slide for anyone.

## A note on the program-level weighting

The overall grade weighting proposed in **IM-2 §1** (problem sets / weekly assessments / Week-7 design
deliverable / capstone / presentation, with a downstream shift under the NextGen compression) is an
**editorial proposal for Prof. Gao to finalize**, not a fixed policy. The per-week assessment files
(`book/weeks/week-0N/assessmentN.md`) remain the authoritative source for each individual rubric; what
IM-2 proposes is only how they sum into a final grade. Treat the program-level percentages as a
starting point pending Prof. Gao's sign-off. The invariant IM-2 defends — that identification,
execution/robustness, and reproducibility together exceed half the capstone grade — should survive any
re-weighting.
