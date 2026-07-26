# Pearson Correlation Coefficient

The Pearson $r$ is the standard measure of linear association between two quantitative variables. It's the formula behind the correlation values reported by software.

**The Intuition:** Think of it as averaging how consistently two variables move together, after standardising each to the same scale. If both variables tend to be above their means together (and below together), $r$ is positive. If one is above while the other is below, $r$ is negative.

**The Math:**

$$r = \frac{\sum_{i=1}^{n} (x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum_{i=1}^{n} (x_i - \bar{x})^2 \sum_{i=1}^{n} (y_i - \bar{y})^2}}$$

The numerator is the sum of cross-products of deviations — it captures how $x$ and $y$ co-vary. The denominator standardises by the total variation in each variable. This forces $r$ into $[-1, +1]$.

**What does this mean for Statistics?** $r^2$ (the coefficient of determination) gives the proportion of variance in $y$ explained by $x$ through the linear relationship. An $r = 0.8$ means $r^2 = 0.64$ — about 64% of the variability in $y$ is accounted for by $x$. The sign of $r$ tells direction; $r^2$ tells strength of explanation.

---

**Key properties:**
- $r$ is unitless — it doesn't change if you rescale $x$ or $y$
- $r$ is symmetric — correlation of $x$ with $y$ equals correlation of $y$ with $x$
- $r = 0$ means no *linear* association, not no association (could be curved)
- Outliers can dramatically inflate or deflate $r$

---
