# How to Use This Book

This book is unusual, so it is worth a few pages explaining how it works before you dive in. Read this section once, all the way through. It will save you time later, and it will tell you why the parts are shaped the way they are.

## The reveal-the-trick learning model

Most textbooks introduce a technique by stating it formally, proving something about it, and then — somewhere near the end, if you are lucky — showing you what it is good for. We do the opposite, on purpose. Every technical idea in this book is taught in the same four-beat rhythm, and once you notice the rhythm you can use it to pace yourself.

First, we **state the result in one plain sentence** — what the tool does, in words, before any symbols. Second, we **show why it works**: the intuition, then a small worked example with actual numbers you can check by hand, and only then the algebra that generalizes the example. Concrete before abstract is a rule we hold to everywhere; you will almost always meet a number before you meet the Greek letter that stands for it. Third — and this is the beat most books skip — we **show you when it fails**: the specific assumption that breaks, what you actually see in your output when it breaks, and how to tell the difference between a result and an artifact. Fourth, we **show the code**, a runnable snippet that does the thing on real data.

We call this revealing the trick because that is honestly what it is. A statistical method is a trick in the good sense — a clever, specific maneuver that works under stated conditions. A magician's trick stops being impressive once you know how it is done; a *scientific* trick becomes more powerful, because now you know exactly when you are allowed to use it. Our goal is for you to never be intimidated by a method again, and never fooled by one either.

## The daily rhythm

The camp runs on a simple daily loop, and the book is built to support it. Each day you will **read the chapter section**, then **work the notebook**, then **do the problem set**, then bring what you found to a **mentor or lab session**.

These four are not redundant; each does a different job. The **chapter** builds the idea and reveals the trick. The **notebook** makes you run it — there is a difference between watching a regression and fitting one yourself on data that does not cooperate, and that difference is most of the learning. The **problem set** is where you find out whether you actually understood the chapter or merely nodded along; some problems are by-hand derivations, some are short interpretation questions, and some ask you to break a method on purpose so you can recognize the failure later. The **mentor or lab session** is where you bring your messiest result and your most stubborn confusion to a person, because some things only come unstuck out loud.

A reasonable day is read for the first part of it, code and problem-set through the middle, and arrive at your lab session in the afternoon with a specific question and a specific number you cannot explain. Do not arrive having read nothing; do not arrive having read everything and tried nothing.

## How the parts interlock

Here is the full set of pieces and how they fit.

**Chapters** carry the ideas. There are five per week, and they assume you read them in order within a week.

**Notebooks** mirror the chapters one-for-one. Every chapter ships a Jupyter notebook with sample output so you can confirm your environment is producing the right answers, and a **"Your Turn"** extension at the end that hands you a related question with less scaffolding. The "Your Turn" sections are not optional decoration; they are where you start practicing the independence that Week 7's research project will demand. Notebooks that touch licensed data — CRSP, Compustat, and the like — pin the exact snapshot date they were run against and note that the licensed data stays read-only on GMU infrastructure. You will learn early that *which version of the data* is part of a result, not a footnote to it.

**Problem sets** test and stretch. They range from a real Wooldridge-level derivation to a short essay arguing why a particular research design does or does not identify the effect it claims. Worked solutions live in the solutions manual (Appendix E), but spend real effort before you open it — a solution you read is worth a fraction of a solution you struggled toward.

**Labs and mentor sessions** are the human layer, where you defend reasoning, debug code with someone watching, and rehearse the skepticism you will later have to survive.

**Reading guides** appear in the middle weeks, when we start dissecting published research. A reading guide walks you through a real paper — including, in places, Prof. Gao's own — the way a critical researcher reads it: what is the question, what is the data, what is the design, what is the *one-sentence identifying assumption*, and where could it be wrong?

**The capstone** is the point of all of it: your original 12–20 page empirical paper, on real public data, with a research design you can defend and a replication package anyone can rerun. Everything else ladders up to this.

## The prerequisite self-test and Appendix A routing

Before Week 1, take the **prerequisite self-test** (the next section of this front matter). It is twenty questions across calculus, probability and statistics, and light Python, and it comes with full worked solutions. Be honest with yourself when you grade it — the point is not a score, it is a routing decision.

The self-test ends with a **routing table** that maps clusters of missed questions to specific remediation. If you stumbled on the calculus questions, you start with **Appendix A: Math Toolkit** before Week 1, not as punishment but because Week 2's inference material will be genuinely harder if your derivatives are rusty. If the Python questions tripped you, **Appendix B** gets your environment and your `pandas` fundamentals solid first. There is no shame in routing through an appendix; there is real cost in skipping it and discovering the gap mid-derivation in Week 3. Maya, one of the students you will follow, routes through Appendix A and is glad she did; Devon skips it, hits a wall on partial derivatives in Week 2, and has to double back. Learn from Devon.

## The standards: reproducibility and honest writing

Two expectations are non-negotiable from Day 1, because they are what separate this from a course where you merely get answers.

**Code must be reproducible.** Every result you report comes from code that runs start to finish on a clean environment — `python=3.11` with the libraries pinned in Appendix B — and produces exactly the number you wrote down. No "it worked on my laptop earlier." We avoid the patterns that silently corrupt results: no deprecated `pd.append`, no chained indexing without an explicit `.copy()`, no confusion between `.iloc` and `.loc`. Secrets such as API keys live in environment variables, never in the notebook. These are not stylistic preferences; each one is a bug that has cost real researchers real retractions.

**Writing must be honest.** When you state a regression specification, you state all of it: the **outcome**, the **treatment or key regressor**, the **controls**, the **fixed effects**, the **clustering** of the standard errors, the **sample**, and the **identifying assumption in one sentence**. You never write that something "controls for endogeneity"; you name the specific threat and the specific design that addresses it. When a result is fragile, you show how fragile. When you cannot rule out an alternative explanation, you write that down — and your paper gets stronger for the admission. This is the specification discipline the whole book runs on, and it is summarized in the conventions that govern every chapter.

## A note on notation and conventions

The book follows a consistent notation throughout — scalars in italics, vectors in bold lowercase, matrices in bold uppercase, hats for estimates, and standard errors always labeled with their flavor. You do not need to memorize the table now; it is there for reference, and we define every term the first time it appears anyway. The same goes for the specification discipline above: you will internalize it by using it, not by studying it in advance.

## The eight-week ladder

Step back and see the shape. **Weeks 1–2** build the engine: regression mechanics and honest inference. **Weeks 3–4** confront causation: what it would take to claim one thing *caused* another, and the designs that make such a claim defensible. **Weeks 5–6** teach you to read the research frontier critically and to turn a frontier paper into a question of your own. **Weeks 7–8** turn you loose to do it — specify, estimate, write, and ship a replicable original paper.

Each rung depends on the one below it. You cannot reason about causation in Week 3 without trusting your inference from Week 2, and you cannot trust your inference without the mechanics from Week 1. Resist the urge to jump ahead to the exciting causal material; the foundations are what make the exciting part trustworthy rather than just exciting. Maya, Devon, Priya, Sam, and Leah will be on the ladder with you the whole way up. By Week 8, like them, you will have done the real thing.
