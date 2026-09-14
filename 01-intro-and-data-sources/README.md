# Module 1: Introduction and Data Sources

Homework for the 2026 cohort — downloading financial data from various sources
(Wikipedia, Yahoo Finance) and performing simple calculations and analysis.

Assignment brief:
https://github.com/DataTalksClub/stock-markets-analytics-zoomcamp/blob/main/cohorts/2026/homework1.md

## Theory recap

- **Data-driven investing**: hypotheses tested on data; risk/reward trade-off.
- **Two data types**: macro (FRED) vs market (Yahoo Finance / `yfinance`).
- **OHLCV** and the role of **Adjusted Close** (adjusted for dividends and splits).
- **Returns**: period growth, CAGR, drawdowns, dividend yield.
- **Data acquisition**: scraping tables with `pandas.read_html`, bulk price
  downloads with `yfinance`, handling differing market calendars and stale facts.

## Results

| # | Question | Answer |
|---|----------|--------|
| Q1 | [Index] Year with most S&P 500 additions (since 2020) | **2025** (18 additions) |
| Q1 (add.) | Current constituents in the index > 20 years | **224** |
| Q2 | [Macro] World indexes beating S&P 500 YTD (1 Jan–21 Aug 2026) | **2** (Nikkei 225, TSX Composite) |
| Q3 | [Index] Median S&P 500 correction drawdown (>=5%, since 1950) | **~8.0%** (7.99) |
| Q4 | [Stocks] AMZN median 2-day return after positive earnings surprise / correlation | **+0.35% / 0.331** |
| Q5 | [Exploratory] Capstone idea | see below |
| Q6 | [Exploratory] New metrics | see below |

### Notes on the numbers

- **Q1**: additions counted per year from the current Wikipedia constituent list;
  2025 leads comfortably (next is 2024 with 16), so the result is stable.
- **Q3**: the computed top-10 drawdowns match the reference list in the brief
  (2007–09: 56.8% / 517 days, 2000–02: 49.1% / 929 days, ...), validating the
  correction-detection logic.
- **Q4**: the sample was restricted to the brief's window (earnings from
  2020-10-29 onward, ~24 quarters). Including the full history back to 2014 skews
  the median upward via early-year outliers (+3900%, +240% surprises).

## Q5 — Capstone project idea

Extend **VeriHub** (auditing how public LLMs represent organisations) into a domain
with objective, free ground truth: verifiable facts about S&P 500 companies. Ask
several public LLMs factual questions per ticker (latest dividend, P/E, last earnings
date, 52-week high, index membership) in **English, French and Dutch**, and score
answers against Yahoo Finance / FRED / Wikipedia. The ML target is a **binary
classifier predicting when an LLM answer is wrong**, using features such as the
stock's volatility, fact recency, ticker popularity (market cap, volume) and query
language.

## Q6 — New metrics

Each metric doubles as a feature for predicting LLM factual errors:

- **Realized volatility** — how fast a company's facts go stale.
- **Market cap & average volume** — popularity, i.e. how well-represented in training data.
- **Earnings dates & surprise** — event recency (error hotspot right after a release).
- **S&P 500 membership changes** — models lag on recent additions/removals.
- **Per-ticker news-coverage volume** — density of published text about the company.

Retrievable via `yfinance`, `pandas.read_html` on the Wikipedia list, and a news/RSS
API such as GDELT.

## Progress

- [x] Q1 — additions per year (2025) + additional (>20y: 224)
- [x] Q2 — indexes YTD vs S&P 500 (2)
- [x] Q3 — median correction drawdown (8.0%)
- [x] Q4 — AMZN earnings surprise (0.35% / 0.331)
- [x] Q5 — capstone idea
- [x] Q6 — new metrics

## Files

- [`homework_1.ipynb`](homework_1.ipynb) — working notebook with all solutions

## Submission

- Form: https://courses.datatalks.club/sma-zoomcamp-2026/homework/hw01
- Leaderboard: https://courses.datatalks.club/sma-zoomcamp-2026/leaderboard
