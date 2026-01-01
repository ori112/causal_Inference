# Causal Inference: Impact of Job Training on Earnings
### *A Robust Analysis of the Lalonde Dataset using Microsoft DoWhy*

## 📌 Project Overview
This project applies **causal inference** to determine the true impact of the National Supported Work (NSW) job training program on future earnings. Using the classic **Lalonde dataset**, I moved beyond simple correlation to isolate the "Causal Effect" by identifying and blocking confounders using the **Backdoor Criterion**.

## 🛠️ Technical Stack
* **Language:** Python 3.12 (Environment managed via `uv`).
* **Framework:** Microsoft `DoWhy`.
* **Libraries:** `Pandas`, `Matplotlib`, `Seaborn`, `Scipy.stats`.

---
## 🧪 The Causal Pipeline
followed the structured 4-step causal inference pipeline:

1.  **Modeling:** Built a **Directed Acyclic Graph (DAG)** to explicitly map causal assumptions, defining relationships between Treatment (`treat`), Outcome (`re78`), and Confounders.

2.  **Identification:** Used the **Backdoor Criterion** and the **Adjustment Formula** to find the mathematical "recipe" (estimand) for the causal effect.
3.  **Estimation:** Compared three distinct estimators—OLS, Stratification, and Weighting—to check for methodological convergence.
4.  **Refutation:** Stress-tested the findings using **Placebo Treatments** and **Random Common Causes** to detect "ghost" patterns or hidden biases.

---

## 📊 Key Findings

### 1. Covariate Imbalance (The Bias)
The **Standardized Mean Difference (SMD)** analysis proved that participants were not chosen at random:
* **Black (1.6708)**: Significant demographic imbalance between groups.
* **Married (-0.7208)**: Treated individuals were significantly less likely to be married.
* **re74 (-0.5968)**: The treated group started with much lower earnings, creating a negative baseline bias.

### 2. Comparative Estimates
| Estimator | Estimated Impact (ATE) | Status |
| :--- | :--- | :--- |
| **OLS Regression** | **$1,577.85** | **Selected** |
| **Stratification** | **$880.00** | **Volatile** |
| **Weighting** | **$415.70** | **Rejected** |

### 3. Validation
| Test Type | Regression Result | Weighting Result | Interpretation |
| :--- | :--- | :--- | :--- |
| **Placebo Treatment** | **-$70.99** | **$2,156.81** | **Regression Passed**: The effect correctly dropped to near zero. **Weighting Failed** by finding a massive "ghost" effect. |
| **Subset Refutation** | **$1,585.94** | **$433.35** | **Regression Passed**: The result remained stable after dropping 10% of the data. |
| **Random Cause** | **$1,577.70** | **$415.70** | Both remained stable, but Weighting's instability in other tests makes it unreliable. |