# Site build & deploy guide

This document explains how the textbook website is built (Quarto), how
to render it locally for editing, and how the GitHub Actions workflow
deploys it to GitHub Pages.

## What gets built

`_quarto.yml` declares a **Quarto book project** that includes:

- `index.qmd` — landing page (hero, quick-stats, 8-week grid)
- `about.qmd`, `404.qmd`
- every chapter under `book/00-front-matter/`, `book/weeks/week-XX/`,
  `book/capstones/`, `book/instructor-manual/`
- every appendix under `book/appendices/A-..E-..`
- every notebook under `notebooks/week-XX/*.ipynb`
- every data card under `data-cards/*.md`

Output is **HTML only** (KaTeX math, full-text search, dark/light
themes). Notebooks are **not executed** on CI or during local render
(`execute.enabled: false` in `_quarto.yml`); their .ipynb output cells
are rendered as-is, and students run them in Colab/Binder/locally via
the launcher buttons that the include script injects.

## Install Quarto (one-time)

macOS:

    brew install --cask quarto

Linux (Ubuntu / WSL):

    # See https://quarto.org/docs/get-started/ for the latest .deb
    curl -fsSL https://quarto.org/download/latest/quarto-linux-amd64.deb -o /tmp/quarto.deb
    sudo dpkg -i /tmp/quarto.deb

Verify:

    quarto --version
    quarto check

## Render locally

From the repo root:

    # Live-reload preview at http://localhost:4200
    quarto preview

    # One-shot full render into ./_site/
    quarto render

    # Render a single file (faster iteration on the landing page)
    quarto render index.qmd --to html

The first full render takes a minute or two because of the 40
notebooks; subsequent renders are cached via the `freeze` directory
(`_quarto.yml` sets `freeze: true`). To force re-render of a file,
delete its entry in `_freeze/` or pass `--cache-refresh`.

## Theme

- Light: **cosmo** + `styles.scss`
- Dark:  **darkly** + `styles.scss`

`styles.scss` overrides typography (serif body, sans nav, JetBrains
Mono code), color palette (restrained academic blue on warm paper),
and adds the landing-page hero/quick-stats/week-grid layout. Edit
`styles.scss` and re-render to see changes.

## Notebook launcher buttons

`_includes/notebook-buttons.html` is included into the `<head>` of
every page (`format.html.include-in-header` in `_quarto.yml`). It is a
small client-side script that detects whether the current page was
rendered from an `.ipynb` source. If so, it injects four buttons under
the page title:

1. **Open in Colab** — `https://colab.research.google.com/github/<owner>/<repo>/blob/<branch>/<path>.ipynb`
2. **Open in Binder** — `https://mybinder.org/v2/gh/<owner>/<repo>/<branch>?filepath=<path>.ipynb`
3. **Download .ipynb** — direct raw.githubusercontent.com link
4. **View on GitHub** — the source `.ipynb` file

The repo owner, repo name, and branch are hard-coded at the top of
`_includes/notebook-buttons.html`. If you fork the repo, update those
three constants.

Binder works because `environment.yml` lives at the repo root.

## GitHub Actions deploy

`.github/workflows/quarto-publish.yml` runs on every push to `main` and
on manual dispatch from the Actions tab. It:

1. Checks out the repo (full history — required by the publish action).
2. Installs Quarto (`quarto-dev/quarto-actions/setup@v2`).
3. Runs `quarto check` for diagnostics.
4. Renders the 8-week book to `_site/` as HTML and PDF
   (`empirical-finance-camp-8wk.pdf`), and the per-week PowerPoint decks
   (Weeks 1–8) into `_site/decks/`.
5. Pushes the rendered `_site/` to the **`gh-pages`** branch via
   `peaceiris/actions-gh-pages@v4` (`force_orphan: true`).

`force_orphan` lets the action create the `gh-pages` branch on the first
run, so no pre-existing branch is needed.

### Enable GitHub Pages (one-time)

1. Go to repository **Settings → Pages**.
2. Under **Build and deployment**:
   - Source: **Deploy from a branch**
   - Branch: **`gh-pages`** / **`/ (root)`**
3. Click **Save**.
4. After 30-60 seconds the site appears at
   `https://<owner>.github.io/<repo>/`.

### Custom domain (optional)

Add a `CNAME` file at the repo root containing your domain (e.g.
`camp.example.edu`) and configure your DNS provider to CNAME that
hostname to `<owner>.github.io`. GitHub Pages will pick up the CNAME
file on the next deploy.

## Troubleshooting

**Build fails on a single chapter.** Quarto's error message names the
file and line. Comment that entry out of the `chapters:` list in
`_quarto.yml`, re-render to confirm the rest builds, then fix the file.

**Math doesn't render.** Confirm `html-math-method: katex` is still in
`_quarto.yml` and that the equation uses standard LaTeX syntax. KaTeX
supports most but not all of LaTeX — see
<https://katex.org/docs/supported.html>.

**Notebook output looks stale.** `freeze: true` keeps previously
rendered outputs. Delete the relevant subdirectory under `_freeze/` and
re-render. (Or just delete the whole `_freeze/` folder.)

**Launcher buttons not showing on a notebook page.** Open the page in
the browser and check the console. The script needs the `<meta
name="quarto:source-file">` tag (Quarto 1.4+) or a URL ending in
`notebooks/week-XX/...html`. If neither matches, the script silently
no-ops by design.

## Local clean

    rm -rf _site _freeze .quarto

That is safe: the next `quarto render` will rebuild everything.
