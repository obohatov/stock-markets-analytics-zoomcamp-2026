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
| `baseline` | none (sanity check: 301 features, CAGR ≈ 1.1195 for `pred10_rf_best_rule_55`) | — |
| `q1_q2` | dummies without `Ticker` and `month_wom` (208 features) + Random Forest rules with thresholds 0.56 … 0.64 | Q1, Q2 |
| `q3` | target = 1 only if `growth_future_30d > 1.022` | Q3 |

Runs in Google Colab or GitHub Codespaces (about 10–15 minutes on a Colab CPU). The dataset is reused from `../03-modeling/` if present, otherwise downloaded from the course Google Drive link (git-ignored).

## Answers

| Question | Answer |
|---|---|
| Q1. CAGR of `pred10_rf_best_rule_55` with fewer dummies | _see section 3 of the notebook_ |
| Q2. Threshold of the best Random Forest rule with < 5 trades per day | _see section 4_ |
| Q3. Number of trades of `pred5_clf_10` with the new target | _see section 5_ |
| Q4. Own strategy | section 7: top-3 daily picks with probability ≥ 0.60, fixed 30 daily capital slots, no duplicate tickers, stop-loss / take-profit, parameters chosen on validation, benchmark vs. S&P 500 |

## Notes

- The course notebook scales `X_test` in the Logistic Regression section and later uses the scaled matrix
  for `predict_proba` with tree models in the threshold table (`df_scores`). The simulation is not affected,
  but that table is. This notebook keeps `X_test` unscaled.
- Model depths, Random Forest parameters and thresholds were selected on the test period in the course,
  so the test results are optimistic. The 33 stocks are today's large caps (survivorship bias).
