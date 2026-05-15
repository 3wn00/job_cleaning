# US Job Market Analysis – Monster.com Dataset

**A hands-on data cleaning + simple dashboard project using real, messy job listing data.**

Dataset: [US jobs on Monster.com – Kaggle](https://www.kaggle.com/datasets/promptcloud/us-jobs-on-monster-com)  
Records: ~22,000 job listings | Source: Monster.com crawl (2016–2017)

---

## What This Project Is

A small, focused learning project split into two phases:

1. **Phase 1 – Data Cleaning:** Dig into a real-world dataset full of inconsistencies, nulls, and messy text. Fix it. Document what you found.
2. **Phase 2 – Simple Dashboard:** Answer 3–4 meaningful questions about the US job market using clean visuals.

No complex pipelines. No machine learning. Just honest, methodical analysis.

---

## The Dataset

The raw CSV (`monster_com-job_sample.csv`) has 14 columns:

| Column | What's in it |
|---|---|
| `country` / `country_code` | Always "United States" / "US" |
| `date_added` | When the listing was crawled |
| `has_expired` | Always `false` – useless column |
| `job_board` | Always "jobs.monster.com" – useless column |
| `job_description` | Long free text – messy HTML/boilerplate |
| `job_title` | Often contains location, company name, and noise |
| `job_type` | Inconsistent: "Full Time", "Full Time Employee", "Full-Time" |
| `location` | City + state, sometimes with ZIP codes |
| `organization` | Company name – many nulls |
| `salary` | ~70% null; mixed formats when present |
| `sector` | Industry category – sometimes multiple values in one cell |

---

## Phase 1 – Data Cleaning

### Goal
Produce a clean dataframe you can actually query. Log every issue you find.

### What to Look For

Work through these cleaning tasks in order:

**1. Drop useless columns**
- `has_expired` and `job_board` have one value each. Drop them early.
- `country` and `country_code` are always "United States" / "US". Drop or note them.

**2. Fix `job_type` inconsistencies**
- `"Full Time"`, `"Full Time Employee"`, `"Full-Time"`, `"Full Time, Employee"` should all map to `"Full Time"`
- Same problem with `"Part Time"` variants
- Decide how to handle `"Temporary/Contract/Project"` – is that one category or three?

**3. Parse `location`**
- Extract city and state into separate columns
- Handle entries with ZIP codes (`"Houston, TX 77040"`) vs without (`"Houston, TX"`)
- Watch for entries that are just a ZIP code, or just a state

**4. Clean `job_title`**
- Many titles include the location: `"IT Support Technician Job in Madison"`
- Strip `" Job in [City]"` suffix where it appears
- Note: some titles are actually full HTML page titles – flag those as unparseable

**5. Handle `salary`**
- Most are null – that's fine, don't try to fill them
- For the ones that exist: are they hourly, annual, or ranges? Try to extract a consistent format
- If it's too messy, document why and leave it as-is

**6. Handle `sector`**
- Some rows have multiple sectors in one cell: `"Hotels and Lodging Personal and Household Services"`
- Decide: take the first one, split into a list, or flag as multi-sector
- Note the null rate

**7. Check `date_added`**
- Confirm it parses as a proper date
- What's the date range of this dataset?

### Deliverable: Data Quality Log

Keep a simple markdown or notebook cell log as you work:

```
| Column      | Issue Found                              | Action Taken          |
|-------------|------------------------------------------|-----------------------|
| job_type    | 6+ variants for "Full Time"              | Standardized to 3 categories |
| location    | 340 rows missing state                   | Extracted what's possible, flagged rest |
| salary      | 71% null                                 | Kept as-is, noted in analysis |
| ...         | ...                                      | ...                   |
```

---

## Phase 2 – Simple Dashboard

### Goal
Answer 3–4 questions using clean charts. Keep it simple: one chart per question.

### Questions to Answer

**Q1. Which sectors are hiring most?**
Bar chart – top 10 sectors by listing count.

**Q2. What job types dominate?**
Pie or bar chart – Full Time vs Part Time vs Contract.

**Q3. Where are the jobs? (Top cities/states)**
Horizontal bar chart – top 15 locations by listing count.

**Q4. How did listings change over time?**
Line chart – listings per month. (Careful: this dataset only covers ~11 months.)

### Tools

Start with whatever you're comfortable with:

| Option | When to use it |
|---|---|
| Python + matplotlib/seaborn | Good for learning charting fundamentals |
| Python + plotly | If you want interactive charts with minimal effort |
| Power BI / Tableau | If you want to practice BI tooling |
| Excel | Totally valid for a first pass |

> Recommendation: do Phase 1 in Python (pandas), then decide on the dashboard tool.

---

## Suggested File Structure

```
monster-jobs-analysis/
├── data/
│   ├── raw/
│   │   └── monster_com-job_sample.csv   # original, never edit this
│   └── clean/
│       └── jobs_clean.csv               # your output from Phase 1
├── notebooks/
│   ├── 01_data_cleaning.ipynb
│   └── 02_dashboard.ipynb
├── data_quality_log.md                  # your cleaning notes
└── README.md
```

---

## What You'll Actually Learn

- How to audit a dataset before touching it
- Dealing with inconsistent categorical values (the `job_type` problem is very realistic)
- Parsing mixed-format text columns
- Deciding when *not* to clean something (salary here)
- Translating a clean dataset into a small, honest story with charts

---

## Questions Worth Asking Once You're Done

- Which sector has the highest proportion of contract/temp roles vs full-time?
- Are certain cities dominated by one sector?
- What does a "messy" job title tell you about how the company posted the listing?

These aren't required – they're the natural next step if you want to go deeper.
