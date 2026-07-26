# More on Regression

Fitting a line is just the start. Now we need to know whether the relationship is real, how confident we are in the slope, and what the predictions actually mean.

**The Intuition:** Your sample regression line $\hat{y} = a + bx$ is an estimate of the true population line $y = \alpha + \beta x + \varepsilon$. The error term $\varepsilon$ is normally distributed with mean 0. Just like a sample mean has uncertainty, so does the sample slope $b$ — and we can quantify it.

**The Math:** The standard error of the slope is $SE_b = \frac{s}{\sqrt{\sum(x_i - \bar{x})^2}}$, where $s$ is the residual standard deviation. A confidence interval for $\beta$ is $b \pm t^* \times SE_b$ with $df = n - 2$. The test statistic for $H_0: \beta = 0$ is $t = b / SE_b$.

**What does this mean for Statistics?** Before trusting a regression, verify the LINE conditions: **L**inearity (residual plot is random), **I**ndependence (random sampling), **N**ormality (residuals are roughly normal), **E**qual variance (no fan shape in residual plot). If these fail, the inference is unreliable.

| Concept | Formula | What it measures |
|---------|---------|------------------|
| Std Dev of Residuals | $s = \sqrt{\frac{\sum(y_i - \hat{y}_i)^2}{n-2}}$ | Typical prediction error |
| Std Error of Slope | $SE_b = \frac{s}{\sqrt{\sum(x_i - \bar{x})^2}}$ | Variability of $b$ across samples |
| Confidence Interval | $b \pm t^* \times SE_b$ | Plausible range for true $\beta$ |
| Test Statistic | $t = b / SE_b$ | How far $b$ is from 0 in SE units |

---

**Setup:** A 95% CI for slope is $(1.2, 3.8)$. Interpret.

**Solution:** We are 95% confident that for each additional unit of $x$, the true mean $y$ increases by between 1.2 and 3.8 units.

**Key insight:** If the CI doesn't contain 0, you reject $H_0: \beta = 0$ — there's a significant linear relationship. If it does contain 0, you can't conclude the slope is different from zero.

---

**Confidence vs Prediction intervals:** A confidence interval for the mean response at $x^*$ is $\hat{y} \pm t^* \times SE_{\hat{y}}$. A prediction interval for an individual response is $\hat{y} \pm t^* \times \sqrt{SE_{\hat{y}}^2 + s^2}$. Prediction intervals are always wider — they account for both the uncertainty in the line AND the scatter of individual points around it.

$SE_{\hat{y}} = s\sqrt{\frac{1}{n} + \frac{(x^* - \bar{x})^2}{\sum(x_i - \bar{x})^2}}$ is smallest when $x^* = \bar{x}$ — predictions are most precise near the centre of your data.

---
