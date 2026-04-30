# Gamma GLM: Model Selection and Diagnostics

A demonstration of Gamma regression as a GLM for right-skewed, positive-valued response data. The focus is on the full modelling workflow: distribution choice, model hierarchy, selection via likelihood ratio test, and residual diagnostics.

## Why Gamma instead of Gaussian?

When the response is strictly positive and right-skewed, a Gaussian assumption is typically inadequate — it allows negative predictions and assumes constant variance. The Gamma distribution is a natural alternative: it has positive support and its variance scales with the square of the mean, which better reflects how spread grows with the response level.

The model uses a log link:

$$Y_i \sim \text{Gamma}(\alpha, \lambda_i), \quad \log\!\left(\frac{\alpha}{\lambda_i}\right) = x_i^\top \beta$$

so coefficients are interpretable on a multiplicative scale after exponentiation.

## Model selection

Three nested models are compared via likelihood ratio test:

| Model | Predictors |
|-------|------------|
| `model1` | intercept only |
| `model2` | group indicator (AG) |
| `model3` | group indicator + continuous covariate (WBC) |

The LRT selects `model2` — the continuous predictor does not significantly improve fit.

## Diagnostics

Standard Pearson/deviance residuals are inadequate for GLMs with non-normal responses. The analysis uses **randomised quantile residuals** (Dunn & Smyth, 1996), which are exactly standard normal under a correctly specified model, making QQ plots and residual-vs-fitted plots directly interpretable. Cook's distance flags influential observations.

## Dataset

Toy dataset from Christensen (2015), *Analysis of Variance, Design and Regression* (2nd ed.), Ch. 22. 33 observations, two predictors, positive continuous response. Used here purely as illustration.

## Reproduce

Requires R with `statmod`, `ggplot2`, and `patchwork`, and [Quarto](https://quarto.org).

```bash
quarto render gamma_glm_demo.qmd
```
