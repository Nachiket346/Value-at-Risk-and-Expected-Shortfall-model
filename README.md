An end-to-end risk management system built to measure, simulate, dynamic backtest, and regulatory-evaluate portfolio risk. The pipeline downloads multi-asset historical market data, computes baseline linear returns, evaluates statistical loss distributions across parametric and non-parametric domains, and subjects out-of-sample models to Basel Committee regulatory benchmarks.

Objective:
Quantify potential portfolio tail-risk using Value at Risk  and Expected Shortfall  at the 99% confidence interval across multiple statistical methodologies (Historical, Parametric Normal, Student’s t-distribution, Monte Carlo).
Conduct statistical backtesting using Kupiec’s Likelihood Ratio (Unconditional Coverage), Independence, and Conditional Coverage tests.
Implement a continuous, out-of-sample rolling 250-day window model to eliminate look-ahead bias.
Map continuous dynamic backtest outcomes directly to the regulatory Basel Market Risk Framework (Traffic-Light System).

Methodology:

Data Acquisition & Portfolio Modeling:
Asset universe: Equities across tech, consumer goods, and financial sectors (AAPL, MSFT, PG, JPM, C).
Time series: Daily adjusted close prices spanning Dec 15, 2023 through Aug 12, 2026 ($N = 664$ daily return observations).
Portfolio composition: Equal weighted vector[0.20, 0.20, 0.20, 0.20, 0.20]'

Tail Risk Modeling (alpha = 0.01):
Historical Simulation: Empirical 1st percentile ranking and tail-loss conditional expectation.
Parametric Normal: Closed-form Gaussian formulation mu+z*sigma.
Parametric Student’s t: Scaled non-standardized t-distribution fit to capture heavy-tailed market behavior.
Monte Carlo Simulation (Normal & t): 10,000 vector draws parameterized by empirical variance-covariance and degree-of-freedom parameters.

Statistical Backtesting:
Kupiec Likelihood Ratio (LR UC): Evaluates whether exception frequencies statistically match the theoretical alpha = 0.01.
Christoffersen Independence Test (LR IND): Evaluates exception clustering properties via Markov transition probabilities.
Conditional Coverage (LR CC): Combined test evaluating frequency and clustering (LR_CC=LR_UC+LR_IND).

Dynamic Out-of-Sample Rolling Backtest & Basel Framework:
A rolling 250-day estimation window shifted by t-1 (zero look-ahead bias) computes continuous historical and parametric VaR forecasts over a 414-day evaluation sample.
Tail loss exceptions on the trailing 250-day evaluation period are assigned to Basel Traffic Light zones (Green: 0–4 exceptions; Yellow: 5–9 exceptions; Red: 10+ exceptions).

Quantified Results with Interpretation:
1.In-Sample Static Risk Metrics (Full Dataset N = 664)


| Method | 99% 1-Day VaR | 99% 1-Day Expected Shortfall (ES) |
|---|---|---|
| **Historical Simulation** | -2.57% | -3.78% |
| **Parametric Normal** | -2.30% | -2.65% |
| **Parametric Student's t** | -2.54% | -3.62% |
| **Monte Carlo (Normal)** | -2.34% | -2.65% |
| **Monte Carlo (Student's t)** | -2.54% | -3.62% |

Interpretation:
Historical VaR (-2.57%): Over a 1-day horizon, there is a 1% probability that the equal-weighted portfolio will experience a loss exceeding 2.57%.
Historical ES (-3.78%): When losses breach the 99% VaR threshold, the average expected loss severity is 3.78%.

Parametric Normal Underestimation: The Normal distribution underestimates tail risk ($\text{VaR} = -2.30\%$, $\text{ES} = -2.65\%$) because it fails to capture leptokurtic tail events.
Student's t-Distribution Model Fit: Fitting a Student's t-distribution yields degrees of freedom v=3.87. The low degree-of-freedom parameter confirms heavy tails, matching the historical estimates far more accurately (VaR = -2.54% ES = -3.62%).

Monte Carlo Convergence: 10,000 Monte Carlo draws converge tightly to their theoretical parametric counterparts for both Normal and t-distributions.

2.Statistical Backtesting (Full Dataset)
Sample Size (N): 664 trading days.
Expected Exceptions (N*0.01): 6.64 breaches.
Actual Exceptions (x): 7 breaches.

| Statistical Test | Likelihood Ratio (LR) Statistic | p-value | Decision (alpha_test = 0.05$) |
|---|---|---|---|
| Unconditional Coverage (LR_UC) | 0.0194 | 0.8893 | Accept H_0 (Model is accurate) |
| Independence (LR_IND) | 3.6185 | 0.0571 | Accept H_0(No significant exception clustering) |
| Conditional Coverage (LR_CC) | 3.6379 | 0.1622 | Accept H_0(Model structurally valid) |

Interpretation:

Coverage Calibration (LR_UC):With 7 observed breaches versus 6.64 expected (N=664, alpha=0.01), (LR_UC = 0.0194,p=0.8893). A high pvalue (>0.05) confirms the model's overall risk coverage is statistically precise.

Market Clustering (LR_IND): (LR_IND = 3.6185,p=0.0571). While passing the 5% threshold (p > 0.05), its proximity to the critical value (3.841) reveals mild exception clustering during high-volatility shocks.

Composite Diagnostic (LR_CC): (LR_CC = 3.6379,p = 0.1622). The model passes joint coverage, but 99.5% of total penalty stems from LR_IND(3.6185 / 3.6379), proving model risk is driven by temporal clustering rather than poor coverage sizing.


3.Out-of-Sample Dynamic Rolling Backtest (414-Day Evaluation Window)

| Metric | Value |
|---|---|
| Evaluation Window Length | 414 days |
| Expected Exceptions| 4.14 breaches |
| **Actual Exceptions (Rolling Historical)| 6 breaches |
| Rolling Kupiec LR_UC| 0.7412 |
| p-value| 0.3893 |
| Model Status| Coverage remains valid out-of-sample |

Interpretation:
Zero Look-Ahead Validation: Evaluated across a 414-day rolling window, the model generated 6 actual breaches against an expected 4.14 breaches (alpha = 0.01).

Out-of-Sample Statistical Validity: The dynamic Kupiec test yielded $LR_{UC} = 0.7412$ with a $p\text{-value}$ of $0.3893$. Because p > 0.05, the model passes unconditional coverage under real-time, out-of-sample conditions.

Regime Adaptability: The low breach count confirms that updating the 250-day estimation window step-by-ahead effectively adapts to shifting market volatility without underestimating forward-looking tail risk.


4.Regulatory Framework (Basel Committee Traffic Light Evaluation)Evaluated over the trailing 250 trading days (t_250 to t_0):
| Model | Exceptions (Last 250d) | Regulatory Zone | Capital Multiplier Impact / Implication |
|---|---|---|---|
| Historical VaR (Rolling)| 1 | Green | No penalty applied — model considered accurate |
| Parametric Normal VaR (Rolling)| 2 | Green| No penalty applied — model considered accurate |

Interpretation:
Regulatory Compliance: Both rolling historical (1 breach) and parametric normal (2 breaches) models fall comfortably within the Green Zone (0–4 exceptions per 250 trading days).

Capital Multiplier Impact: The model receives a scaling factor multiplier of 3.00 (the baseline value), incurring zero additional regulatory capital penalties.

Model Soundness: The low exception counts over the trailing 250 days align with the Basel framework guidelines for internal market risk models, placing the portfolio within standard operational thresholds.

Techstack:Python,NumPy,Pandas,SciPy(scipy.stats),yFinance,Matplotlib.

Future Enhancements:
Volatility Filtering: Implement GARCH(1,1) or EGARCH models to capture dynamic time-varying volatility clusters and adjust rolling $\text{VaR}$ continuously.
Multivariate Fat Tails: Incorporate Multivariate Student's t or Copula-based Monte Carlo simulations to preserve non-linear joint asset dependency structures.
Extreme Value Theory (EVT): Model tail losses using the Generalized Pareto Distribution (GPD) via Peak-Over-Threshold (POT) methodologies.
Component / Marginal VaR: Decompose total portfolio risk into incremental asset-level risk contributions for active rebalancing.

Key Takeaways:
Fat Tails Matter: Standard Normal parametric models understate extreme downside risks in equity portfolios. Student's t-distributions (3.87) capture leptokurtic losses and match empirical historical tail behavior.

Zero Look-Ahead Validation: In-sample static risk metrics often obscure dynamic regime shifts. A 250-day dynamic step-ahead rolling model ensures true out-of-sample validity.

Regulatory Compliance: The portfolio risk system aligns with international banking standards (Basel Traffic Light Framework), operating within the Green Zone with the standard base multiplier of 3.00 and zero regulatory penalty add-ons.
