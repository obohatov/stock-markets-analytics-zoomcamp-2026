# Module 5 — Deployment and Automation

Homework for Module 5 of the [Stock Markets Analytics Zoomcamp 2026](https://github.com/DataTalksClub/stock-markets-analytics-zoomcamp) (DataTalks.Club).
Task: [homework5.md](https://github.com/DataTalksClub/stock-markets-analytics-zoomcamp/blob/main/cohorts/2026/homework5.md).

## What the homework is about

The module moves a trading model from a notebook into a daily pipeline. The homework asks a different question: what is worth investigating at all? It needs no code, model or backtest. The answer is three research ideas that differ in their economic mechanism, data and horizon.

| # | Idea | Mechanism | Main data | Horizon |
|---|---|---|---|---|
| 1 | AI-scare overreaction in information services | Narrative-driven overreaction | Hand-labelled AI events and prices | 30–90 trading days |
| 2 | Ukrainian agri stocks in Warsaw | Local operating data precedes results | Ukrainian price and export data | 1–3 months before a report |
| 3 | EU procurement awards and mid-cap contractors | Small contract wins add up before they reach reported results | TED award notices | 1–2 quarters |

None of these ideas is claimed to be profitable. No hypothesis below has been tested yet.

---

## Question 1: AI-scare overreaction in information services

**Market and scope.** Listed legal, data and information-services companies in Europe and the US, starting with RELX, Wolters Kluwer, Thomson Reuters and FactSet. The horizon is 30–90 trading days after an AI-related sell-off.

**Investment thesis.** A new AI product can push these stocks down sharply within a day, before anyone can know how much revenue is really at risk. On 3 February 2026, RELX and Wolters Kluwer fell more than 10% and Thomson Reuters and FactSet fell about 10% after Anthropic released a legal plugin for Claude Cowork. I expect some of these moves to reverse relative to the sector when the product does not replace a paid workflow, and to persist when it does.

**Potential edge.** The market prices the headline first. My work is in AI evaluation, so I can judge what a product actually does: whether it depends on proprietary data, whether it replaces a paid task, and whether it is shipped or only announced. The edge is in sorting the events, not in forecasting the sector.

**Data and sources.**

- Checked: [the February 2026 sell-off figures](https://fintool.com/news/anthropic-ai-software-stock-selloff), reported by a secondary source. [Outsell](https://outsellinc.com/?p=34383) says all three of RELX, Thomson Reuters and Wolters Kluwer grew faster in H1 2026 while their shares fell. That is an industry analyst's view, and the details are behind a login.
- To verify: daily prices and sector benchmarks, plus a complete list of AI-related events with exact dates and times.
- To build: the event table, including quarterly revenue and analyst estimate changes after each event.

**Research approach.** An event study. I would score each event on the three questions above before looking at the returns that followed, so that hindsight does not shape the labels. The test compares sector-adjusted returns of "substitution" and "announcement" events. The decision it informs is whether to buy after the fall or stay out.

**Validation and risks.** The benchmark is a sector ETF or an equal-weight basket of peers. I would reject the idea if "announcement" events do not outperform "substitution" events after costs, or if estimates keep falling after the event. The main risks are the small number of events, overlap between them, and the chance that the falls are a correct repricing of the business model. Events are also clustered in 2026, so the sample may reflect one market regime.

---

## Question 2: Ukrainian agri stocks in Warsaw

**Market and scope.** Ukrainian agricultural holdings listed on the Warsaw Stock Exchange, starting with Astarta and Kernel. The horizon is 1–3 months before a financial report.

**Investment thesis.** The main drivers of these companies' results are public before the report: commodity prices, logistics costs and export volumes. Astarta fell 19.14% on 20 October 2026, and trading was suspended. It had reported an H1 net loss of €14.07 million against a €42.27 million profit a year earlier. The article links the loss mainly to lower sugar and milk prices and higher soybean logistics costs. I expect a simple cost and price indicator to anticipate the direction of such surprises at least some of the time.

**Potential edge.** The useful information is often in Ukrainian-language trade sources and is spread across many places. Coverage of these stocks is likely thin (still to be measured). I spent more than 20 years in Ukrainian business journalism, including editing a fuel and energy journal, so I know where this kind of data is published and how reliable it tends to be.

**Data and sources.**

- Checked: [the Astarta report and price fall](https://kyivindependent.com/shares-in-one-of-ukraines-biggest-farm-companies-fall-to-a-two-year-low/), and [Kernel's latest quote](https://stockanalysis.com/quote/wse/KER/) showing trading on 18 September 2026 at 19.56 PLN, with 15,482 shares traded that day.
- To verify: Ukrainian sugar and milk price series, soybean logistics costs, export statistics, historical prices for both stocks, reporting dates, and Kernel's current listing status. [Kernel applied to delist](https://interfax.com/newsroom/top-stories/95980/) in the past, so its status matters.
- To measure: average daily turnover, bid-ask spread and analyst coverage.

**Research approach.** I would build one company-level indicator from a few public series, weighted by each company's revenue mix. I would compare it with the following report and with the price reaction. It is a transparent rule, not a machine learning model, because there are too few reports to train one. The decision it informs is whether to hold or avoid a stock before a report. Short selling is unlikely to be practical on such thin stocks.

**Validation and risks.** The benchmarks are the WIG index and simply holding the stock through every report. I would reject the idea if the indicator does not beat those benchmarks after spreads, or if a trade of realistic size cannot be filled. War news can dominate prices, and Kernel's shares [rose 26% in one session in February 2025 on reports of possible peace talks](https://en.usm.media/kernel-shares-rise-26-amid-reports-of-a-possible-end-to-the-war/), so I would mark such periods separately. Other risks are few reports, publication lags in Ukrainian statistics, and companies leaving the exchange.

---

## Question 3: EU procurement awards and mid-cap contractors

**Market and scope.** European mid-cap companies with large public-sector revenue. First candidates are CAF, Stadler Rail, Strabag, Sopra Steria and Indra, though their public-sector exposure still has to be checked. The horizon is 1–2 quarters.

**Investment thesis.** Very large contracts are usually announced by the company and priced quickly. Smaller awards are public but may not be announced one by one. Added up, they could show a change in demand before it reaches reported orders or revenue. I expect a rise in awarded value relative to company revenue to precede positive revenue surprises.

**Potential edge.** The data is free, but it is hard to use. Winners appear under local subsidiaries and consortia, in many languages. Matching those names to a listed parent takes work, and that work is the possible advantage. I work with multilingual data in French, Dutch and English, and the same skills apply here.

**Data and sources.**

- Checked: [the official TED API](https://docs.ted.europa.eu/api/latest/search.html) offers a search endpoint for published notices without authentication, plus bulk XML downloads, according to its documentation. I have not run it yet. [Article 50 of Directive 2014/24/EU](https://www.legislation.gov.uk/eudr/2014/24/article/50/2018-01-01/data.htm) gives authorities up to 30 days after the contract is concluded to send the award notice.
- To verify: coverage of winner names and contract values, API limits, each company's order backlog and revenue history, and daily prices.

**Research approach.** I would map TED winners to listed parents, then calculate rolling 12-month awarded value divided by trailing revenue for each company. The signal is the year-on-year change. It would rank companies each quarter. Dating each award by its publication date, not its award date, prevents look-ahead. The decision it informs is which contractors to overweight relative to peers.

**Validation and risks.** The benchmark is an equal-weight basket of the same companies, plus a sector index. I would reject the idea if the signal adds nothing to backlog figures that companies already report, or if it does not predict revenue surprises. Risks include framework agreements whose stated value is only a ceiling, consortium shares that are not disclosed, one-off large contracts, and awards below the publication threshold. Margins are a separate risk: more contracts do not automatically mean more profit.
