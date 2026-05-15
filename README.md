# US Job Market Analysis – Monster.com

A personal learning project where I practice data cleaning on a real, messy dataset and then build a simple dashboard to answer a few questions about the US job market.

**Dataset:** [US jobs on Monster.com](https://www.kaggle.com/datasets/promptcloud/us-jobs-on-monster-com) – ~22,000 job listings crawled from Monster.com between 2016 and 2017.

---

## Why This Project

I wanted to practice the part of data analysis that tutorials usually skip over – dealing with a dataset that hasn't been pre-cleaned for you. This one has inconsistent categories, a lot of nulls, messy free text, and a few columns that are completely useless. Good practice.

---

## Project Structure

```
monster-jobs-analysis/
├── data/
│   ├── raw/
│   │   └── monster_com-job_sample.csv
│   └── clean/
│       └── jobs_clean.csv
├── notebooks/
│   ├── 01_data_cleaning.ipynb
│   └── 02_dashboard.ipynb
├── data_quality_log.md
└── README.md
```

---

## The Dataset

14 columns, most of which need work:

| Column | Notes |
|---|---|
| `job_title` | Often includes location and noise, e.g. *"IT Support Technician Job in Madison"* |
| `job_type` | Inconsistent – "Full Time", "Full Time Employee", "Full-Time" all mean the same thing |
| `location` | City + state, sometimes with ZIP codes attached |
| `sector` | Industry category – some rows have multiple sectors crammed into one cell |
| `salary` | ~70% null, mixed formats when present |
| `job_description` | Long free text, some entries contain raw HTML |
| `has_expired` / `job_board` | Always the same value – dropped immediately |

---

## Phase 1 – Data Cleaning

My cleaning process, roughly in order:

1. **Drop useless columns** – `has_expired`, `job_board`, `country`, and `country_code` carry no information
2. **Standardize `job_type`** – mapped the many variants down to three categories: Full Time, Part Time, and Contract
3. **Parse `location`** – split city and state into separate columns, handled entries with and without ZIP codes
4. **Clean `job_title`** – stripped the `" Job in [City]"` suffix that appears on most titles; flagged the ones that are unparseable
5. **Handle `sector`** – some rows list multiple sectors in one field; I took the first one and flagged multi-sector rows
6. **Leave `salary` alone** – too inconsistent and too sparse to clean meaningfully; documented why in the quality log

I kept a [data quality log](data_quality_log.md) throughout, noting every issue found and what I did about it.

---

## Phase 2 – Dashboard

Four questions I wanted to answer with the clean data:

- **Which sectors are hiring most?** – top 10 sectors by listing count
- **What job types dominate?** – Full Time vs Part Time vs Contract breakdown
- **Where are the jobs?** – top 15 cities and states
- **How did listings change over time?** – monthly trend across the ~11-month window

Built in Python using pandas and matplotlib/seaborn.

---

## Tools

- Python, pandas, matplotlib, seaborn

---

## What I Learned

- How to audit a dataset before touching it
- That deciding *not* to clean a column is a valid and sometimes correct choice
- How inconsistent real categorical data is compared to tutorial datasets
- How to turn a messy dataset into a small, honest visual story
