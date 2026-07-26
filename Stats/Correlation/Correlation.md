# Correlation

Scatterplots show relationships, but we need a single number to summarise how strong and in what direction that relationship is. That number is the correlation coefficient $r$.

**The Intuition:** Imagine plotting study hours vs exam scores. If every student who studies more scores higher, the points cluster along an upward line — strong positive correlation. If they scatter randomly, $r$ is near zero. If higher study hours somehow predict lower scores, $r$ is negative.

**The Math:** The correlation coefficient is $r = \frac{1}{n-1}\sum z_{x_i} z_{y_i}$, where $z_{x_i} = \frac{x_i - \bar{x}}{s_x}$ and $z_{y_i} = \frac{y_i - \bar{y}}{s_y}$ are standardised scores. Equivalently: $r = \frac{\sum(x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum(x_i - \bar{x})^2 \sum(y_i - \bar{y})^2}}$.

**What does this mean for Statistics?** $r$ ranges from $-1$ to $+1$. Values near $\pm 1$ indicate strong linear relationships; values near $0$ indicate weak or no linear relationship. Correlation is the foundation for regression — you can't build a reliable prediction line without first knowing whether the relationship is strong enough to model.

| $r$ value | Interpretation |
|-----------|----------------|
| $+1$ | Perfect positive linear |
| $+0.7$ to $+1$ | Strong positive |
| $0$ to $+0.3$ | Weak positive |
| $0$ | No linear relationship |
| $0$ to $-0.3$ | Weak negative |
| $-0.7$ to $-1$ | Strong negative |
| $-1$ | Perfect negative linear |

---

**Key insight:** $r$ only measures *linear* relationships. A perfect parabolic relationship can have $r = 0$. Always look at the scatterplot — the number alone can mislead.

---
