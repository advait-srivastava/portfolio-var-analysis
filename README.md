# Portfolio VaR Analysis — Historical Simulation vs. Parametric (Variance-Covariance)

Value at Risk analysis of an equal-weighted, five-stock Indian equity portfolio, computed two independent ways — Historical Simulation and the Parametric (Variance-Covariance) method — over ~244 daily returns (13 Jun 2025 – 11 Jun 2026). Prepared for the WILP Finance Lab, Risk Management theme, BITS Pilani.

- **`Historical_and_Parametric_VaR.xlsx`** — the full model: reconstructed daily closing prices, log returns, per-stock risk statistics, the covariance/correlation matrix, and both VaR calculations, each on its own sheet.
- **`VaR_Report.docx`** — the write-up: methodology, results, and interpretation.

## Portfolio

Equal-weighted, ₹100,000 in each of five large-cap Indian names (₹500,000 total): **TCS, Wipro, Bharti Airtel, Adani Enterprises, Godrej Industries** — spanning IT services, telecom, infrastructure, and diversified industry. Daily closing prices were reconstructed from S&P Capital IQ's cumulative percentage data (`Price = Base Price × (1 + CIQ% / 100)`) via the WILP Finance Lab; daily log returns then computed as `ln(Pₜ / Pₜ₋₁)`.

## Methodology

**Historical Simulation** — sorts the actual portfolio return history from worst to best and reads off the loss at the target confidence level, with no distributional assumption.

**Parametric (Variance-Covariance)** — assumes normally distributed returns; portfolio volatility is derived from the weight vector and the full covariance matrix (`σ_p = √(wᵀΣw)`), then scaled by a z-score (1.645 at 95%, 2.326 at 99%).

Both are reported at 1-day and 10-day horizons, with the 10-day figure scaled from the 1-day figure by the square-root-of-time rule (`×√10`).

## Individual stock risk

| Stock | Annualized Volatility |
|---|---|
| Godrej Industries | 38.0% |
| Adani Enterprises | 34.7% |
| Wipro | 25.5% |
| TCS | 24.8% |
| Bharti Airtel | 19.7% |

Correlations are mostly low (0.05–0.30) — the exception is TCS/Wipro at **0.62**, unsurprising since both are large IT services firms. Low cross-correlation is what gives the equal-weighted portfolio its diversification benefit: portfolio volatility (1.11% daily / 17.65% annualized) comes in well below what a simple average of the five individual volatilities would suggest.

## Results

| Method | 95% 1-day | 99% 1-day | 95% 10-day | 99% 10-day |
|---|---|---|---|---|
| Historical Simulation | ₹11,684 (2.34%) | ₹14,648 (2.93%) | ₹36,948 | ₹46,320 |
| Parametric (Var-Cov) | ₹9,142 (1.83%) | ₹12,927 (2.59%) | ₹28,911 | ₹40,880 |
| **Difference (HS − Parametric)** | **₹2,541** | **₹1,720** | **₹8,037** | **₹5,440** |

## Interpretation

Historical Simulation produces a **larger** VaR than the Parametric method at every horizon and confidence level. That gap is the actual finding: it indicates the portfolio's real return distribution has fatter tails than the normal distribution the Parametric method assumes (leptokurtosis) — large down-days occurred more often historically than a bell curve would predict, so the Parametric method understates worst-case risk for this portfolio. Historical Simulation is the more conservative, realistic estimate here; Parametric remains useful as a fast, closed-form approximation.

**Suggested extensions** (from the report's conclusion): backtest the VaR estimates against realized breaches, add an Expected Shortfall (CVaR) measure to capture average loss beyond the VaR threshold, and reconsider the weighting of the two highest-volatility names (Godrej Industries, Adani Enterprises) if lower portfolio risk is desired.

## Data source

Daily prices sourced via S&P Capital IQ through the BITS WILP Finance Lab. Only derived closing prices, log returns, and summary statistics are included here — no raw Capital IQ terminal exports.
