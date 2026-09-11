# Assessing the Fit in Least-Squares Regression

## Definition

Whether a fitted line is any good:

- **Residual:** $e_i = y_i - \hat{y}_i$ — the line's "mistake" at $x_i$.
- **Residual SD:** $s = \sqrt{\frac{\sum(y_i-\hat{y}_i)^2}{n-2}}$ — typical prediction error in the units of $y$.
- **Coefficient of determination:** $R^2 = 1 - \frac{SS_{res}}{SS_{tot}}$ — proportion of variance explained.

| Metric | Measures | Range |
|--------|----------|-------|
| $r$ | direction + strength of linear association | $-1$ to $+1$ |
| $R^2$ | proportion of variance in $y$ explained | 0–100% |
| $s$ | typical prediction error | 0–∞ |

## The Intuition

Residuals are the line's mistakes. Good fit → residuals look like random noise (no pattern, no fan). A curve or funnel in the residual plot means the linear model is wrong.

## The Toolkit

| Quantity | Formula |
|----------|---------|
| Residual | $e_i = y_i - \hat{y}_i$ |
| Residual SD | $s = \sqrt{\frac{\sum(y_i-\hat{y}_i)^2}{n-2}}$ |
| $R^2$ | $1 - \frac{SS_{res}}{SS_{tot}}$ |

## Derivation

$R^2 = r^2$ for simple regression — the squared correlation equals the explained-variance proportion. The $n-2$ degrees of freedom reflect the two estimated parameters ($a, b$). [Full derivations: [[Linear Regression]]]

## Method

1. **Plot the residuals** — random scatter = appropriate model; pattern = wrong model.
2. Interpret $R^2$ (relative) and $s$ (absolute, same units as $y$).
3. Look for outliers / leverage / influential points.

## Worked Examples

**Setup:** A residual plot shows a U-shape.

**Solution:** The linear model is inappropriate — consider a quadratic or transformation.

**Key insight:** Any visible pattern in residuals means the model is wrong, regardless of $R^2$.

---

**Setup:** $s = 25.3$ predicting house prices (thousands of $).

**Solution:** Predictions are typically off by about \$25,300.

**Key insight:** $R^2$ is relative; $s$ is absolute — the most intuitive accuracy measure.

## Common Traps

- High $R^2$ with patterned residuals — still a bad model
- Outlier in $y$ (big residual) vs high leverage (extreme $x$) vs influential (changes the model)
- Degrees of freedom $n-2$, not $n$
- Residuals must be checked by plotting, not just summarised

## Connections

- [[Linear Regression]] — the fit · [[More on Regression]] — inference
- [[Bias in Sampling]] — data quality limits fit
- [[Maths/Pure/04-Calculus/08-Partial-Differentiation/04.8-Partial-Differentiation]]
