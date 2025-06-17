# 🏆 Serie A Bayesian Football Match Modeling & Prediction

This project applies Bayesian statistical modeling to analyze and predict outcomes of football matches from the 2000 Serie A season using the `footBayes` package. It focuses on estimating team-specific offensive and defensive abilities, performing posterior predictive checks, forecasting results, and reconstructing league rankings.

## 📌 Project Objectives

- Estimate home advantage, team attack, and defense strengths
- Compare Bayesian and MLE-based model outcomes
- Validate models with posterior predictive checks
- Predict match outcomes and simulate end-of-season rankings
- Evaluate models using LOOIC for performance comparison

## 🧰 Tools & Libraries

- R packages: `footBayes`, `rstan`, `bayesplot`, `loo`, `ggplot2`, `dplyr`, `tidyverse`

## ⚽ Dataset

Subset of the `italy` dataset from `footBayes`, focusing on the 2000 Serie A season (`italy_2000`).

## 📈 Models Trained

| Model Type      | Description                        | Dynamic   | Priors                 |
|-----------------|------------------------------------|-----------|-------------------------|
| `biv_pois`      | Bivariate Poisson                  | No / Yes  | Default, Student-t, Laplace |
| `double_pois`   | Double Poisson                     | Yes       | Student-t, Cauchy       |

- All Bayesian models use 4 chains with 200 iterations
- Both static and dynamic models trained
- Prior variations tested to compare effects

## 📊 Visualizations & Outputs

- **Posterior Distributions**: `home`, `rho`, `sigma_att`, `sigma_def`
- **Team Abilities**: Attack and defense strength comparison (Bayesian vs MLE)
- **Posterior Predictive Checks**:
  - Goal difference distributions
  - Match-level predictions
- **Forecasts & Rankings**:
  - Match outcome predictions (e.g., Reggina vs AC Milan)
  - Team-specific and aggregate league rank probabilities

## 🧪 Model Evaluation

- Leave-One-Out Cross-Validation (LOOIC) used to compare model performance:
  ```r
  log_lik <- extract_log_lik(fit)
  loo_result <- loo(log_lik)
