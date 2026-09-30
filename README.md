# Options Pricing and Earnings Risk Model

Does the options market correctly price earnings reactions? This project compares options implied earnings moves with realized, market-adjusted stock reactions for 10 AI, semiconductor, and power companies, then uses Monte Carlo simulation to test whether the difference is statistically meaningful.

Built in Python with the help of Claude Code.

## Sample

The most recent completed earnings release, as of September 2026, for AMD, AMAT, AMZN, CEG, GEV, META, MSFT, MU, NVDA, and VST.

## Method

1. **Event study.** Each stock's earnings reaction is measured from the last close before the release to the first close after it, net of the S&P 500 (SPY) move that day.
2. **Implied vs. realized.** Each reaction is compared with the move priced by the at-the-money straddle before the release.
3. **Monte Carlo significance test.** 200,000 earnings seasons are simulated under the assumption that options priced every event correctly, using both normal and fat-tailed (Student's t) jump distributions with antithetic variates.
4. **Straddle pricing model.** A jump-diffusion Monte Carlo pricer values earnings straddles and is validated against Black-Scholes.

## Results

| Ticker | Implied move | Realized move vs. market | Realized / implied |
|---|---|---|---|
| AMZN | 7.2% | +14.6% | 2.03 |
| MSFT | 7.2% | +13.8% | 1.92 |
| MU | 11.0% | +15.6% | 1.41 |
| NVDA | 6.1% | +8.1% | 1.33 |
| META | 8.4% | -9.6% | 1.15 |
| GEV | 8.6% | -8.6% | 1.00 |
| AMD | 9.9% | -6.8% | 0.69 |
| AMAT | 9.6% | -4.9% | 0.51 |
| VST | 7.9% | -1.2% | 0.15 |
| CEG | Not available | -1.4% | Not available |

**Key findings**

* Realized moves averaged 1.13x the implied move, and five of nine stocks moved more than priced.
* Under fair pricing, a result this large occurs in about 29% of simulated seasons, so the gap is not statistically significant with nine events.
* MSFT and AMZN each moved roughly twice their implied move, while VST, AMAT, and AMD moved well less than priced.

## Data sources

* **Implied moves:** Barchart's weekly *Option Volatility and Earnings Report* (at-the-money straddle for the nearest expiration). Micron's figure comes from Saxo's straddle-based preview. No published implied move was found for CEG.
* **Prices:** Yahoo Finance daily closes.

## How to run

```
pip install numpy pandas scipy matplotlib yfinance
jupyter notebook earnings_implied_vs_realized.ipynb
```

Sections 1 through 4 run as is. Set `RUN_LIVE = True` in Section 5 to pull 12 quarters of earnings history per stock with `yfinance` and run a beta-adjusted market model event study.

## Limitations

One event per stock, implied moves from published sources rather than historical option chains, and a straddle payoff proxy that ignores bid-ask spreads. Section 5 extends the sample to 12 quarters per stock.
