# Module 2: Dataframe Analysis

Homework: https://github.com/DataTalksClub/stock-markets-analytics-zoomcamp/blob/main/cohorts/2026/homework2.md

## Q1 — [IPO] Withdrawn IPOs by company type

**Answer: Acquisition Corp — $499.98M**

32 withdrawn filings on IPOScoop. Classification rules are ordered, so the first match
wins: `Coolbit Technologies Ltd.` is `Technologies` rather than `Limited`, and
`pan-Africa Corp.` is `Acquisition Corp` because the rule matches the bare substring
`Corp`. Six rows have zero shares, and all six also lack a price range, so the fallback
to `Est $ Vol` applies cleanly and no zero values leak into the totals.

| Company Type | Total, $M | Count |
| --- | ---: | ---: |
| Acquisition Corp | 499.98 | 5 |
| Inc. | 351.00 | 1 |
| Holdings | 311.66 | 4 |
| Other | 290.44 | 8 |
| Limited | 203.85 | 8 |
| Technologies | 184.90 | 3 |
| Group | 32.50 | 3 |

## Q2 — [IPO] Median Sharpe ratio for 2025 IPOs (first 8 months)

**Answer: 0.05**

146 IPOs priced before 1 September 2025 with a non-zero return; 133 tickers still
resolve on Yahoo Finance, 132 present in the 2026-09-11 snapshot, 130 with a full
252-day history.

Median `growth_252d` is 0.595 against a mean of 1.058 — the typical 2025 IPO lost about
40% of its value over the year, and the mean is pulled above 1 by a few extreme winners
(max 33.6x).

Two caveats on the statistics:

- The task defines volatility as `Close.rolling(30).std() * sqrt(252)`, i.e. the standard
  deviation of *prices*, not returns. Its scale depends on the price level of the stock,
  so cheap stocks get a mechanically higher Sharpe.
- A couple of barely-traded tickers have a rolling std of exactly 0, producing `inf`
  Sharpe values. The median is unaffected; the mean is not usable.

## Q3 — [IPO] Fixed months holding strategy

**Answer: 1 month — median growth 0.9354**

| Horizon | Median | Mean |
| --- | ---: | ---: |
| 1 month | 0.9354 | 95.75 |
| 2 months | 0.8930 | 67.98 |
| 3 months | 0.8272 | 51.25 |
| 4 months | 0.7304 | 22.70 |
| 5 months | 0.6909 | 54.24 |
| 6 months | 0.7260 | 64.01 |
| 7 months | 0.6620 | 59.27 |
| 8 months | 0.6035 | 21.46 |
| 9 months | 0.5803 | 20.01 |
| 10 months | 0.5333 | 11.26 |
| 11 months | 0.4818 | 8.90 |
| 12 months | 0.4918 | 5.82 |

The median declines almost monotonically with the holding period and stays below 1
throughout: buying at the first-day close and holding loses money for the typical IPO,
and the longer the hold the worse it gets. The shortest horizon is "optimal" only in the
sense of losing the least.

The gap between median and mean is the real lesson here — a mean of 95.75 against a
median of 0.94 at the 1-month horizon. The means are distorted by at least one ticker
whose IPO-day close was around a cent (reverse split or bad Yahoo data), producing
growth values in the thousands.

## Q4 — [Strategy] Simple RSI-based trading strategy

**Answer: $65.81 thousand**

5,206 trades between 2000-01-01 and 2025-06-01 where RSI dropped below 30, $1,000
invested per signal, 30-day forward return. Average 30-day return 1.26%, win rate 55.13%
— matching the expected observations in the task.

## Q5 — [Exploratory] Predicting a positive-return IPO

Buying at the first-day close and holding is a losing default, as Q3 shows. Changes worth
testing:

1. Skip the first-day close. Enter after the quiet period expires or at lock-up
   expiration (~180 days), when hype pricing has faded.
2. Filter on deal quality: tier-1 underwriters and offering size above ~$50M. Micro-cap
   deals placed by small brokers produce most of the left tail — the same segment that
   dominates the withdrawal list in Q1.
3. Require momentum: buy only IPOs still trading above their offer price after a month.
4. Replace the fixed holding period with stop-loss / take-profit rules. The return
   distribution is strongly right-skewed, so cutting losses reshapes it more than
   choosing between a 6- and a 9-month horizon.
5. Rank candidates by risk-adjusted return rather than raw growth.

All of this needs out-of-sample validation: with ~130 tickers over a single year it is
easy to find a filter that fits history and fails afterwards.

## Progress

- [x] Q1 — withdrawn IPO value by company class (Acquisition Corp, $499.98M)
- [x] Q2 — median Sharpe as of 2026-09-11 (0.05)
- [x] Q3 — optimal holding period (1 month, 0.9354)
- [x] Q4 — net income from RSI < 30 trades ($65.81K)
- [x] Q5 — strategy ideas

## Notes on reproducibility

IPOScoop tables are live, so the numbers drift. The task expects 148 tickers after the
Q2 filter; this run got 146, because `Return` is computed from the current price and a
couple of stocks have since landed on exactly 0.00% and were filtered out.

`data.parquet` (Q4, 130 MB) is gitignored — re-download it by running the notebook.

## Files

- [`homework_2.ipynb`](homework_2.ipynb) — working notebook with all solutions

## Submission

- Form: https://courses.datatalks.club/sma-zoomcamp-2026/homework/hw02
- Leaderboard: https://courses.datatalks.club/sma-zoomcamp-2026/leaderboard
