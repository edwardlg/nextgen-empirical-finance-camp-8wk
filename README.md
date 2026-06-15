# Empirical Finance from the Ground Up

### A Textbook, Lab Manual, Problem-Set Bank, Capstone Gallery, and Instructor's Manual for the GMU NextGen Empirical Finance Research Camp — **8-Week Edition**

**Host:** Prof. Lei Gao, Associate Professor of Finance, Costello College of Business, George Mason University
**Feeds:** NextGen FinTech Scholars / Data Science Young Scholars Research Program (Schar School)
**Audience:** mathematically gifted U.S. high-school students (AP Calc BC + AP Stats; some Python)
**License intent:** CC-BY-NC (see front-matter title page)

---

## 1. What this is

This repository is a publication-quality, self-contained curriculum for an **intensive 8-week empirical-finance research camp** aimed at top high-school students. It is the **8-Week Edition** of the NextGen camp: the full instructional core (Weeks 1–8) with the symposium-and-paper essentials folded into the arc rather than spread across extra weeks — second-round **robustness v2** in Week 4, the **manuscript build** in Week 7, and the **conference talk, poster, defense, and submission** in Week 8. The deeper, long-form treatments of conference prep and paper refinement remain available on demand (office hours). It is simultaneously:

- a **textbook** that rebuilds probability, OLS, and modern causal inference from first principles, using the "reveal-the-trick" pedagogy (state the result, show why it works, show when it fails, show the code);
- a **lab manual** of hands-on, dataset-driven exercises;
- a **problem-set bank** with a fully worked solutions manual;
- a **capstone gallery** of five publication-style example papers students can model their own work on; and
- an **instructor's manual** for deploying and grading the camp.

The camp is hosted by Prof. Lei Gao (Costello College of Business, GMU) and is designed to feed GMU Schar School's **NextGen FinTech Scholars / Data Science Young Scholars Research Program**. The book deepens the methodological core; the live NextGen program leads on conference presentation and paper refinement (see the [Articulation Matrix](book/00-front-matter/01-articulation-matrix.md)).

**Quick stats**

| | |
|---|---:|
| Weeks of instruction | 8 |
| Chapters / reading guides + appendix sections | ~44 |
| Verified Jupyter notebooks | 40 |
| Data cards (Appendix C / `data-cards/`) | 34 |
| Appendices | 5 (A–E) |
| Capstone example papers | 5 |
| Instructor's-manual modules | 6 |

---

## 2. Repository map

```
nextgen-empirical-finance-camp-8wk/
├── README.md                         This file — master entry point.
├── CONVENTIONS.md                    "The Constitution": voice, notation, spec discipline, reproducibility, citations.
├── install-tools.sh                  One-shot installer for CLI tooling (jq, fd, gh, ...).
├── .gitignore                        Excludes licensed data and secrets from version control.
│
├── book/                             All prose.
│   ├── TOC.md                        The full plan: every chapter, problem set, notebook, lab, mentor session, word budget.
│   ├── 00-front-matter/              Preface, articulation matrix, how-to-use, prerequisite self-test.
│   ├── weeks/                        The 8-week arc.
│   │   ├── week-01/ ... week-08/     Each week: a week-opening narrative, 5 chapters, 5 problem sets, lab/reading pack, mentor session, assessment. Week 4 adds robustness v2; Week 7 adds the manuscript build; Week 8 adds the conference talk/poster/defense and submission.
│   ├── appendices/
│   │   ├── A-math-toolkit/           Matrix algebra, optimization, asymptotics, distributions reference.
│   │   ├── B-python-latex-setup/     Conda env, GitHub Classroom, WRDS Cloud, Hopper SLURM, Overleaf/LaTeX.
│   │   ├── C-data-dictionary/        Pointer/index for the data cards.
│   │   ├── D-style-guide/            Empirical-finance writing: tables, regressions, robustness, replication packet.
│   │   └── E-solutions-manual/       Every problem set fully worked, organized by week.
│   ├── capstones/                    5 publication-style example papers + annotated "how it was built" notes.
│   └── instructor-manual/            Pacing, rubrics, pitfalls, mentor notes, answer keys, equity/access.
│
├── notebooks/                        Runnable companions, one per chapter.
│   └── week-01/ ... week-08/         40 verified notebooks mirroring the chapters.
│
├── data-cards/                       ~34 data-source cards (provider, coverage, identifiers, access, license, gotchas).
│
├── docs/
│   └── superpowers/specs/            Design specs (front-matter & TOC design, citation anchor list).
│
└── agents/
    └── handoffs/                     Per-slice agent handoff logs and the running [CHECK] ledger.
```

---

## 3. The 8-week arc

Each week is a week-opening narrative, five-or-more chapters, five daily problem sets (solutions in Appendix E), a notebook per chapter with a "Your Turn" extension, a lab manual (Weeks 1–4, 7–8) or paper-reading guides (Weeks 5–6), a 60-minute Lei Gao mentor session, and an end-of-week assessment with rubric. Weeks 1–6 build the toolkit; Weeks 7–8 are the student's own research project, carried through robustness, manuscript, conference talk, and submission. The symposium-and-paper material that a longer camp spreads over extra weeks is **folded into the arc**: robustness v2 into Week 4, the manuscript build into Week 7, and the conference talk/poster/defense and submission into Week 8 (with long-form treatments available on demand in office hours).

| Week | Theme | Link |
|---|---|---|
| 1 | **Foundations & inference** — probability, sampling, the logic of inference, with simulation as the microscope. | [week-01](book/weeks/week-01/README.md) |
| 2 | **The OLS engine** — OLS in matrix form, Gauss–Markov, FWL, and the three ways the story breaks (heteroskedasticity, clustering, misspecification). | [week-02](book/weeks/week-02/README.md) |
| 3 | **Causal inference I** — potential outcomes, selection-on-observables (matching, entropy balancing, AIPW), and instrumental variables. | [week-03](book/weeks/week-03/README.md) |
| 4 | **Causal inference II + robustness v2** — DiD/event studies, the staggered-adoption crisis, RD, synthetic control, shift-share — plus multiple testing, heterogeneity, mechanisms, and external validity. | [week-04](book/weeks/week-04/README.md) |
| 5 | **Reading the frontier I** — one classic empirical paper per day, decoded with a fixed Reader's-Guide anatomy. | [week-05](book/weeks/week-05/README.md) |
| 6 | **Reading the frontier II + AI** — text-as-data and modern empirics, plus a module on using LLMs responsibly as a research co-pilot. | [week-06](book/weeks/week-06/README.md) |
| 7 | **Research project I + manuscript build** — from question to a falsifiable, pre-registered design with data in hand; and the abstract, tables, figures, intro, and literature review that carry it. | [week-07](book/weeks/week-07/README.md) |
| 8 | **Research project II + symposium & submission** — execution, robustness stress-tests, writing to publication standard, defense; the conference talk, poster, and Q&A; and a real submission (Young Scholars Journal, MARS, SSRN, the circuit). | [week-08](book/weeks/week-08/README.md) |

See [`book/TOC.md`](book/TOC.md) for the line-by-line plan and word budget.

---

## 4. How to use the book (student path)

1. **Start in the front matter.** Read the [Preface](book/00-front-matter/00-preface.md) (why empirical finance, and why a high-schooler can do real research), then the [Articulation Matrix](book/00-front-matter/01-articulation-matrix.md) to see how the camp maps onto the live NextGen program.
2. **Read [How to Use This Book](book/00-front-matter/02-how-to-use.md).** It explains the daily rhythm and how chapters, problem sets, notebooks, labs, and mentor sessions interlock, and introduces the recurring student cast (Maya, Devon, Priya, Sam, Leah).
3. **Take the [Prerequisite Self-Test](book/00-front-matter/03-prerequisite-self-test.md).** It routes you: if you miss the calculus/probability/Python items, read **Appendix A** ([Math Toolkit](book/appendices/A-math-toolkit/README.md)) and **Appendix B** ([Python + LaTeX Setup](book/appendices/B-python-latex-setup/README.md)) first.
4. **Work the weeks in order.** For each chapter, read the prose, run the matching notebook in [`notebooks/`](notebooks/README.md), then do that day's problem set; check yourself against [Appendix E](book/appendices/E-solutions-manual/README.md).
5. **Aim at the capstone.** Weeks 7–8 turn the toolkit into your own paper; the [Capstone Gallery](book/capstones/README.md) shows five finished exemplars.

---

## 5. How to deploy the camp (instructor path)

1. **Set up the environment.** Follow **Appendix B** ([B.1 Toolchain](book/appendices/B-python-latex-setup/B1-toolchain.md)): install Miniconda/Miniforge and create the camp environment from the `environment.yml` declared there (`conda env create -f environment.yml`). The stack is pinned: `python=3.11`, `pandas>=2.2`, `numpy`, `scipy`, `statsmodels`, `linearmodels`, `pyfixest`, `matplotlib`, `wrds`, `yfinance`, `pandas-datareader`, `requests`, `pyarrow`.
2. **Wire GitHub Classroom.** See [B.2](book/appendices/B-python-latex-setup/B2-github-classroom.md) for the assignment-repo and submission workflow.
3. **Provision licensed data on GMU infrastructure.** WRDS Cloud access and querying are covered in [B.3](book/appendices/B-python-latex-setup/B3-wrds-cloud.md); GMU Hopper SLURM templates (batch jobs, A100 GPU for the AI module) in [B.4](book/appendices/B-python-latex-setup/B4-hopper-slurm.md). **Per CONVENTIONS §5, licensed data (CRSP, Compustat, etc.) stays read-only on GMU infrastructure (Hopper / WRDS Cloud) and is never committed to this repo.**
4. **Configure the AI module keys (Week 6).** Provide credentials via **environment variables only** — `ANTHROPIC_API_KEY` for the Anthropic Messages API and `AZURE_OPENAI_KEY` (with the GMU Azure OpenAI deployment) for the GMU gateway. A **no-key local fallback** runs the same exercises against Ollama / a Hopper A100, so students without keys are never blocked.
5. **Run the camp.** The [Instructor's Manual](book/instructor-manual/README.md) provides the [pacing guide](book/instructor-manual/IM1-pacing-guide.md) (the 8-week clock — the instructional core plus the folded-in symposium, manuscript, and submission material), [grading rubrics](book/instructor-manual/IM2-grading-rubrics.md), [common pitfalls](book/instructor-manual/IM3-common-pitfalls.md), [guest-lecture/mentor notes](book/instructor-manual/IM4-guest-lectures-mentor-notes.md), [answer keys + anchor work](book/instructor-manual/IM5-answer-keys-anchor-work.md), and [equity/access notes](book/instructor-manual/IM6-equity-access.md).

---

## 6. Reproducibility & conventions

This project enforces a strict reproducibility discipline, set out in [`CONVENTIONS.md`](CONVENTIONS.md):

- **Pinned environment.** `python=3.11` plus the pinned scientific stack above; every code block must run end-to-end on a fresh conda env.
- **Notebooks verified headless.** All 40 notebooks are designed to execute non-interactively, with sample output and a "Your Turn" extension.
- **Secrets via environment variables only.** No keys are hard-coded; `.env` and `*.key` are git-ignored.
- **Licensed data never committed.** Data files (`data/`, `*.parquet`, `*.csv`) are git-ignored except the documentation in `data-cards/`; licensed snapshots stay on GMU infrastructure with their snapshot date pinned in the notebook that uses them.
- **The `[CHECK]` policy.** Unverified citations and live API/endpoint specifics are tagged `[CHECK]` rather than fabricated, so a human can confirm them before publication.

See [`CONVENTIONS.md`](CONVENTIONS.md) for the full constitution: audience and voice, the recurring student cast, notation, empirical-spec discipline, code rules, citation rules, and file/naming conventions.

---

## 7. Credits & status

- **Author/host:** Prof. Lei Gao, Costello College of Business, George Mason University.
- **Build process:** drafted via a multi-agent workflow (Planner → Writer → Coder → Reviewer), with every slice logged under [`agents/handoffs/`](agents/handoffs/README.md).
- **Open items:** outstanding `[CHECK]` items — chiefly exact page ranges/venues for the Week 5–6 cited papers (Fama–French 1992/1993, Jegadeesh–Titman 1993, Petersen 2009, Bertrand–Duflo–Mullainathan 2004, KPSS 2017, Hoberg–Phillips 2016, Loughran–McDonald 2011, Bartlett et al. 2022, and Bhutta–Hizmo–Ringo's venue) — are tracked in [`agents/handoffs/`](agents/handoffs/README.md) and consolidated at the foot of [`book/TOC.md`](book/TOC.md). All Prof. Gao citations follow `CONVENTIONS.md §6` verbatim.
