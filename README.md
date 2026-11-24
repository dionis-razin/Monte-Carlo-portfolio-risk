# Monte Carlo Portfolio Risk Simulation

1-month portfolio risk simulation (95% VaR and Expected Shortfall) built in Python with correlated asset returns and a correlation stress test.

## What It Does
- Pulls daily closing prices for 5 US stocks (`AAPL`, `MSFT`, `GOOGL`, `JPM`, `XOM`) via `yfinance` (Jan 2024 – Nov 2026) and calculates daily returns.
- Estimates mean returns, volatility, and the empirical correlation matrix.
- Runs 10,000 Monte Carlo simulations over a 21-trading-day horizon using Cholesky decomposition to preserve asset correlations.
- Computes 95% Value at Risk (VaR) and Expected Shortfall (ES) for an equally weighted portfolio (20% per stock).
- Stress-tests the portfolio by shifting correlations toward `1.0` to measure how risk scales when diversification breaks down.
- Plots the correlation heatmap (`seaborn`) and the simulated return distribution with VaR/ES cutoffs (`matplotlib`).

## Results
- **95% VaR (1-month):** `-[XX.XX]%`
- **95% ES (1-month):** `-[XX.XX]%`
- **Correlation stress test:** Pushing average pairwise correlation from `[0.XX]` to `[0.XX]` increased 95% VaR by `+[XX.X]%`.
