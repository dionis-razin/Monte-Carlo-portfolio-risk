# Quant Finance Projects

This repository contains three practical projects in quantitative finance written in Python (Google Colab). The goal of these projects is to learn how financial risk, statistical trading strategies, and backtesting work in practice.

## Repository Structure

- `01_monte_carlo_risk.ipynb` — Portfolio risk simulation using the Monte Carlo method.
- `02_pairs_trading_ewa_ewc.ipynb` — Pairs trading strategy using two related ETFs (`EWA` and `EWC`).
- `03_moving_average_backtest_spy.ipynb` — Backtest of a 50/200 Moving Average Crossover strategy on `SPY`.

---

## Project 1: Monte Carlo Portfolio Risk Simulation

**Goal:** Estimate how much money a stock portfolio can lose in a bad month and see how correlation between stocks affects total risk.

**How it works:**
1. Download at least 2 years of daily stock prices for 3–5 stocks using `yfinance`.
2. Calculate average returns, volatility (how much prices move up and down), and the correlation matrix (how closely the stocks move together).
3. Run 10,000 random simulations for a 1-month period (21 trading days). We use Cholesky decomposition so the random numbers keep the real correlations between stocks.
4. Calculate two risk metrics:
   - **95% Value at Risk (VaR):** The loss level that we expect to exceed only 5% of the time.
   - **Expected Shortfall (ES):** The average loss in the worst 5% of scenarios.
5. Test what happens when correlations increase (for example, during a market crash).

**Key Takeaway:**
- *(In progress — add your VaR and ES numbers here after running the notebook.)*
- When stocks start moving together more closely, diversification stops working well and portfolio risk grows much faster.

---

## Project 2: Pairs Trading (`EWA` / `EWC`)

**Goal:** Build a trading strategy that looks for two assets that usually move together, waits for their prices to move apart, and bets that they will come back together.

**How it works:**
1. **Find a pair and test the link:** We used two country ETFs — Australia (`EWA`) and Canada (`EWC`) — because both economies depend heavily on natural resources. The Engle-Granger test gave a `p-value = 0.01` (below `0.05`), which confirms a real long-term link between them.
2. **Calculate the spread and hedge ratio:** Using linear regression, we found a hedge ratio of `0.5175`. Because `EWC` moves more strongly than `EWA`, for every $1,000 in `EWA`, we take an opposite position of $517.50 in `EWC`.
3. **Check the return speed (Half-Life):** We measured how fast the price gap (spread) usually closes. The half-life was **17.4 trading days** (about 3.5 weeks), which is in a good working range (5 to 30 days).
4. **Create trading signals (Z-Score):** We measured how far the spread is from its 30-day average:
   - Buy the spread when `Z-Score < -2`.
   - Sell the spread when `Z-Score > +2`.
   - Close the trade when `Z-Score` returns near `0` (between `-0.5` and `+0.5`).
5. **Backtest:** We shifted trading signals by 1 day so we never trade on future prices.

**Results & Conclusions:**
- The strategy made about **+10%** over almost 3 years (around 3% per year), with a peak of +16.5% and noticeable drawdowns in early 2024 and early 2026.
- **Why it is not ready for real trading:** A 3% yearly return is too small. In real life, broker fees (4 trades for each full cycle), slippage, and daily fees for short-selling would easily wipe out the profit. Also, the link between two assets can break temporarily, so a real version needs a stop-loss (for example, closing the trade if `|Z-Score| > 3.5`).

---

## Project 3: Moving Average Crossover Backtest (`SPY`)

**Goal:** Test a classic trend-following strategy and see how realistic testing rules (1-day delay and trading fees) change the results compared to simply buying and holding the market.

**How it works:**
1. **Data & Indicators:** Downloaded daily prices for `SPY` (S&P 500 ETF) from 2018 to 2026. Calculated the 50-day Simple Moving Average (`SMA_50`, short-term trend) and the 200-day Simple Moving Average (`SMA_200`, long-term trend).
2. **Trading Rule & 1-Day Delay:** Buy and hold `SPY` when `SMA_50 > SMA_200`, and stay in cash otherwise. Because the signal only appears after the market closes, we shift the position by 1 day (`shift(1)`) to avoid look-ahead bias (using future data).
3. **Transaction Costs:** Subtracted **0.1%** on every buy and sell to include broker fees and price slippage.
4. **Benchmark Comparison:** Compared the strategy against passive **Buy & Hold** using cumulative return, annual return (CAGR), maximum drawdown (biggest drop from a peak), and Sharpe ratio (return per unit of risk).

**Results (2018–2026):**

| Metric | SMA 50/200 (After Fees) | Buy & Hold (`SPY`) |
| :--- | :---: | :---: |
| **Cumulative Return** | 84.09% | 187.48% |
| **Annualized Return (CAGR)** | 7.95% | 14.16% |
| **Max Drawdown** | -33.72% | -33.72% |
| **Sharpe Ratio** | 0.57 | 0.78 |

**Key Conclusions:**
- **Underperformed Buy & Hold:** The strategy made less than half the profit of simply holding `SPY`, had a lower Sharpe ratio, and did not reduce the worst drawdown.
- **Different market conditions:** The strategy worked well during the slow, steady drop in **2022** (it exited to cash in time). However, it failed during fast crashes and quick recoveries (like **spring 2020** and **spring 2025**) because moving averages react too slowly — it sold near the bottom and missed much of the rebound.
- **Impact of fees:** Because `SMA 50/200` makes very few trades over 8 years, the 0.1% fee had only a small effect. Faster averages (like `5/20`) would trade much more often and lose much more to fees.

---

## How to Run

1. Open any `.ipynb` file in **Google Colab** or **Jupyter Notebook**.
2. Install the required libraries if needed:
   ```bash
   pip install yfinance pandas numpy matplotlib statsmodels
