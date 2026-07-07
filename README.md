# DataCamp Course Catalog Analysis

A strategic analysis of the DataCamp course catalog built on a custom dataset scraped from the platform's public course catalog. The project covers the full workflow: raw nested JSON → cleaned tidy dataset → exploratory analysis with business-oriented conclusions.

**Research question:** Which technologies attract the most learner interest, and which retain students best?

## Key findings

- **Python dominates volume** (~460k course starts), while **Theory courses lead retention** (~60% completion rate).
- **Git, GitHub and DVC courses are "hidden gems"** — modest audiences but exemplary completion rates.
- **n8n is a critical outlier** (~12% completion). Note: n8n courses are also among the newest in the catalog, and completion rates naturally lag behind course starts, so course age is a likely confounder here.
- The findings are summarized in a **Strategic Content Matrix** — a scatter of course starts (log scale) vs. completion rate, divided into four quadrants: STARS, GEMS, HYPE and NICHE.

## Data

The dataset covers **619 courses × 166 raw attributes**, captured from the Algolia search API that powers DataCamp's public course catalog. The data files are **not** included in this repository. To recreate them:

1. Open [datacamp.com/courses-all](https://www.datacamp.com/courses-all) in a browser with DevTools open (Network tab, filter: `Fetch/XHR`).
2. Page through the catalog and capture the Algolia query responses (JSON).
3. Save each response as `courses_all_p1.json`, `courses_all_p2.json`, … `courses_all_p21.json` **next to the notebook** (the notebook loads them with relative paths).

Keep in mind that the catalog changes over time — a fresh scrape will produce slightly different numbers than the ones shown in the executed notebook outputs.

## Method

1. **Loading and merging** — the 21 JSON pages are flattened with `pd.json_normalize` (Algolia `hits` record path) and concatenated.
2. **Cleaning** — programmatic detection and removal of single-value and non-English columns, type corrections, ordered categoricals (skill level, duration), author list split into separate columns; 166 columns reduced to ~20.
3. **Feature engineering** — `completion_rate` (completions / starts) and `days_since_launch`.
4. **Exploratory analysis** — starts vs. completions per technology, top/bottom retention ranking, and the Strategic Content Matrix.

## How to run

```bash
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook dc_course_analysis.ipynb
```

Place the scraped `courses_all_p*.json` files in the repository root before running the notebook (see **Data** above).

## Repository structure

```
dc_course_analysis.ipynb   # the full analysis (executed outputs included)
requirements.txt           # Python dependencies
README.md
```
