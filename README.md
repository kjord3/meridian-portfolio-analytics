Meridian Capital Partners — Portfolio Analytics Platform

Live dashboard: kjord3.github.io/meridian-portfolio-analytics

An automated portfolio analytics pipeline built for a fictitious boutique wealth management firm, Meridian Capital Partners — replacing a manual, spreadsheet-based reporting process with a live, self-refreshing dashboard.


The problem

Meridian's analysts were tracking a 20-holding equity portfolio in Excel: pulling prices by hand, recalculating risk metrics in scattered formulas, and rebuilding charts every time a client asked for an update. It didn't scale, it was error-prone, and it told them nothing about risk beyond simple returns.

The approach

Three pieces, each doing one job well:

StageToolWhat it doesIngest & analyzePython (Google Colab)Pulls live prices and macro data via API, computes risk-adjusted performance metricsStoreGitHubHolds the processed CSVs the dashboard reads fromPresentHTML / JS (Chart.js)Fetches the CSVs and renders an interactive dashboard — no server required

Data flows one direction, start to finish: API → Python → CSV → GitHub → Browser. Re-running the notebook and pushing the new CSVs is the only manual step; everything downstream updates automatically.

What it does

Data sources


yfinance — daily prices for 20 holdings + S&P 500 benchmark
FRED API — 10-year Treasury yield, CPI, unemployment rate, Fed funds rate


Analytics layer


Annualized return, volatility, Sharpe ratio, Sortino ratio, max drawdown — per holding and portfolio-wide
CAPM regression (via statsmodels) for beta and alpha on every holding against the benchmark
Correlation matrix across all 20 holdings, for a diversification check
Monte Carlo simulation (10,000 runs) for 1-day 95% Value-at-Risk and Conditional VaR
30-day rolling volatility for a risk trend view


Dashboard


Overview — portfolio value, YTD return, Sharpe ratio, max drawdown, cumulative return chart, sector allocation
Risk Analysis — rolling volatility, correlation heatmap, per-holding risk metrics table
Allocation — weight by sector and by holding
Macro Context — portfolio performance overlaid against interest rates, inflation, and unemployment


The dashboard fetches its data directly from this repo's meridian_processed_data/ folder on load — open the link and it's already populated, no setup required.

Tech stack

Python · pandas · numpy · statsmodels · yfinance · fredapi · HTML/CSS/JS · Chart.js · PapaParse · Google Colab · GitHub Pages

Repo structure

├── index.html                    # the dashboard (GitHub Pages entry point)
├── meridian_portfolio_pipeline.ipynb   # data ingestion + analytics notebook (Colab)
├── meridian_processed_data/      # exported CSVs the dashboard reads
│   ├── dim_holdings.csv
│   ├── dim_dates.csv
│   ├── fact_prices.csv
│   ├── fact_returns.csv
│   ├── fact_risk_metrics.csv
│   ├── fact_macro.csv
│   ├── fact_portfolio_summary.csv
│   ├── fact_correlation.csv
│   └── portfolio_summary_metrics.csv
└── README.md

Running it yourself


Open meridian_portfolio_pipeline.ipynb in Google Colab.
Add a free FRED API key in the config cell.
Run all cells — this pulls fresh data and exports the 9 CSVs to data/processed/.
Replace the CSVs in meridian_processed_data/ with the new exports and push.
The dashboard picks up the update automatically on next page load — no code changes needed.


Roadmap


 Schedule the notebook to run automatically via GitHub Actions (daily, after market close), so the data refreshes with zero manual steps
 Add a benchmark comparison line (portfolio vs. S&P 500) to the cumulative return chart
 Expand to a multi-asset-class portfolio (bonds, commodities) to test the pipeline beyond equities


Skills demonstrated

Statistical modeling (CAPM, Monte Carlo simulation), API-based data engineering, risk-adjusted performance analysis, and building a lightweight client-side data application from a fully custom data model — no BI tool or backend required.


Meridian Capital Partners is a fictitious client used for demonstration purposes. Market and macroeconomic data are real, sourced from Yahoo Finance and the Federal Reserve Economic Data (FRED) API.

Built with Claude.
