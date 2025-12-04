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
Historical ES is the most conservative due to real fat-tail behavior.
Parametric ES is least conservative, as the normal distribution underestimates extreme losses.
Monte Carlo VaR/ES lies between the two, giving a balanced tail estimate.

Crisis Stress Test Results
| Scenario                 | VaR 99%     | ES 99%      |
| ------------------------ | ----------- | ----------- |
| **Market Crisis Regime** | **−14.28%** | **−15.72%** |



Crisis Impact:
Tail losses worsen by 5–6× relative to normal market conditions.
ES rises from ~3% to over 15%, indicating extreme tail-risk concentration.
This confirms high systemic correlation + volatility spikes = severe downside risk.

Technical Stack:Python,Yfinance, NumPy, Pandas, SciPy, Matplotlib, Jupyter Notebook.

Future Enhancements:
Stressed covariance VaR
GARCH(1,1) volatility-based VaR
T-distribution parametric VaR
Expected Shortfall regression (ESReg)
EVT (Extreme Value Theory) Peaks-Over-Threshold model
