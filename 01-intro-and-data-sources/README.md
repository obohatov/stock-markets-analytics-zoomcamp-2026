# Module 1: Introduction and Data Sources

## Theory (brief)

- **Data-driven investing**: hypotheses tested on data; risk/reward trade-off.
- **Two data types**: macro (FRED) vs market (Yahoo Finance / `yfinance`).
- **OHLCV** and the special role of **Adjusted Close** (adjusted for dividends and splits).
- **Macro indicators**: GDP (`GDPC1`), rates `DGS2`/`DGS10`, inverted yield curve as a recession signal.
- **Returns**: period growth, CAGR, dividend yield.
- **Setup**: Colab + `yfinance` + `pandas_datareader`; API selection principles (coverage, frequency, rate limits, free vs paid).

## Homework — 7 questions

Numeric answers (submitted to the leaderboard):

1. **[Macro] Average GDP growth in 2023.** `GDPC1` from FRED -> YoY growth (shift by 4 quarters) -> average of the 4 quarters of 2023, rounded to 1 decimal.
2. **[Macro] Inverse yield curve.** `DGS10 - DGS2` since 2000-01-01 -> minimum value of the spread, rounded to 1 decimal.
3. **[Index] Which index is better.** S&P 500 (`^GSPC`) vs IPC Mexico (`^MXX`), 5-year growth -> larger value in % (nearest integer).
4. **[Stocks OHLCV] 52-week range ratio (2023).** Top-6 stocks -> `(max - min) / max` of Adj Close over 2023 -> largest, rounded to 2 decimals.
5. **[Stocks] Dividend yield.** Same 6 companies -> sum of 2023 dividends / Adj Close on the last trading day -> largest %, rounded to 1 decimal.

Free text:

6. **[Exploratory] New metrics.** Find and describe additional metrics / time series useful for the project.
7. **[Exploratory] Earnings-driven strategy.** Describe an idea for selecting companies based on upcoming earnings data.

> Note: date ranges / years may differ slightly in the 2026 cohort version —
> cross-check against the `cohorts/2026` folder in the course repo once published.

## Progress

- [x] Q1 — average GDP growth 2023
- [ ] Q2 — inverse yield curve
- [ ] Q3 — index comparison
- [ ] Q4 — 52-week range ratio
- [ ] Q5 — dividend yield
- [ ] Q6 — new metrics (text)
- [ ] Q7 — earnings strategy (text)

## Files

- [`homework_1.ipynb`](homework_1.ipynb) — working notebook with solutions
