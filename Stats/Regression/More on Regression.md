# More on Regression (Inference)

## Definition

Sample line $\hat{y} = a + bx$ estimates the true population line $y = \alpha + \beta x + \varepsilon$. Inference on the slope:

- **SE of slope:** $SE_b = \frac{s}{\sqrt{\sum(x_i-\bar{x})^2}}$
- **CI for $\beta$:** $b \pm t^*\times SE_b$, $df = n-2$
- **Test $H_0{:}\ \beta = 0$:** $t = b/SE_b$

**LINE conditions:** **L**inearity, **I**ndependence, **N**ormality of residuals, **E**qual variance.

## The Intuition

The sample slope has uncertainty, just like a sample mean. Quantify it with $SE_b$; wrap a confidence interval around it. If the CI excludes 0, the linear relationship is significant.

## The Toolkit

| Concept | Formula |
|---------|---------|
| Residual SD | $s = \sqrt{\frac{\sum(y-\hat{y})^2}{n-2}}$ |
| SE of slope | $SE_b = \frac{s}{\sqrt{\sum(x-\bar{x})^2}}$ |
| Confidence interval | $b \pm t^*SE_b$ |
| Test statistic | $t = \frac{b}{SE_b}$ |
| SE of $\hat{y}$ | $s\sqrt{\frac1n + \frac{(x^*-\bar{x})^2}{\sum(x-\bar{x})^2}}$ |

## Method

1. Verify the LINE conditions before trusting inference.
2. CI for slope: exclude 0 → significant relationship.
3. Prediction vs confidence intervals: prediction is always wider (includes scatter $s$).

## Worked Examples

**Setup:** 95% CI for slope is (1.2, 3.8).

**Solution:** 95% confident each unit of $x$ increases mean $y$ by 1.2–3.8. Since 0 is excluded, reject $H_0: \beta = 0$.

**Key insight:** The CI's exclusion of 0 is the significance test.

---

**Setup:** Why is a prediction interval wider than a confidence interval?

**Solution:** Confidence covers the *line's* location; prediction adds the scatter $s$ of individual points around it.

**Key insight:** $\hat{y}$ is most precise at $x^* = \bar{x}$ — the SE formula's minimum.

## Common Traps

- Interpreting a CI as a prediction interval (and vice versa)
- Inference invalid when LINE conditions fail
- Extrapolating beyond the data
- Using $z^*$ instead of $t^*$ for small samples

## Connections

- [[Linear Regression]] · [[Least Squares regression]] · [[Common Distributions]] — the $t$ distribution
- [[Central Limit Theorem]] — large-$n$ justification
- [[Maths/Pure/04-Calculus/02-Differentiation/04.2-Differentiation]] — the minimisation
