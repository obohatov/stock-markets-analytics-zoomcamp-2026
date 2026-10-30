# Module 4 — Trading Strategy and Simulation

Homework for Module 4 of the [Stock Markets Analytics Zoomcamp 2026](https://github.com/DataTalksClub/stock-markets-analytics-zoomcamp) (DataTalks.Club).
Task: [homework4.md](https://github.com/DataTalksClub/stock-markets-analytics-zoomcamp/blob/main/cohorts/2026/homework4.md).

## What the module is about

A classification model only answers "will this stock grow in the next 30 trading days?".
A trading strategy adds the decisions around it: the probability threshold for a trade, how much to invest,
how many positions to hold, and the cost of every trade. The module turns model predictions into a
simulated trading result on the test period (about 4 years) and compares predictors by **CAGR** and by
**average number of trades per day**, not by precision alone.

Simulation `sim1` used throughout: invest $100 in every positive prediction, sell after 30 trading days,
0.1% fee on buy and on sell. Required capital is approximated as `100 * 30 * Q75(daily trades)`.

## Notebook

[`homework_4.ipynb`](homework_4.ipynb) reproduces the modelling and simulation
steps of the course notebook inside one function, `run_experiment()`, and runs it for each homework setup:

| Experiment | Change vs. course setup | Questions |
|---|---|---|
| `baseline` | none (sanity check: 301 features, best ML predictor `pred5_clf_10` with CAGR 1.1195, as in the lecture) | — |
| `q1_q2` | dummies without `Ticker` and `month_wom` (208 features) + Random Forest rules with thresholds 0.56 … 0.64 | Q1, Q2 |
| `q3` | target = 1 only if `growth_future_30d > 1.022` | Q3 |

Runs in Google Colab or GitHub Codespaces (about 10–15 minutes on a Colab CPU). The dataset is reused from `../03-modeling/` if present, otherwise downloaded from the course Google Drive link (git-ignored).

## Answers

| Question | Answer |
|---|---|
| Q1. CAGR of `pred10_rf_best_rule_55` with fewer dummies | **1.1279** → closest option **1.126** (vs. 1.1108 with 301 features; it becomes the best predictor) |
| Q2. Threshold of the best Random Forest rule with < 5 trades per day | **0.57** (`pred13_rf_best_rule_57`: CAGR 1.0920, 4.85 trades per day) |
| Q3. Number of trades of `pred5_clf_10` with the new target | **8** average trades per day (8.01; 7,337 trades in total, CAGR 1.1218; baseline: 10.3 per day, 9,861 trades, CAGR 1.1195) |
| Q4. Own strategy | section 7: top-3 daily picks with probability ≥ 0.60, fixed 30 daily capital slots, no duplicate tickers, stop-loss / take-profit, parameters chosen on validation, benchmark vs. S&P 500 |

## Observations

- **Fewer features helped the Random Forest** (Q1): dropping 93 sparse ticker and week-of-month dummies raised
  the CAGR of `pred10_rf_best_rule_55` from 1.111 to 1.128.
- **CAGR is not monotonic in the threshold** (Q2): from 0.55 to 0.65 the number of trades falls from 3,756 to 92,
  precision moves only between 0.60 and 0.67, and CAGR falls from 1.128 to 1.009. Higher precision does not
  compensate for the lost volume.
- **A stricter target makes the tree more selective, not more precise** (Q3): with "growth > 2.2%" as the target,
  precision of every predictor drops to about 0.47 (it is measured against the new target), but `pred5_clf_10` trades
  25% less often with a slightly higher CAGR. Random Forest rules with thresholds tuned for the old target almost stop trading.

## Notes

- The course notebook scales `X_test` in the Logistic Regression section and later uses the scaled matrix
  for `predict_proba` with tree models in the threshold table (`df_scores`). The simulation is not affected,
  but that table is. This notebook keeps `X_test` unscaled.
- Model depths, Random Forest parameters and thresholds were selected on the test period in the course,
  so the test results are optimistic. The 33 stocks are today's large caps (survivorship bias).
