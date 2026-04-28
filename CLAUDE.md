# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project nature

This is a **research project, not a software package**. All analysis lives in three Jupyter notebooks; there are no `.py` modules, no test suite, no CI. The repo accompanies an academic publication ("Counting Worlds", Chapter 7 of *Aspects of Nigeria, an Emerging African Regional Power*, 2026) and analyzes early Lagos newspapers (Lagos Observer 1882–1888, Lagos Weekly Record 1891–1930s).

## Setup and run

```bash
pip install -r requirements.txt
jupyter notebook Counting_Worlds_cleaned.ipynb   # full analysis (116 cells)
jupyter notebook demo_en.ipynb                   # demo, English
jupyter notebook demo_ja.ipynb                   # demo, Japanese
```

The demo notebooks run end-to-end against `sample_data/` (5 fictional 1882 articles per file). The main notebook expects real CSVs under `./data/` — these are **not in the repo** for copyright reasons, so the main notebook cannot be executed in a fresh clone.

## Code architecture

### `load_newspaper_data()` is the integration point

The entire pipeline is built around one unified loader function defined in **each of the three notebooks** (duplicated, not imported from a shared module). It auto-detects newspaper format from filename substrings and normalizes column names:

| Format | Detection | Source columns | Normalization |
|---|---|---|---|
| LOE / LWRE (editorials) | `LOE` / `LWRE` in filename | `id, text, Publication Date, Year, Years` | `Publication Date` → `date` |
| LOC (correspondences) | `LOC` in filename | `no, id_1, text, year, date` | `no` → `id`, `id_1` → `composite_id` |

After loading, every dataframe has the same shape with added metadata columns `data_source` and `article_type`. Downstream cells assume this normalized schema. **When changing the loader, update all three notebooks** — there is no single source of truth.

For non-LOE/LOC/LWR newspapers the caller must pass `data_type=` and `data_source=` explicitly, otherwise auto-detection silently falls through.

### Analysis pipeline (main notebook only)

The flow in `Counting_Worlds_cleaned.ipynb` is sequential and stateful — cells depend on variables defined earlier:

1. Load LOE + LOC + LWRE via `load_newspaper_data()` and concatenate
2. Preprocessing (spaCy tokenization, stopwords, lemmatization)
3. Basic stats (article counts, vocabulary diversity over time)
4. TF-IDF and LDA topic modeling (8–10 topics) via gensim + pyLDAvis
5. **Geographic representation analysis** at four nested scales: Local (Lagos/Yoruba) → Regional (West Africa) → Continental (Africa) → Global (Europe/America/Asia). Word lists for each scale are defined inline; expanding them is the most common type of edit.
6. Pronoun analysis — We/Our vs They/Their co-occurrence with the geographic scales above
7. "Native" terminology frequency and context over time
8. Sentiment via TextBlob

Outputs are written to sibling folders that the notebook creates on first run: `basic_stats/`, `text_mining/`, `geographic_analysis/`, `pronoun_analysis/`, `visualizations/`. These folders are gitignored.

### Demo notebooks

`demo_en.ipynb` and `demo_ja.ipynb` are **introductory tutorials, not reduced versions of the main analysis**. They cover only steps 1–3 (load + basic stats) using `sample_data/`. The two are byte-equivalent except for the language of markdown cells; any code change must be mirrored.

## Editing notebooks

- Bilingual documentation is a hard requirement: every user-facing change to README or demo notebooks must be made in both EN and JA versions (`README_EN.md` / `README_JA.md`, `demo_en.ipynb` / `demo_ja.ipynb`). The `README.md` at repo root is just a language switcher.
- The main notebook is ~2 MB with embedded outputs and base64 images. Prefer targeted edits over re-running and committing fresh outputs unless the user asks for it — large diff churn is expensive in review.
- Notebook cells contain a few "改善予定" (planned improvement) comments around geo-entity tagging, spaCy model size, CSV output format, and dedup logic. Treat these as backlog notes, not bugs.

## Data conventions

- Real data goes in `./data/` (gitignored, not present). Filenames include the format tag (LOE/LOC/LWRE) so the loader can dispatch.
- Sample data in `sample_data/` is **synthetic** — fictional 1882 articles for demo only, not historical.
- The loader is filename-driven; renaming a CSV without an LOE/LOC/LWR tag will break auto-detection silently.
