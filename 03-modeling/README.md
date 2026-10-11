# Module 3: Analytical Modeling

Homework: https://github.com/DataTalksClub/stock-markets-analytics-zoomcamp/blob/main/cohorts/2026/homework3.md

Base notebook (course): https://github.com/DataTalksClub/stock-markets-analytics-zoomcamp/blob/main/03-modeling/%5B2026%5D_Module_03_Colab_Time_Series_Modeling.ipynb

## Setup

- Dataset: `stocks_df_combined_2026_09_18.parquet.brotli` (33 large-cap stocks from the US, EU and India, ~144 MB), downloaded at runtime via `gdown` from the course's shared Google Drive link. Not committed: exceeds GitHub's 100 MB file limit.
- Sample: 2000-01-03 to 2026-09-18, ~202k ticker-day rows.
- Target: `is_positive_growth_30d_future` (price higher in 30 trading days).
- Temporal split: train 2000-01-03 – 2018-09-13, validation 2018-09-14 – 2022-09-15, test 2022-09-16 – 2026-09-18.
- All Decision Trees use `random_state=42` for reproducibility.

## Q1 — Dummies for Month and Week-of-Month

## Q2 — New "hand" rules on macro and technical indicator variables

## Q3 — Unique correct predictions of a 10-level Decision Tree (`pred5_clf_10`)

## Q4 — Hyperparameter tuning for a Decision Tree

## Q5 — [Exploratory] What data is missing?

## Progress

- [ ] Q1 — highest absolute correlation of a `month_wom` dummy with the target (3 decimals)
- [ ] Q2 — precision of the best new hand rule (`pred3` / `pred4`) on TEST (3 decimals)
- [ ] Q3 — TEST records where only `pred5_clf_10` is correct
- [ ] Q4 — optimal `max_depth` (1–12) by TEST precision
- [ ] Q5 — proposed new indicators and data sources

## Files

- [`homework_3.ipynb`](homework_3.ipynb) — working notebook with all solutions

## Submission

- Form: https://courses.datatalks.club/sma-zoomcamp-2026/homework/hw03
- Leaderboard: https://courses.datatalks.club/sma-zoomcamp-2026/leaderboard
