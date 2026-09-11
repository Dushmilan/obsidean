# Linear Regression

## Definition

Fitting a line to predict one variable from another:

$$\hat{y} = a + bx, \qquad b = r\cdot\frac{s_y}{s_x}, \qquad a = \bar{y} - b\bar{x}$$

$b$ = slope (change in $y$ per unit $x$); $a$ = intercept (predicted $y$ at $x = 0$). The line always passes through $(\bar{x}, \bar{y})$.

## The Intuition

Draw a straight line through a cloud of points — as close to all of them as possible. Least squares minimises the sum of squared vertical distances (residuals) between the points and the line.

## The Toolkit

| Quantity | Formula |
|----------|---------|
| Slope | $b = r\frac{s_y}{s_x}$ |
| Intercept | $a = \bar{y} - b\bar{x}$ |
| Residual | $e_i = y_i - \hat{y}_i$ |
| Residual sum | $\sum e_i = 0$ (by construction) |

## Derivation

Minimise $\sum(y_i - a - bx_i)^2$ by setting the partial derivatives to zero — solving the two normal equations gives $b = r s_y/s_x$ and $a = \bar{y} - b\bar{x}$. [Full derivations: [[Maths/Pure/04-Calculus/08-Partial-Differentiation/04.8-Partial-Differentiation]]]

## Method

1. Check the scatterplot is roughly linear.
2. Compute $\bar{x}, \bar{y}, s_x, s_y, r$.
3. $b = r s_y/s_x$; $a = \bar{y} - b\bar{x}$.
4. Sanity check: the line passes through the means.

## Worked Examples

**Setup:** Given $\bar{x} = 50$, $\bar{y} = 80$, $s_x = 10$, $s_y = 15$, $r = 0.8$, find the line.

**Solution:** $b = 0.8\times\frac{15}{10} = 1.2$; $a = 80 - 1.2\times50 = 20$. So $\hat{y} = 20 + 1.2x$.

**Key insight:** Each unit of $x$ predicts $+1.2$ units of $y$.

## Common Traps

- Extrapolating beyond the data range
- Interpreting $a$ when $x = 0$ is meaningless for the data
- Using regression on curved relationships
- Residuals always sum to zero — check it as a sanity test

## Connections

- [[Least Squares regression]] — assessing fit · [[More on Regression]] — inference
- [[Correlation]] — where $r$ comes from
- [[Maths/Pure/04-Calculus/08-Partial-Differentiation/04.8-Partial-Differentiation]] — the minimisation
