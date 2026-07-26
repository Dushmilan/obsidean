# Assessing the Fit in Least-Squares Regression

Getting a regression line $\hat{y} = a + bx$ is easy. Knowing whether it's any good — whether the model is appropriate, how precise the predictions are, and how much variance it explains — that's the real work.

**The Intuition:** Residuals are the "mistakes" your line makes. If the line fits well, the residuals should look like random noise — no pattern, no trend, no fanning. If you see a curve or a funnel, the linear model is wrong and you need a different approach.

**The Math:** A residual is $e_i = y_i - \hat{y}_i$. The standard deviation of residuals is $s = \sqrt{\frac{\sum(y_i - \hat{y}_i)^2}{n-2}}$ — your typical prediction error in the original units of $y$. The coefficient of determination is $R^2 = 1 - \frac{SS_{\text{res}}}{SS_{\text{tot}}}$, where $SS_{\text{tot}} = \sum(y_i - \bar{y})^2$ and $SS_{\text{res}} = \sum(y_i - \hat{y}_i)^2$.

**What does this mean for Statistics?** $R^2$ tells you what *proportion* of the variability in $y$ is explained by the linear relationship with $x$. An $R^2 = 0.84$ means 84% of the variation is accounted for. The remaining 16% is unexplained scatter. The $n - 2$ degrees of freedom in $s$ reflects that you estimated two parameters ($a$ and $b$) from the data.

| Metric | What it measures | Range |
|--------|------------------|-------|
| $r$ | Direction and strength of linear association | $-1$ to $+1$ |
| $R^2$ | Proportion of variance in $y$ explained by $x$ | $0$ to $1$ (or 0% to 100%) |
| $s$ | Typical prediction error (same units as $y$) | $0$ to $\infty$ |

---

**Setup:** A residual plot shows a U-shaped pattern. What now?

**Solution:** The linear model is inappropriate. The curved pattern indicates a non-linear relationship — consider a quadratic model or transformation.

**Key insight:** Random scatter in the residual plot means the linear model is appropriate. Any visible pattern means it isn't, no matter how high $R^2$ is.

---

**Setup:** $s = 25.3$ for a model predicting house prices (in thousands of dollars). Interpret.

**Solution:** When using the regression line to predict house prices, predictions will typically be off by about $\$25{,}300$.

**Key insight:** $s$ is in the same units as $y$ — it's the most intuitive measure of prediction accuracy. $R^2$ is relative; $s$ is absolute.

---

**Outliers vs leverage vs influence:**
- **Outlier in $y$:** large residual, pulls the line up/down, decreases $R^2$
- **High leverage point:** extreme $x$-value, acts as a lever on the slope
- **Influential point:** removing it changes the model significantly (slope, intercept, or $R^2$)

Always check residual plots and look for points that stand apart from the rest.

---
