# Linear Regression

Correlation tells you *whether* two variables are related. Regression tells you *how* — it fits a line through the data so you can predict one variable from the other.

**The Intuition:** Imagine drawing a straight line through a cloud of points. You want the line to go through the "middle" of the cloud, as close as possible to all the points simultaneously. That's exactly what least-squares regression does — it minimises the sum of squared vertical distances (residuals) between the points and the line.

**The Math:** The least-squares line is $\hat{y} = a + bx$, where $b = r \cdot \frac{s_y}{s_x}$ is the slope and $a = \bar{y} - b\bar{x}$ is the intercept. The slope $b$ tells you how much $y$ changes for each unit increase in $x$. The intercept $a$ is the predicted $y$ when $x = 0$.

**What does this mean for Statistics?** Regression is the workhorse of prediction and causal inference. Once you have the line, you can predict $y$ for any $x$, test whether the slope is significantly different from zero, and quantify how much variability the model explains.

---

**Key insight:** The line always passes through $(\bar{x}, \bar{y})$ — the point of means. This is a useful sanity check: if someone gives you a regression line that doesn't go through the means, something's wrong.

**Residual:** $e_i = y_i - \hat{y}_i$. The sum of residuals is always zero in least-squares regression — positive and negative residuals cancel out by construction.

---
