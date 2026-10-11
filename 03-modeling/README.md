# Module 3: Analytical Modeling

Homework: https://github.com/DataTalksClub/stock-markets-analytics-zoomcamp/blob/main/cohorts/2026/homework3.md

Base notebook (course): https://github.com/DataTalksClub/stock-markets-analytics-zoomcamp/blob/main/03-modeling/%5B2026%5D_Module_03_Colab_Time_Series_Modeling.ipynb

## Setup

- Dataset: `stocks_df_combined_2026_09_18.parquet.brotli` (33 large-cap stocks from the US, EU and India, ~144 MB), downloaded at runtime via `gdown` from the course's shared Google Drive link. Not committed: exceeds GitHub's 100 MB file limit.
- Sample: 2000-01-03 to 2026-09-18, ~202k ticker-day rows.
- Target: `is_positive_growth_30d_future` (price higher in 30 trading days).
- Temporal split: train 2000-01-03 – 2018-09-13, validation 2018-09-14 – 2022-09-15, test 2022-09-16 – 2026-09-18.
- All Decision Trees use `random_state=42`.
- Environment: GitHub Codespaces (the notebook also runs in Colab).

## Results

| Question | Result in this notebook | Submitted option |
|---|---|---|
| Q1: max abs. correlation of a `month_wom` dummy | 0.025 (`October_w4`) | 0.025 |
| Q2: precision of the best new hand rule on TEST | 0.588 (`pred3_manual_dgs10_5`) | 0.588 |
| Q3: TEST rows where only `pred5_clf_10` is correct | 1,243 | 1171 (closest option) |
| Q4: optimal `max_depth` (1–12) by TEST precision | 4 | 4 |

## Q1 — Dummies for Month and Week-of-Month

Week of month = `(day - 1) // 7 + 1`: 7-day blocks of calendar days, so `w5` covers days 29–31. The month name is taken from `Date`, because the course code converts `Month` to a month number. Dummies are generated for `Month`, `Weekday`, `Ticker`, `ticker_type` and `month_wom`.

The strongest `month_wom` dummy is `October_w4` with |corr| = 0.025. Small in absolute terms, but a dummy that covers ~1/60 of the rows can reach at most |corr| ≈ 0.11 against a target with ~57% positives. Caveats: the maximum over ~60 dummies is inflated by chance, and the correlation is computed on the full sample, TEST included.

## Q2 — New "hand" rules on macro variables

Two rules from the positive branches of the 2026 tree:

- `pred3_manual_dgs10_5`: `(DGS10 <= 4.5) & (DGS5 <= 4)`
- `pred4_manual_dgs10_fedfunds`: `(DGS10 > 4) & (FEDFUNDS <= 4.795)`

Best: `pred3`, precision 0.588 on TEST. Reference points on the same TEST set: "always predict 1" gives ≈0.570 (the base rate); the course rules give 0.565 (`pred0`), 0.579 (`pred1`) and 0.573 (`pred2`).

Both rules use only US macro variables, so on any date all 33 tickers get the same prediction. These are market-timing signals with roughly one independent decision per trading day, not stock picks.

## Q3 — Unique correct predictions of a 10-level Decision Tree

`DecisionTreeClassifier(max_depth=10, random_state=42)`, fitted on TRAIN+VALIDATION with the new `month_wom` dummies; predictions for the whole dataset are stored as `pred5_clf_10`. `only_pred5_is_correct` = the tree is correct and all hand rules `pred0`–`pred4` are wrong. "Correct" is symmetric: a correct 0 counts as much as a correct 1.

Result: 1,243 TEST rows; the closest form option (1171) was submitted. Q1, Q2 and Q4 match the form options exactly, so the data pipeline is the same as the reference, and the gap comes from the depth-10 tree itself. Macro variables are constant across tickers within a day and change monthly, so deep nodes often contain several features that split the rows identically. Such ties are broken by scikit-learn's internal feature permutation, which depends on `random_state`, column order and library version, and each different choice changes the whole branch below it. `random_state=42` makes the result reproducible within one environment, not across environments.

## Q4 — Hyperparameter tuning for a Decision Tree

Trees with `max_depth` 1–12 were fitted on TRAIN+VALIDATION. The highest TEST precision is at depth 4; this tree is stored as `pred6_clf_best`. A shallow tree winning is consistent with the theory: deeper trees use slow macro variables and cumulative indicators (OBV, A/D line) as time stamps and memorise past periods.

The depth is selected on TEST, and TEST precision is reported for that same choice, which is optimistic. The clean protocol (fit on TRAIN, select on VALIDATION, evaluate on TEST once) is included in the notebook as an optional cell.

## Q5 — [Exploratory] What data is missing?

The macro block is US-only (CPI, Fed funds, Treasury yields), while 21 of the 33 companies are European or Indian. In the course run the tree's most important features are slow macro variables (core CPI, Fed funds, 365-day growth of gold and oil), so the biggest gaps are regional and regime data:

1. **Local monetary policy and inflation**: ECB deposit facility rate (FRED: `ECBDFR`), euro-area HICP, RBI repo rate and India CPI.
2. **Currencies**: EUR/USD (FRED: `DEXUSEU`) and USD/INR (FRED: `DEXINUS`). Several tickers are US-listed shares of foreign companies (HDB, IBN, INFY, NVO, TTE), so their USD returns mix company performance with FX moves.
3. **Regional benchmarks**: Nifty 50 (`^NSEI`), Euro Stoxx 50 (`^STOXX50E`) and CAC 40 (`^FCHI`) from Yahoo Finance, which also allow relative strength of a stock vs its own market.
4. **Market-wide risk sentiment**: VIX (FRED: `VIXCLS`) and India VIX.
5. **Yield-curve spread as a ready feature**: 10Y minus 2Y (FRED: `T10Y2Y`) instead of separate yield levels.
6. **Earnings**: report dates and surprises, since a 30-day window often contains a report.

Cumulative indicators (OBV, A/D line) also need a transformation: they trend over time and work as time stamps for the tree; their 30-day changes or z-scores would generalise better.

## Methodological notes

- **Labels at the end of the sample.** The last 30 trading days have no 30-day future (`growth_future_30d` is NaN), and their label is 0. This penalises any rule that fires at the end of TEST. The rows are kept for consistency with the course and the grading.
- **No gap between splits.** The 30-day target windows overlap, so the last ~30 days of VALIDATION have labels computed from TEST-period prices. A gap (embargo) between splits would be cleaner.
- **Test-set selection in Q4**, see above.

## Progress

- [x] Q1 — highest absolute correlation of a `month_wom` dummy with the target
- [x] Q2 — precision of the best new hand rule on TEST
- [x] Q3 — TEST records where only `pred5_clf_10` is correct
- [x] Q4 — optimal `max_depth` by TEST precision
- [x] Q5 — proposed new indicators and data sources

## Files

- [`homework_3.ipynb`](homework_3.ipynb) — working notebook with all solutions

## Submission

- Form: https://courses.datatalks.club/sma-zoomcamp-2026/homework/hw03
- Leaderboard: https://courses.datatalks.club/sma-zoomcamp-2026/leaderboard
