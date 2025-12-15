Market Risk Measurement & Backtesting Framework (VaR & ES)

Objective:
Demonstrate hands-on risk analytics and model validation capability by building, comparing, and validating 99% one-day Value-at-Risk (VaR) and Expected Shortfall (ES) models for an equity portfolio. The framework mirrors how Market Risk and Risk Analytics teams estimate tail risk, assess model adequacy, and support daily risk reporting in line with Basel market risk principles.

Process:
Constructed an equal-weighted equity portfolio (AAPL, MSFT, PG, JPM, C) representing a diversified trading-book exposure.

Computed daily portfolio P&L returns using adjusted closing prices.

Implemented three industry-standard VaR and ES methodologies:
Historical Simulation (non-parametric)
Gaussian / Parametric (variance–covariance)
Monte Carlo Simulation (multivariate normal with rolling covariance)

Generated rolling 250-day risk forecasts to replicate daily risk monitoring and limit reporting.

Conducted formal model validation using regulatory-accepted VaR backtests.

Compared model conservatism and tail sensitivity through a VaR risk dashboard.

Results (99% Confidence Level):


VaR & ES Estimates

| Model                 | VaR (99%) | ES (99%) | Tail Severity | Interpretation                                              |
| --------------------- | --------- | -------- | ------------- | ----------------------------------------------------------- |
| Historical            | ≈ −2.3%   | ≈ −2.7%  | Medium        | Empirical tail losses based on realized market history      |
| Parametric (Gaussian) | ≈ −2.1%   | ≈ −2.4%  | Lower         | Volatility-driven estimate under normality assumptions      |
| Monte Carlo           | ≈ −2.6%   | ≈ −3.0%  | Highest       | Correlation-aware simulation capturing joint extreme losses |

Comparative Interpretation:

Monte Carlo VaR and ES are the most conservative, with VaR ≈ −2.6% and ES ≈ −3.0%, implying an additional ~40 bps tail loss beyond VaR due to correlation and joint extreme scenarios.

Historical VaR vs ES shows a tail premium, with VaR ≈ −2.3% and ES ≈ −2.7%, representing ~35–45 bps incremental loss severity after a VaR breach.

Parametric VaR and ES are least conservative, with VaR ≈ −2.1% and ES ≈ −2.4%, giving a narrower ~30 bps VaR–ES gap, reflecting normality assumptions and potential underestimation of fat-tailed risk.


Backtesting Results (99% VaR):



| Model          | Test                        | Test Statistic | p-value | Decision        | Interpretation                                           |
| -------------- | --------------------------- | -------------- | ------- | --------------- | -------------------------------------------------------- |
| Historical VaR | Kupiec POF                  | —              | 0.162   | Pass            | Exception frequency aligned with 1% theoretical level    |
| Parametric VaR | Kupiec POF                  | —              | 0.059   | Borderline Pass | Mild tail underestimation but not statistically rejected |
| Parametric VaR | Christoffersen Independence | 2.42           | ≈ 0.12  | Pass            | No evidence of exception clustering                      |
| Parametric VaR | Conditional Coverage        | 5.98           | ≈ 0.05  | Borderline Pass | Joint frequency and independence within tolerance        |

Backtesting Interpretation:
Exception Frequency: Kupiec p-values indicate observed breaches are statistically consistent with the 99% confidence level.
Independence: Christoffersen Independence test shows no clustering of exceptions, confirming adequate responsiveness to volatility.
Coverage: Conditional Coverage places Gaussian VaR at the regulatory boundary, acceptable but requiring monitoring during stressed conditions.

Model Validity Assessment:
Statistical Adequacy: VaR framework meets Basel-style backtesting requirements.
Dynamic Risk Capture: Independence results confirm absence of systematic VaR breaches.
Regulatory Standing: Gaussian VaR is borderline under conditional coverage; overall framework remains within acceptable thresholds.

Final Verdict
This 99% VaR and ES framework is model-valid and fit for daily market risk measurement. Historical VaR is robust, and Monte Carlo VaR/ES are preferred for conservative tail-risk assessment.

Tech Stack:Python,NumPy,Pandas,SciPy,YFinance,Matplotlib.

Future Enhancements
ES backtesting and validation.
EWMA/GARCH for Parametric VaR.
Stress testing and scenario analysis.
FRTB-style liquidity horizons and capital aggregation.
Automated daily risk reporting and limit monitoring.

Key Takeaways:
Developed and validated a 99% one-day VaR and ES framework consistent with daily market risk analytics and internal model validation practices.
Monte Carlo VaR/ES are the most conservative, capturing correlation-driven joint tail risk; Historical VaR/ES provide robust empirical benchmarks.
Parametric (Gaussian) VaR is statistically acceptable but operates close to regulatory tolerance, warranting closer monitoring in stressed markets.
Formal Kupiec and Christoffersen backtests confirm the framework is model-valid under normal market conditions.
