# Correlation

## Definition

A single number summarising the strength and direction of a linear relationship:

$$r = \frac{1}{n-1}\sum z_{x_i}z_{y_i} = \frac{\sum(x_i-\bar{x})(y_i-\bar{y})}{\sqrt{\sum(x_i-\bar{x})^2\sum(y_i-\bar{y})^2}}, \qquad r \in [-1, +1]$$

| $r$ | Interpretation |
|-----|----------------|
| $+1$ | perfect positive linear |
| $0.7$–$1$ | strong positive |
| $0$–$0.3$ | weak positive |
| $0$ | no *linear* relationship |
| $-0.7$ to $-1$ | strong negative |
| $-1$ | perfect negative linear |

## The Intuition

Plot study hours vs exam scores: points clustering along an upward line → strong positive $r$; random scatter → near 0; upward hours predicting lower scores → negative.

## The Toolkit

| Quantity | Formula |
|----------|---------|
| Correlation | $r = \frac{\sum(x-\bar{x})(y-\bar{y})}{\sqrt{\sum(x-\bar{x})^2\sum(y-\bar{y})^2}}$ |
| Standardised score | $z_x = \frac{x-\bar{x}}{s_x}$ |
| $r$ in z-scores | $r = \frac{1}{n-1}\sum z_x z_y$ |

## Derivation

The numerator is the sum of cross-products of deviations — how $x$ and $y$ co-vary. The denominator standardises by each variable's total variation, forcing $r$ into $[-1,+1]$. Full derivations: [Pearson Coefficient Coefficient]

## Method

1. Always plot the scatterplot first — $r$ alone can mislead.
2. Compute $r$; interpret direction (sign) and strength (magnitude).
3. Check for outliers — they can inflate or deflate $r$ dramatically.

## Worked Examples

**Setup:** A perfect parabola. What is $r$?

**Solution:** $r = 0$ — the relationship is strong but *not linear*.

**Key insight:** $r$ only measures linear relationships — the number alone can mislead.

## Common Traps

- $r$ measures *linear* association only — curved relationships can have $r = 0$
- Correlation doesn't imply causation
- Outliers dominate $r$ — inspect the plot
- Units don't matter ($r$ is unitless) but scaling does change $r$ for some formulas — use standardised form

## Connections

- [[Pearson Coefficient Coefficient]] · [[Linear Regression]] - the next step
- [[Bias in Sampling]] - study design limits interpretation
- [[Maths/Pure/01-Algebra/04-Polynomials/04-Polynomials]] - squared sums
