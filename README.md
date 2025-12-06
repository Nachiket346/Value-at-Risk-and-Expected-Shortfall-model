Objective:
A complete Market Risk Measurement Framework that computes and analyzes Value-at-Risk (VaR) and Expected Shortfall (ES) using Historical, Parametric, and Monte Carlo methods. The project assesses portfolio vulnerability under severe market-crisis stress scenarios.

Process
1. Return & Portfolio Construction
Loaded daily asset returns and formed a weighted portfolio.
Computed mean return vector and covariance matrix for risk estimation.

2. VaR & ES Estimation
Implemented three industry-standard risk models:
Historical VaR/ES (non-parametric, raw empirical distribution)
Parametric Gaussian VaR/ES (μ, σ, and z-scores)
Monte Carlo Simulation (1000 multivariate scenarios)

3. Crisis Stress Testing
A realistic market-crisis regime was simulated with:
Mean shock: −7% collapse in expected daily returns
Volatility multiplier: 2.5× increase
Correlation shift: +0.35 to reflect systemic contagion
A new covariance matrix was generated and stress-scenario VaR/ES was computed.

Results

| Method                    | VaR 99%    | ES 99%     |
| ------------------------- | ---------- | ---------- |
| **Historical**            | **−2.41%** | **−4.51%** |
| **Parametric (Gaussian)** | **−2.47%** | **−2.84%** |
| **Monte Carlo**           | **−2.66%** | **−2.96%** |

Interpretation:
VAR:
Monte Carlo VaR (−2.66%) is the most conservative, slightly higher in magnitude than historical and Gaussian, because simulations generate many extreme paths—even if they’re not fully fat-tailed.

Historical VaR (−2.41%) is based on real observed losses. It reflects actual market history but may miss hypothetical extreme shocks not present in the dataset.

Parametric VaR (−2.47%) assumes returns are normally distributed.it often underestimates tail losses, but in this  case it is close to historical because the sample volatility likely matches the Gaussian assumption reasonably well.

Expected Shortfall:
Historical ES (−4.51%) is the most conservative because it uses actual return distributions, which naturally contain fat tails, volatility clustering, and crisis-day losses. since it does not assume normality, it fully captures extreme downside moves observed in real markets.

Parametric ES assumes returns follow a normal distribution, so tail losses are smooth and symmetric. Because real markets have fat tails and negative skew, Parametric ES typically underestimates extreme losses. That’s why it is much less severe than historical ES.

Monte Carlo ES generates thousands of simulated return paths using modelled volatility and correlation, allowing more extreme scenarios than the Gaussian approach. however, unless fat-tailed distributions are used, it still underestimates real crisis-level losses, giving an ES of −2.96%—more conservative than parametric but far below the historical ES.

Crisis Stress Test Results
| Scenario                 | VaR 99%     | ES 99%      |
| ------------------------ | ----------- | ----------- |
| **Market Crisis Regime** | **−14.28%** | **−15.72%** |

Intepretation:
Crisis VaR 99% widened to −14.28%, roughly 5–6× higher than normal-market VaR (−2.41% to −2.66%), indicating a sharp jump in potential worst-day losses under a market-crisis regime.
The surge in crisis VaR reflects volatility spikes and correlation breakdowns, showing how systemic stress can multiply downside risk far beyond calm-market estimates.

Crisis ES 99% deteriorated to −15.72%, about 4–6× more severe than normal-market ES (−2.84% to −4.51%), highlighting the extreme clustering of losses deep in the tail.
ES increases more than VaR during crisis conditions because it averages all tail events, capturing compounding fat-tail shocks that VaR misses and revealing the full depth of crisis-driven risk.

Technical Stack:Python,Yfinance, NumPy, Pandas, SciPy, Matplotlib, Jupyter Notebook.

Future Enhancements:
Stressed covariance VaR
GARCH(1,1) volatility-based VaR
T-distribution parametric VaR
Expected Shortfall regression (ESReg)
EVT (Extreme Value Theory) Peaks-Over-Threshold model
