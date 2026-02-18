# Copilot Instructions

## Project Overview

This is a **Sphinx documentation site** for VoIP content, deployed to GitHub Pages. Source files live in `docs/` (reStructuredText), and the built output goes to `output/`.

## Build Commands

```bash
# Install dependencies
pip install -r requirements.txt

# Build HTML (single-page)
make html

# Build dirHTML (directory-based, used for GitHub Pages deployment)
make dirhtml

# Live-reloading dev server
sphinx-autobuild docs output/html

# Install Sphinx tooling (one-time)
pipx install sphinx
pipx install sphinx-autobuild
```

> CI builds with `make dirhtml` and deploys `output/dirhtml/` to GitHub Pages on pushes to `main` or `dev`.

## Architecture

```
docs/          # RST source files
  conf.py      # Sphinx config (theme: alabaster, project: voip)
  index.rst    # Root toctree — includes year-level indexes via glob
  <year>/      # e.g. 2000/, 2001/, 2002/
    index.rst  # Year-level toctree — includes month subdirs via glob
    <month>/   # e.g. 01/, 02/
      index.rst  # Month-level content page
output/        # Build artifacts (not committed)
requirements.txt  # sphinx, furo
```

### Content hierarchy

The site is organized by **year → month**, using `:glob:` in each `toctree` to automatically pick up subdirectories. Adding a new year or month only requires creating the folder with an `index.rst` following the same pattern — the parent toctree picks it up automatically.

## Key Conventions

- All content is written in **reStructuredText** (`.rst`).
- The root `docs/index.rst` uses `:glob:` with patterns `*`, `2000/*`, `2001/*`, `2002/*` — add a new year pattern here when starting content for a new year.
- Theme is `alabaster` (configured in `docs/conf.py`). `furo` is listed in `requirements.txt` but not currently active.
- Build output directory is `output/` (set via `BUILDDIR` in `Makefile`), not the Sphinx default `_build/`.
- The CI workflow (`sphinx.yaml`) only triggers on `main` and `dev` branches.
