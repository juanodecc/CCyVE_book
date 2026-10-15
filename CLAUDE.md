# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A Jupyter Book (v1, `jupyter-book<2`) for the course "Control Clásico y por Variables de Estado" (Ingeniería Electrónica, Universidad Nacional de Río Negro). Content is in Spanish (`language: es`). Published at https://juanodecc.github.io/CCyVE_book.

## Build

```bash
conda env create -f environment.yml   # env name: book (python 3.11, control, slycot, sympy, jupytext, ...)
conda activate book
git submodule update --init --recursive   # content lives in the submodule
jupyter-book build .                      # output in _build/html
jupyter-book clean .                      # clear build/execution cache
```

Notebooks are executed at build time (`execute_notebooks: cache`), so a code error in any page breaks or degrades the build. `slycot` must come from conda (pip install fails in CI — that's why the workflow uses Miniconda).

## Structure

- **Content lives in the `sources/` git submodule** (`juanodecc/control_book_source`), shared across several books. Edits to chapters must be committed and pushed inside `sources/` first, then the submodule pointer committed in this repo. This repo only holds book-specific config plus `ccyve_sources/` (the landing page `intro.md` and images).
- `_toc.yml` selects and orders which submodule notebooks are included in this book (root is `ccyve_sources/intro`). Adding a page to the book = adding its path here (without extension).
- Chapters are **MyST Markdown notebooks paired via jupytext** (`formats: ipynb,md:myst`, `{code-cell} ipython3` blocks); edit the `.md` files. `.py` files are also registered as jupytext notebooks in `_config.yml`.
- `_config.yml` `repository.url` points to the sources repo (edit/issue buttons), while `baseurl` and utterances comments point to this repo.

## Deploy

`.github/workflows/book.yml`: every push to `master` checks out with submodules, builds with the conda env, and publishes `_build/html` to GitHub Pages (`gh-pages` branch). There is no separate test/lint step — a successful `jupyter-book build .` is the check.
