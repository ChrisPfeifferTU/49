<!-- .github/copilot-instructions.md -->
# Copilot instructions for the Thermolab (TN13015) repo

This file gives concise, actionable guidance for AI coding agents working on this repository.

- Purpose: the repo is a Jupyter Book-style portfolio for a course. Main outputs are a static site built with MyST/Jupyter Book and exported artifacts (PDF/typst).

- Key locations:
  - `Content/` — primary learning material and notebooks (e.g., `Content/Labs`, `Content/PySim`).
  - `toc.yml` — canonical table-of-contents; any added notebook must be referenced here to appear in the site.
  - `myst.yml` — project metadata and site options; it `extends` `plugins.yml`, `toc.yml`, and `export.yml`.
  - `css/` — site styles (see `myst.yml` `style` setting pointing at `css/TUD_stylesheet.css`).
  - `Figures/` — image assets referenced from the book pages.
  - `.github/workflows/deploy.yml` — GitHub Actions deploy flow (used for CI and GitHub Pages). The workflow builds with `myst` and deploys `_build/html`.

- Build & preview (reproducible locally):
  - Install Python deps: `pip install -r requirements.txt` (uses `jupyter-book`, `myst` ecosystem, `ipywidgets`, etc.).
  - Match CI: install the MyST CLI used in CI via npm: `npm install -g mystmd`.
  - Build HTML: `myst build --html` (this is the exact command used in CI).
  - Serve locally for verification: `python -m http.server --directory _build/html 8000` and open `http://localhost:8000`.
  - Alternative: `jupyter-book build .` often works, but prefer `myst build --html` to match CI.

- Common developer tasks & conventions:
  - When adding or updating notebooks, also update `toc.yml` so they are included in the site navigation.
  - Keep figures in `Figures/` and reference them with relative paths (examples: `Figures/Cover.jpg`, `Figures/logo.png`).
  - Styling is centralized in `css/TUD_stylesheet.css`; if making site-wide visual changes edit that file.
  - `myst.yml` contains the `BASE_URL` and `site` options; CI sets `BASE_URL` to `/${{ github.event.repository.name }}` — be mindful if deploying to a different path.

- Testing & QA:
  - There are no unit tests in the repo. Validate changes by building the site locally (`myst build --html`) and opening the generated `_build/html`.
  - For notebook-specific checks, open the notebook in JupyterLab (dependencies from `requirements.txt`) and run cells to ensure outputs render.

- Patterns and examples to follow:
  - Content is organized by folder and referenced in `toc.yml`. Example entry:
    - `- file: Content/Labs/Warmtecapaciteit.ipynb`
  - Plugins are declared in `plugins.yml` and are loaded by `myst.yml` (do not remove without checking effect on builds).

- When editing this repo, prefer minimal, focused changes:
  - Update `toc.yml` and `myst.yml` only when necessary.
  - Keep notebook metadata intact; do not strip outputs unless explicitly asked.

- Housekeeping & deployment notes:
  - CI triggers on `push` to `main` and deploys to GitHub Pages. The workflow requires Node 18 and `mystmd` (installed via npm) as used in `.github/workflows/deploy.yml`.
  - If changing site layout or base URL, update `myst.yml` and verify a local build.

If any of these areas are unclear or you want the instructions expanded (for example: step-by-step local dev setup, recommended virtualenv commands, or a checklist for adding new lab notebooks), tell me which section to expand and I'll update this file.
