# Lab 8 — Final Manuscript + Repo + Defense

This is the terminal lab of the camp, and it does not introduce a single new idea. That is by design. Everything you need was built in the seven weeks behind you: the estimate you can defend (Week 8 chapters), the robustness battery that survived (Chapter 8.2), the publication tables and figures generated from code (nb8.3), the repository you stood up in Lab 7, the pre-analysis plan you tagged, the identification memo whose threats table you wrote when you were calm. Lab 8 is the lab where those pieces stop being a pile of parts and become the three artifacts a researcher actually hands over: a **compiled paper**, a **one-click replication packet**, and an **eight-minute defense** that survives the questions. Chapter 8.5 made the argument for why those three are the form your credibility takes; this lab is the bench where you build them.

The discipline this lab drills is the one from Chapter 8.5 §6, stated as a procedure rather than a slogan: **a result a stranger cannot reconstruct is not yet a result.** So the "result" you are checking here is operational, exactly as it was in Lab 7 — not "is my coefficient right?" but "can a stranger, on a machine that has never seen my project, type one command and watch my paper rebuild from raw data to a finished PDF, and can I stand in front of that stranger for eight minutes and not flinch when they attack it?" Those are yes/no questions you can actually test, and the submission checklist at the end is how you test them. Work through the three parts in order; each builds the surface the next one defends.

One housekeeping note before you start. The camp container ships a LaTeX engine — TinyTeX with `latexmk`, `tectonic`, and `pdflatex` all on the `PATH` — and `make`, so every command in this lab runs as written on the container. If you are on your own laptop and have no TeX install, you will use the Overleaf path in Part 1 instead of the local compile; the `.tex` is the durable artifact, and a PDF is a convenience some machines produce and some cannot (this is the same gate nb8.5 builds around `shutil.which`).

---

## Learning goals

By the end of this lab you will be able to:

1. **Compile a paper from a minimal AEA-style LaTeX template** — a complete `main.tex` whose tables are `\input{}`ed and whose figures are `\includegraphics{}`ed, never pasted — via *both* the Overleaf workflow and a local `latexmk`/`tectonic` build.
2. **Finalize the Lab 7 repository as a one-click replication packet** — wire a `Makefile` (and an equivalent `run_all.py`) with `data`, `tables`, `figures`, `paper`, `all`, and `clean` targets that regenerate every artifact from raw inputs under a single fixed `SEED`.
3. **Run the `make clean && make all` honesty test** — delete every derived file, rebuild from nothing, and confirm the paper comes back identical (and know exactly which kinds of bytes will *not* be identical, and why).
4. **Rehearse the eight-minute talk against a timer** — a three-pass protocol, a pre-decided cut line, and a threats-table-driven script for the question period.
5. **Triage a hostile question live** — distinguish a *survivable* critique (attacks a number) from a *fatal* one (attacks the comparison), and know your retreat position before you walk in.
6. **Submit the capstone** against a checklist a referee — or Prof. Gao, at the conference — would run: paper PDF, repo URL, deck, replication packet verified.

---

## Setup

You already have the tools from Lab 7: Git, a GitHub account, and conda. Lab 8 adds one capability — turning LaTeX into a PDF — and the camp container has it. Confirm what is present before you start; a one-line probe each, exactly as the notebooks gate their compile:

```bash
latexmk --version       # the build driver (wraps pdflatex and runs the right number of passes)
tectonic --version      # a self-contained alternative engine (downloads packages on demand)
pdflatex --version      # the underlying engine, if you ever call it directly
make --version          # GNU make, for the Makefile targets
```

If `latexmk` and `tectonic` both print a version, you can build locally. If none of them resolves — likely on a personal laptop — that is fine: you will push the `paper/` folder to Overleaf and compile in the browser, which needs no local install at all. Either road ends at the same PDF.

Work inside the capstone repository you cloned in Lab 7 (`capstone-2026-<you>`), on the machine where you have been committing all week. If your project touches licensed data (CRSP, Compustat, anyone on the equities or fundamentals tracks), the Lab 7 rule still binds: the licensed bytes live only on GMU infrastructure, the repo travels freely, and the packet ships the *recipe* (the pull script in `src/`), never the data.

---

## Part 1 — Compile the LaTeX paper

A Markdown draft is for you; a compiled PDF is for the reader. The journals your capstone is modeled on — and the Schar Young Scholars Journal your paper may land in — typeset in LaTeX, and the convention in empirical economics is the **AEA article style**: a single-column article class with a title block, an abstract, numbered sections, `booktabs` tables, and a references list. You do not need the official `aea` document class to learn the workflow — a plain `article` with the right packages produces the same shape and compiles anywhere — so we build the minimal-but-complete skeleton first, and note where the AEA class drops in.

### 1.1 The `paper/` folder, finished

Recall the layout from Lab 7: your write-up lives in `paper/`, and Week 8's analysis writes its outputs into `paper/tables/` and `paper/figures/`. By now that folder looks like this, and every file under `tables/` and `figures/` was *written by your code* (nb8.3, nb8.5), never typed:

```
paper/
├── main.tex              # the manuscript (this section builds it)
├── tables/
│   └── main_results.tex  # GENERATED by src/03_analysis.py — never hand-edited
└── figures/
    └── event_study.pdf   # GENERATED by src/03_analysis.py — never hand-edited
```

The load-bearing rule of the whole lab lives in that comment: the manuscript *reads* the table and figure; it never *contains* a coefficient. The number `−1.8` appears nowhere in `main.tex` — it lives only in the generated `main_results.tex`, which `main.tex` `\input`s. That is the machine-traversable chain from raw bytes to PDF that Chapter 8.5 §5 demands, and it is what makes the honesty test in Part 2 possible.

### 1.2 A minimal but complete `main.tex`

Here is the skeleton. It is deliberately small but complete in *shape*: a title block, an abstract, an introduction, a data section, an empirical-design section, a results section that pulls in the generated table and figure, a conclusion, and a references list. Write this to `paper/main.tex`:

```latex
\documentclass[12pt]{article}
\usepackage[margin=1in]{geometry}
\usepackage{graphicx}      % \includegraphics for the figure
\usepackage{booktabs}      % \toprule/\midrule/\bottomrule for clean tables (Appendix D)
\usepackage{amsmath}       % aligned equations, \text, etc.
\usepackage{hyperref}      % clickable \ref and \cite
\graphicspath{{figures/}}  % so \includegraphics finds figures/ without a path prefix

\title{Fair-Lending Examination Programs and the Minority--White Denial Gap}
\author{Maya R.\thanks{NextGen FinTech Scholars, George Mason University.}}
\date{August 15, 2026}

\begin{document}
\maketitle

\begin{abstract}
We estimate how state fair-lending examination programs affect the county-level
minority--white mortgage-denial gap, exploiting the staggered timing of adoption.
We find the gap narrows by about 1.8 percentage points after adoption, with flat
pre-adoption trends consistent with the identifying assumption.
\end{abstract}

\section{Introduction}
The hook (the concrete stake), the one-line statement of the contribution, and a
one-sentence preview of the finding --- the Chapter 8.3 introduction in miniature.

\section{Data}
Sources, sample, and construction; point the reader at the data cards in the
replication packet rather than reproducing them here.

\section{Empirical design}
State the specification in the CONVENTIONS \S4 form: outcome, treatment, controls,
fixed effects, clustering, sample, and the identifying-assumption sentence \cite{cs}.

\section{Results}
Table~\ref{tab:main} reports the headline estimate; Figure~\ref{fig:event} plots the
event-study coefficients, whose flat pre-period leads visualize the parallel-trends defense.

\input{tables/main_results.tex}        % the table the analysis WROTE

\begin{figure}[t]\centering
  \includegraphics[width=0.72\linewidth]{event_study.pdf}   % the figure the analysis WROTE
  \caption{Event-study estimates with 95\% confidence intervals.}
  \label{fig:event}
\end{figure}

\section{Conclusion}
What you showed, under which assumption, and --- the most credibility-building clause ---
what you are \emph{not} claiming.

\begin{thebibliography}{9}
\bibitem{cs} Callaway, B., \& Sant'Anna, P.~H.~C. (2021). Difference-in-differences
with multiple time periods. \textit{Journal of Econometrics}, 225(2), 200--230.
\end{thebibliography}

\end{document}
```

Two things in this skeleton are worth dwelling on, because they are the lines you will get wrong if you skim. First, the two load-bearing lines are `\input{tables/main_results.tex}` and `\includegraphics[width=0.72\linewidth]{event_study.pdf}` — they are the *entire reason* this is not copy-paste. Second, a subtle compile trap that bit the verification of this very lab: if you use `\cite{cs}` with a manual `thebibliography` block (as above), do **not** also load the `natbib` package — `natbib`'s author-year mode is incompatible with a hand-written `thebibliography` and the build will fail with `Package natbib Error: Bibliography not compatible with author-year citations`. Use plain `\cite{}` with `\bibitem{}` as shown, *or* switch to BibTeX with a `.bib` file and `\bibliography{}` — but not the half-and-half. The skeleton above is the working combination; it compiles to a clean two-page PDF with no errors.

**Dropping in the real AEA class.** When your camp distributes the AEA template, the only change is the first line and the bibliography mechanism: `\documentclass[12pt]{aea}` instead of `article`, the `aea.bst` BibTeX style, and `\bibliography{references}` reading a `references.bib`. The body — `\maketitle`, `\begin{abstract}`, the sections, the `\input`/`\includegraphics` — is identical. Learn the shape on `article`; swap the class when the template lands. `[CHECK]` — confirm the exact AEA class/`.bst` filenames against the template the camp distributes; they are referenced here by their standard names, not copied from a distributed file.

### 1.3 The Overleaf workflow (no local install)

Overleaf is a browser LaTeX editor; it needs nothing on your machine. The workflow is three moves. **First**, create a new project and upload your `paper/` folder — `main.tex`, the `tables/` folder, and the `figures/` folder, preserving the structure (Overleaf keeps subfolders, and your `\input{tables/...}` and `\graphicspath{{figures/}}` depend on them). **Second**, set the main document to `main.tex` and the compiler to `pdfLaTeX` (Menu → Settings → Compiler) — or `LaTeX` if you later add BibTeX. **Third**, click *Recompile*; the PDF renders in the right pane, and Overleaf reruns the passes needed to resolve your `\ref`s and `\cite`s automatically.

The discipline that matters on Overleaf is the same one that matters everywhere: **the `tables/` and `figures/` you upload must be the ones your code just generated**, not files you edited in the browser. The honest move is to regenerate them locally (or on the container) with your analysis, then upload; if you ever edit a number in Overleaf, you have broken the chain and reintroduced the exact drift the whole packet exists to prevent. Overleaf is a *compiler*, not a place to author results.

### 1.4 The local `latexmk` / `tectonic` path

On the camp container, you skip the upload entirely and compile in place. The simplest robust command is `latexmk`, which figures out how many passes are needed to resolve cross-references and citations and runs them for you:

```bash
cd paper
latexmk -pdf -interaction=nonstopmode main.tex   # builds main.pdf, reruns passes as needed
```

`-pdf` selects PDF output (via `pdflatex` under the hood); `-interaction=nonstopmode` means a recoverable warning does not stop the build to wait for keyboard input — important when this runs unattended inside `make`. If you prefer the self-contained engine, `tectonic` needs no separate driver and downloads any missing packages on first use:

```bash
cd paper
tectonic main.tex            # single command; resolves passes internally, writes main.pdf
```

Either way you get `paper/main.pdf`. To start clean — which you will do constantly while debugging the build — `latexmk -C` deletes every auxiliary file (`.aux`, `.log`, `.fls`, `.fdb_latexmk`, `.out`) and the PDF, so the next build is from nothing. A note from verifying this lab: a *first* `latexmk` run on a fresh paper may print "Latex failed to resolve N reference(s)" and rerun automatically — that is `latexmk` doing its job across passes, not an error, and the final exit code is what you check. A genuinely broken build (a missing `\input` file, an undefined control sequence, the `natbib` clash above) exits non-zero and names the offending line in `main.log`; grep it with `grep -nE "^!|Error" main.log`.

---

## Part 2 — Finalize the GitHub repo as the one-click packet

The packet is not a new thing you build now; it is the Lab 7 repository, finished. Lab 7 gave you the layout, the pinned environment, the README, the data cards, the verified `.gitignore`, and the `pap-filed` tag. Part 2 adds the last load-bearing piece from Chapter 8.5 §5: a **single entry point that regenerates every artifact from raw inputs**, under a **single fixed SEED**, so a fresh clone goes from raw bytes to a compiled PDF with no human in the loop.

### 2.1 The one seed, in one place

Anywhere randomness enters your analysis — a bootstrap, a train/test split, a permutation test, a synthetic-control placebo — it must flow from *one named constant*, set once and imported everywhere, never scattered ad hoc. This is the difference between a confidence interval that is `[1.3, 2.3]` every run and one that drifts to `[1.2, 2.4]` and quietly destroys a reviewer's trust. Put the seed (and your project paths) in `src/config.py`:

```python
# src/config.py -- project-wide constants. ONE seed, imported everywhere.
from pathlib import Path

SEED = 20260815   # the conference date; any FIXED, named int -- fixed BEFORE results, never tuned after

ROOT     = Path(__file__).resolve().parents[1]   # the repo root, computed from this file
DATA_RAW  = ROOT / "data" / "raw"
DATA_PROC = ROOT / "data" / "processed"
TABLES    = ROOT / "paper" / "tables"
FIGURES   = ROOT / "paper" / "figures"
```

Every script that draws randomness imports `SEED` and threads it through a single modern NumPy generator — `rng = np.random.default_rng(SEED)` — and every draw goes through `rng`, never through the legacy global `np.random.seed`. The seed is recorded in your README so a reviewer can see it was fixed before results, not searched over afterward to get a prettier interval (which would be the garden of forking paths from Chapter 7.3 wearing a disguise). Computing `ROOT` from `__file__` rather than hard-coding a path is what lets the repo run unchanged on the container, on Hopper, and on a reviewer's laptop.

### 2.2 Code in run-order

Your `src/` holds the pipeline as numbered scripts, so the order is unambiguous (Lab 7's rule: *logic in `src/`, story in `notebooks/`*). Three steps, each consuming the previous step's output:

```python
# src/01_pull_data.py -- pull/load raw inputs; public data caches to data/raw/,
# licensed data is reconstructed by a pinned query on GMU infra (NOT shipped).
import numpy as np, pandas as pd
from config import SEED, DATA_RAW

def main():
    DATA_RAW.mkdir(parents=True, exist_ok=True)
    rng = np.random.default_rng(SEED)
    # Stand-in for a real pull (your WRDS/FRED/HMDA query goes here):
    raw = pd.DataFrame({"county": np.arange(240),
                        "state":  rng.integers(0, 30, size=240),
                        "x":      rng.normal(0, 1, size=240)})
    raw.to_parquet(DATA_RAW / "raw_panel.parquet")
    print(f"[01] wrote raw_panel.parquet ({len(raw)} rows)")

if __name__ == "__main__":
    main()
```

```python
# src/02_build_dataset.py -- build the analysis dataset from the raw cache.
# No look-ahead, no leakage (Chapter 7.4); pure function of the raw inputs.
import pandas as pd
from config import DATA_RAW, DATA_PROC

def main():
    DATA_PROC.mkdir(parents=True, exist_ok=True)
    raw = pd.read_parquet(DATA_RAW / "raw_panel.parquet")
    analysis = raw.assign(treated=(raw["state"] < 15).astype(int)).copy()  # .copy(): no chained assignment
    analysis.to_parquet(DATA_PROC / "analysis.parquet")
    print(f"[02] wrote analysis.parquet ({len(analysis)} rows)")

if __name__ == "__main__":
    main()
```

```python
# src/03_analysis.py -- estimate; WRITE every table to paper/tables/ and figure to
# paper/figures/. Deterministic via the single SEED. Nothing here is ever pasted by hand.
import matplotlib; matplotlib.use("Agg")     # headless: render to file, never to a screen
import matplotlib.pyplot as plt
import numpy as np, pandas as pd
from config import SEED, DATA_PROC, TABLES, FIGURES

def main():
    TABLES.mkdir(parents=True, exist_ok=True)
    FIGURES.mkdir(parents=True, exist_ok=True)
    rng = np.random.default_rng(SEED)

    _ = pd.read_parquet(DATA_PROC / "analysis.parquet")    # your real estimation reads this
    event_time = np.array([-3, -2, -1, 0, 1, 2, 3])
    true_beta  = np.where(event_time >= 0, -1.80, 0.0); true_beta[event_time == -1] = 0.0
    se  = rng.uniform(0.18, 0.30, size=event_time.size)
    est = true_beta + rng.normal(0.0, se); est[event_time == -1] = 0.0; se[event_time == -1] = 0.0
    post = event_time >= 0
    att   = float(est[post].mean())
    att_se = float(np.sqrt((se[post] ** 2).sum()) / post.sum())
    lo, hi = att - 1.96 * att_se, att + 1.96 * att_se

    # --- figure: pin the PDF metadata date so the bytes are reproducible (see 2.5) ---
    fig, ax = plt.subplots(figsize=(6, 3.6))
    ax.errorbar(event_time, est, yerr=1.96 * se, fmt="o", color="black", capsize=3)
    ax.axhline(0.0, color="0.4", lw=0.8)
    ax.set_xlabel("Event time (years since adoption)")
    ax.set_ylabel("Effect on denial gap (pp)")
    fig.tight_layout()
    fig.savefig(FIGURES / "event_study.pdf", bbox_inches="tight",
                metadata={"CreationDate": None})    # <- drop the timestamp; bytes now reproducible
    plt.close(fig)

    # --- table: rendered from the analysis variables, NEVER typed ---
    tex = ("% main_results.tex -- GENERATED by 03_analysis.py; do not edit by hand.\n"
           "\\begin{table}[t]\\centering\n"
           "\\caption{Effect on the minority--white denial gap}\\label{tab:main}\n"
           "\\begin{tabular}{lc}\\hline\\hline\n"
           f"Post-period ATT (pp) & {att:.2f} \\\\\n"
           f"95\\% CI & $[{lo:.2f},\\ {hi:.2f}]$ \\\\\n"
           "\\hline\\hline\n\\end{tabular}\n\\end{table}\n")
    (TABLES / "main_results.tex").write_text(tex, encoding="utf-8")
    print(f"[03] ATT={att:.3f} CI=[{lo:.3f},{hi:.3f}]; wrote table + figure")

if __name__ == "__main__":
    main()
```

In your real capstone, `03_analysis.py` is the nb8.3 / nb8.2 logic — your `pyfixest` regressions, your `etable` LaTeX, your event-study figure. The synthetic mini-analysis above is a stand-in so the skeleton runs end-to-end; the *artifact-writing discipline* is what transfers unchanged.

### 2.3 The `Makefile`

The `Makefile` is the one-click. Each target names what it produces; each depends on the targets that must run first, so `make` enforces the order for you. Write this as `Makefile` in the repo root — and note the one rule that trips up every beginner: **`make` recipe lines must be indented with a real TAB, not spaces** (a space-indented recipe gives the cryptic `*** missing separator` error):

```makefile
# Makefile -- one-click replication. `make all` rebuilds raw -> paper/main.pdf.
.PHONY: all data tables figures paper clean

PY  := python
SRC := src

all: paper                     ## the default target: build everything

data:                          ## pull/load raw, then build the analysis dataset
	$(PY) $(SRC)/01_pull_data.py
	$(PY) $(SRC)/02_build_dataset.py

tables: data                   ## estimate; write paper/tables/*.tex (deterministic via SEED)
	$(PY) $(SRC)/03_analysis.py

figures: data                  ## figures are written by the same analysis step
	$(PY) $(SRC)/03_analysis.py

paper: tables figures          ## compile the manuscript, pulling in the just-built artifacts
	cd paper && latexmk -pdf -interaction=nonstopmode main.tex

clean:                         ## delete every derived file so a rebuild is honest
	rm -rf data/processed/* paper/tables/* paper/figures/* \
	       paper/main.pdf paper/main.aux paper/main.log paper/main.fls \
	       paper/main.fdb_latexmk paper/main.out
```

The dependency chain is the lesson: `make all` wants `paper`, which needs `tables` and `figures`, which both need `data`, which runs the pull and the build. `make` walks that graph and runs each step once, in order — so `make data` rebuilds just the dataset, `make tables` rebuilds the dataset *and then* the tables, and `make all` does the whole thing to PDF. Run `python` not `python3` inside `make`, or set `PY := python3`, depending on what your environment names the interpreter — the camp container's `capstone` env exposes `python`.

### 2.4 An equivalent `run_all.py` (no `make` required)

`make` is not installed everywhere — Windows laptops especially — so ship a Python entry point that does the same thing. It runs each step as a subprocess (so a failure stops the build loudly, never silently half-building) and gates the LaTeX compile on whether an engine exists, the same `shutil.which` pattern nb8.5 uses:

```python
#!/usr/bin/env python
"""run_all.py -- the make-free one-click entry point.

    python run_all.py            run the whole pipeline raw -> PDF
    python run_all.py --clean    delete derived files first (the honesty test)
"""
import argparse, shutil, subprocess, sys
from pathlib import Path

ROOT = Path(__file__).resolve().parent
SRC  = ROOT / "src"

# Ordered pipeline: (human label, command). Order is the whole point.
STEPS = [
    ("pull raw data",              [sys.executable, str(SRC / "01_pull_data.py")]),
    ("build analysis data",        [sys.executable, str(SRC / "02_build_dataset.py")]),
    ("estimate + write artifacts", [sys.executable, str(SRC / "03_analysis.py")]),
]
DERIVED = [ROOT / "data" / "processed", ROOT / "paper" / "tables", ROOT / "paper" / "figures"]

def clean():
    """Delete every derived output so the rebuild starts from nothing."""
    for d in DERIVED:
        if d.exists():
            for p in d.iterdir():
                if p.name == ".gitkeep":
                    continue
                p.unlink() if p.is_file() else shutil.rmtree(p)
    pdf = ROOT / "paper" / "main.pdf"
    if pdf.exists():
        pdf.unlink()
    print("[clean] removed derived outputs")

def run_step(label, cmd):
    print(f"[run] {label}: {' '.join(cmd)}")
    subprocess.run(cmd, cwd=SRC, check=True)   # cwd=SRC so `from config import SEED` resolves

def compile_paper():
    """Compile IF a LaTeX engine is present; otherwise skip cleanly (the .tex is the deliverable)."""
    paper = ROOT / "paper"
    for engine, cmd in (("latexmk",  ["latexmk", "-pdf", "-interaction=nonstopmode", "main.tex"]),
                        ("tectonic", ["tectonic", "main.tex"])):
        if shutil.which(engine):
            print(f"[paper] compiling with {engine}")
            subprocess.run(cmd, cwd=paper, check=True)
            return True
    print("[paper] no LaTeX engine found; .tex is assembled and ready (build on Overleaf).")
    return False

def main():
    ap = argparse.ArgumentParser(description="One-click replication build.")
    ap.add_argument("--clean", action="store_true", help="delete derived files first")
    args = ap.parse_args()
    if args.clean:
        clean()
    for label, cmd in STEPS:
        run_step(label, cmd)
    compile_paper()
    print("Done. Rebuilt paper/main.pdf from raw. Compare to the committed PDF.")

if __name__ == "__main__":
    main()
```

Pick one entry point as your *documented* one (the README's one line) and keep the other as a fallback; they do the same work. `python run_all.py` is the equivalent of `make all`; `python run_all.py --clean` is `make clean && make all`.

### 2.5 The `make clean && make all` honesty test

Here is the test that proves the packet is real. Delete every derived file and rebuild from nothing:

```bash
make clean && make all          # or: python run_all.py --clean
```

If the paper comes back identical, the chain holds: no number in the PDF was typed by hand, because there was nothing left on disk to copy from — every table and figure was *regenerated* from raw inputs through your seeded code. If a number *drifts*, you just found a hand-edit, and you found it before a reviewer did. To check identity mechanically, hash the generated artifacts before and after:

```bash
make all
md5sum paper/tables/main_results.tex paper/figures/event_study.pdf > before.txt
make clean && make all
md5sum -c before.txt            # OK on every line == byte-identical rebuild
```

A subtlety worth knowing, because it bit the verification of this lab and it will bite you: **the table `.tex` will be byte-identical, but a figure saved as PDF will not be — unless you pin its metadata.** matplotlib stamps the *current wall-clock time* into the PDF's `/CreationDate`, so two runs of identical code produce figure files that differ in those few metadata bytes while the *plotted data is pixel-for-pixel the same*. The fix is in `03_analysis.py` above: pass `metadata={"CreationDate": None}` to `savefig` (or export `SOURCE_DATE_EPOCH`), and the figure bytes become reproducible too. Knowing *which* differences are cosmetic (a timestamp) versus *which* are real (a drifted coefficient) is the whole skill — a reviewer who sees the data unchanged does not care about a timestamp, but a drifted coefficient is a broken packet.

### 2.6 Commit the finished packet

The packet is the repository, so finishing it is a commit, not an upload:

```bash
git add src/config.py src/01_pull_data.py src/02_build_dataset.py src/03_analysis.py \
        Makefile run_all.py paper/main.tex
git commit -m "Wire one-click packet: Makefile/run_all + seeded pipeline -> paper.pdf"
git push
```

Do **not** commit `paper/main.pdf`, `paper/tables/*.tex`, or `paper/figures/*` if they are derived — they regenerate from `make all`, and committing them invites exactly the drift the packet prevents (add them to `.gitignore` alongside `data/`). The one nuance some advisors prefer: commit the *final* `main.pdf` as a released artifact so a reader without a TeX install can read the paper — if so, treat it as a build output you refresh with every result change, never as a source. And the rule from Lab 7 still governs: **no secrets, no licensed data, ever** — your `config.py` holds a seed, not an API key; keys come from `os.environ`, and the `.gitignore` you verified in Lab 7 is the wall.

---

## Part 3 — Dry-run the eight-minute talk

The paper and the packet are the two surfaces a skeptic touches alone. The talk is the surface they touch *with you in the room*, and it is where the work is actually judged. Chapter 8.5 gave you the six-beat arc and the question-period theory; Part 3 turns that into a rehearsal protocol you execute against a clock.

### 3.1 The timed rehearsal protocol

A budget you have not tested out loud is a wish. Rehearse the full talk, timed, **at least three times**, and cut until it fits in **seven minutes, not eight** — the eighth minute is your insurance against the cough, the lagging clicker, the pause you will take, not your content. Run the three passes deliberately, each with a different job:

- **Pass 1 — out loud, alone, timed, no stopping.** Even if a sentence comes out badly, keep going; the only goal is a total time. Write it down. It will be over eight minutes. Everyone's first pass is.
- **Pass 2 — cut, then re-time.** Find the slowest part (it is almost always the motivation, where nervous speakers pile on context) and cut it to the bone. Re-run. The target is the *shape* from Chapter 8.5 §3: fast through question-and-why, slow through design and headline, brisk through robustness and contribution.
- **Pass 3 — in front of one person, timed, and let them interrupt.** A peer who asks one question mid-talk simulates the real thing far better than a mirror. If their question knocks you off your time, that is the rehearsal working.

A bare timer is enough — your phone, or a one-liner you can run beside your slides on the container:

```bash
# a dead-simple per-slide timer: press Enter to advance, prints elapsed per slide and total
python - <<'PY'
import time
start = time.time(); last = start; slide = 1
print("Press Enter at each slide change; Ctrl-D to finish.")
try:
    while True:
        input(f"  [slide {slide}] running... ")
        now = time.time()
        print(f"  -> slide {slide}: {now-last:5.1f}s   (total {now-start:5.1f}s)")
        last = now; slide += 1
except EOFError:
    total = time.time() - start
    print(f"\nTOTAL: {total:5.1f}s  ({total/60:.1f} min)  --- target < 7:00")
PY
```

Watch *where* the time goes, not just the total. If the design slide is under a minute, you are rushing the spine of the talk; if a motivation slide is over ninety seconds, cut it. And the discipline that saves you live: **pre-decide your cut line.** Before you stand up, name the one slide you will skip if you reach minute six still on the result — almost always the second robustness detail or the third motivation point, *never* the design slide and *never* the headline. When you cut on the fly without a plan, you cut badly; when you have pre-decided, you cut cleanly and nobody notices.

### 3.2 Anticipating the threats-table questions

The questions you will get are not a surprise — they are **the rows of your threats-and-responses table from Chapter 7.5**, the one you wrote weeks ago when you were calm. Every hard question in an empirical seminar has the same shape: *"Isn't your estimate just picking up [some confounder] rather than the effect you claim?"* That is your identifying assumption being attacked at a weak point you already named in column 1.

So before the talk, turn each row of the threats table into a prepared four-part answer, and rehearse saying it in this order: the threat (concede it is the central worry, do not dismiss it), the signature (*if* the confounder were driving the result, what would we see?), what you did (the test or design choice from column 3), and the residual concern (what column 4 honestly admits remains). Said live: *"Yes, that's the central worry — if [confounder] were driving this, we'd see [signature]; we checked, and we see [reassuring result]; what I still can't fully rule out is [residual], though [a bound on how bad it could be]."* That answer makes you look *more* credible than a confident dismissal, because it shows you saw the problem before they did.

Build the prep table from your real threats table and rehearse from it like flashcards:

| Their likely question (threat) | Signature if true | What I did (col. 3) | Residual (col. 4) |
|--------------------------------|-------------------|---------------------|-------------------|
| "Were treated/control diverging pre-treatment?" | upward-sloping pre-period leads | event-study plot: flat leads, all CIs cross zero | can't prove parallel trends, only fail to reject |
| "Did you cluster at the right level?" | SEs too small if treatment is state-level | clustered by state (30 clusters), shown on robustness slide | few clusters — wild-cluster bootstrap as a check |
| "What about a contemporaneous policy?" | a jump at the same date from another cause | no other program adopted on this timing in these states | a state-specific shock I haven't named |

The first row is a *fatal*-class question (it attacks the comparison); the second is *survivable* (it attacks a number). Knowing which is which before you walk in is the next section.

### 3.3 The fatal-vs-survivable triage

This is the distinction that lets you keep your composure when a question lands hard. **A survivable critique attacks a number; a fatal critique attacks the comparison.**

A **survivable** critique asks whether a *choice* changes the answer: "cluster differently," "winsorize at 1% not 5%," "did you control for firm size?" These you answer with "we checked; it doesn't" (point at the robustness slide) or "fair — let me re-run it and report back." The estimate might move a little; it does not vanish. These map exactly to the *testable* threats from Chapter 7.5 §4 — the ones you could write a test for.

A **fatal** critique attacks the *identifying assumption itself*: "your treated and control groups were already diverging before treatment," "your instrument plausibly affects the outcome directly," "the same cutoff switches eligibility for a different program." If the attacker is right, no amount of re-clustering saves you — the comparison was never clean. These are the *arguable* (not testable) threats from Chapter 7.5 §4, and they are fatal precisely *because* you can only argue them, not test them.

The move when a fatal critique lands and the attacker is *right* is the hardest discipline in the whole camp, so rehearse it explicitly: **do not defend the indefensible.** Arguing harder in front of a room that can see the break converts a damaged paper into a damaged reputation. Instead, concede at the level the critique was made and retreat to what survives: *"You're right that I can't rule that out with this design — so the honest reading is that this is a strong association consistent with the mechanism, not the clean causal estimate I'd hoped for, and that's a real limitation."* A conceded fatal critique is a wounded paper but an intact scientist. And the retreat almost always lands somewhere real: the critique is usually fatal to the *causal* claim but not the *descriptive* one — "this is a robust pattern that future work with [the better design] should test causally" is a genuine, honest, publishable contribution. **Know your retreat position before you walk in.** That is what the residual-concern column was for.

The third move, for the question you genuinely cannot answer: say "I don't know" *with structure*, not as a shrug. *"I don't know — that's outside what my design can speak to, because [reason]; my guess is [direction], but I'd want to [the specific check] before claiming it."* A located "I don't know" proves the rest of your "I know"s are trustworthy.

---

## You're done when…

Use this checklist. A box is checked only when you can point at the command output or the file that proves it — this is the exact audit a referee, or Prof. Gao at the conference, would run.

- [ ] **Paper PDF compiles.** `cd paper && latexmk -pdf main.tex` (or `tectonic main.tex`, or Overleaf *Recompile*) produces `main.pdf` with a clean exit and no undefined references. The PDF has a title, abstract, the five sections, the `\input`ed table, and the `\includegraphics`ed figure.
- [ ] **No hard-coded results in `main.tex`.** Grep it: `grep -E "[0-9]\.[0-9]" paper/main.tex` returns only the year/version, never a coefficient — every number lives in `tables/` and `figures/`, generated by code.
- [ ] **One-click rebuild from raw.** `make all` (or `python run_all.py`) runs pull → build → analysis → compile with no human in the loop, on a clone that has never run before. Targets `data`, `tables`, `figures`, `paper`, `all`, `clean` all work.
- [ ] **Honesty test passes.** `make clean && make all` regenerates everything; `md5sum -c before.txt` shows the table `.tex` byte-identical (and the figure too, with the metadata date pinned). Any drift was hunted down and was a hand-edit.
- [ ] **SEED fixed and named.** A single `SEED` lives in `src/config.py`, is threaded through `np.random.default_rng(SEED)`, and is documented in the README as fixed before results. `grep -rn "np.random.seed\|default_rng" src/` shows no scattered or unseeded randomness.
- [ ] **No secrets, no licensed data.** `git ls-files` shows no `.env`, no `*.key`, no `*.parquet`, no CRSP/Compustat extract; keys are read from `os.environ`. (The Lab 7 `.gitignore` still holds.)
- [ ] **Talk fits in seven minutes.** Timed rehearsal ≥ 3×; final pass under 7:00 on the timer; cut line pre-decided and written on a sticky note.
- [ ] **Question prep done.** Each threats-table row has a prepared four-part answer; each is tagged fatal or survivable; your retreat position for the worst fatal critique is written out in full.
- [ ] **Submission assembled.** Paper PDF, repo URL (the Classroom repo — the repo *is* the submission), the slide deck, and the replication packet *verified* by a fresh-clone `make all`.

---

## Connecting to Assessment 8 — the capstone

This lab is the build bench; **Assessment 8 is the capstone itself** — the graded deliverable that the whole camp pointed at. Assessment 7 graded the *design half* of your project (the pre-analysis plan plus the identification memo, filed as a tagged commit) against a rubric it told you was "the one your Week-8 capstone will be graded against too." Assessment 8 grades the *execution half* on that same rubric: the analysis you ran after `pap-filed`, the robustness battery that survived (Chapter 8.2), the paper written so a referee believes it (Chapter 8.3), the peer review you incorporated (Chapter 8.4), and — the two surfaces this lab built — the eight-minute defense and the one-click packet.

The mapping is direct, so treat this checklist as a pre-grade audit. Assessment 8 asks: does `make all` on a fresh clone reproduce your headline number? (Part 2.) Does your paper state its specification in the CONVENTIONS §4 form and name what it does *not* claim? (Part 1's `main.tex` sections.) Does your talk survive the threats-table questions, and do you concede the fatal critique honestly rather than bluff it? (Part 3.) The repository you have been committing into since Lab 7 *is* the submission — there is no separate upload; what you push is what is graded, and the commit history (PAP tagged before results, deviations logged after) is part of the grade.

Then look at the **capstone gallery** in `book/capstones/` — five finished student papers, each with its talk and its one-click packet, as exemplars of the bar you are clearing. `[CHECK]` — the gallery is a stub at the time of writing; when it is populated, read one paper not for its *result* but for its *seams*: find its identification slide, find the residual concern its threats table admits, and run its `make all` to watch its headline emerge from raw data. That is how you will read every empirical paper from now on, and how you want yours read.

You started this camp computing that the average return was eight percent and thinking it was an answer. It was the first question. What you have now — a question nobody had cleanly answered, attacked with a design you can defend sentence by sentence, estimated with honest uncertainty, robust to the choices you worried about, written so a referee believes it, and packaged so a skeptic can rebuild every claim with one command — is an original empirical paper. The single sentence this whole camp existed to let you say truthfully is: *"Here is what I found, here is exactly why you should believe it, and here is the command that lets you check me."* Compile the paper, finish the packet, rehearse the defense. Then go say it.

---

### References

- American Economic Association. *Sample References and Author Resources* (the AEA article class and `aea.bst` BibTeX style). `https://www.aeaweb.org/journals/policies/templates` `[CHECK]` exact template/class filenames against the version the camp distributes.
- Gentzkow, M., & Shapiro, J. M. (2014). *Code and Data for the Social Sciences: A Practitioner's Guide.* University of Chicago. (Run-order, `make`, and the one-command-rebuild discipline.)
- Wilson, G., Bryan, J., Cranston, K., Kitzes, J., Nederbragt, L., & Teal, T. K. (2017). Good Enough Practices in Scientific Computing. *PLoS Computational Biology*, 13(6), e1005510.
- See also Chapter 8.5 (the talk and packet, in prose), Lab 7 (the repository this packet finishes), Chapter 7.5 (the threats table the question period reads from), Appendix D (the binding table/figure/packet standard), and CONVENTIONS §4–§5.
