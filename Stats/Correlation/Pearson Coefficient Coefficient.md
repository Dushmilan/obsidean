# Pearson Correlation Coefficient

## Definition

The standard measure of linear association between two quantitative variables:

$$r = \frac{\sum_{i=1}^{n}(x_i-\bar{x})(y_i-\bar{y})}{\sqrt{\sum_{i=1}^{n}(x_i-\bar{x})^2 \sum_{i=1}^{n}(y_i-\bar{y})^2}}$$

**Coefficient of determination:** $R^2 = r^2$ — the proportion of variance in $y$ explained by $x$.

## The Intuition

Averaging how consistently two variables move together after standardising each to the same scale. Both above their means together → positive $r$; one above while the other below → negative.

## The Toolkit

| Property | Value |
|----------|-------|
| Range | $[-1, +1]$ |
| Unitless | rescaled $x$ or $y$ doesn't change it |
| Symmetric | $r(x,y) = r(y,x)$ |
| $r = 0$ | no *linear* association (could be curved) |
| Outliers | can dramatically inflate or deflate $r$ |

## Derivation

$r = \frac{1}{n-1}\sum z_xz_y$ — the average product of z-scores. The denominator's normalisation is what bounds $r$ to $[-1,1]$ (Cauchy–Schwarz). [Full derivations: [[Correlation]]]

## Method

1. Verify both variables are quantitative and the relationship looks linear (plot!).
2. Compute $r$ (or use the z-score product form).
3. Interpret: $r^2$ = explained variance; sign = direction.

## Worked Examples

**Setup:** $r = 0.8$. What does $r^2$ tell you?

**Solution:** $r^2 = 0.64$ — about 64% of the variability in $y$ is accounted for by $x$.

**Key insight:** Sign gives direction; $r^2$ gives strength of explanation.

## Common Traps

- $r = 0$ ≠ no association — only no *linear* association
- Outliers can flip the sign or inflate $r$
- $r$ is symmetric — don't imply causation from it
- Compare $r$ values only for comparable contexts

## Connections

- [[Correlation]] · [[Linear Regression]] — $b = r\cdot s_y/s_x$
- [[Maths/Pure/01-Algebra/04-Polynomials/04-Polynomials]] — sums of squares
