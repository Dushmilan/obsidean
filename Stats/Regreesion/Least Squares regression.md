---
date: 2026-07-21
type: stats-concept
source: manual
status: reviewed
tags: [stats, regression, least-squares]
---

# Lesson 5: Assessing the Fit in Least-Squares Regression

When fitting a least-squares regression line $\hat{y} = a + bx$, simply getting an equation isn't enough. We must evaluate **how well** the linear model fits the data and whether a linear model is even appropriate in the first place.

---

## 1. Residuals Refresher

A **residual** measures the vertical distance between an actual observed data point and the point predicted by the regression line.

$$\text{Residual } (e_i) = y_i - \hat{y}_i = \text{Actual } y - \text{Predicted } y$$

- **$e_i > 0$ (Positive):** The actual point is *above* the line (the line underestimated $y$).
- **$e_i < 0$ (Negative):** The actual point is *below* the line (the line overestimated $y$).
- **$\sum e_i = 0$:** The sum (and mean) of all residuals in least-squares regression is always **zero**.

---

## 2. Residual Plots (Assessing Linearity)

A **residual plot** is a scatter plot with the explanatory variable ($x$) or predicted values ($\hat{y}$) on the horizontal axis and the residuals ($e$) on the vertical axis.

**Key Rule:** If a linear model is appropriate, the residual plot should show a **random scatter** of points centered around $e = 0$, with no discernible shape or pattern.

| Residual Plot Visual | Meaning | Decision |
| :--- | :--- | :--- |
| **Random Scatter** | Equal variance, no clear trend | Linear model is **appropriate** |
| **Curved Pattern** | The relationship is non-linear | Linear model is **INAPPROPRIATE** |
| **Fan/Funnel Shape** | Non-constant variance | Predictions become less reliable as $x$ increases |

---

## 3. Standard Deviation of Residuals ($s$)

While $R^2$ gives a relative measure (percentage), $s$ gives an **absolute measure of prediction error** in the original units of the response variable $y$.

**Formula:**
$$s = \sqrt{\frac{\sum (y_i - \hat{y}_i)^2}{n - 2}} = \sqrt{\frac{SS_{\text{res}}}{n - 2}}$$

**Why $n - 2$ Degrees of Freedom?**
We divide by $n - 2$ instead of $n - 1$ because we have estimated **two parameters** from the sample data: the slope ($b$) and the intercept ($a$).

**Required AP Exam Interpretation Template for $s$:**
*"When using the least-squares regression line to predict response variable $y$ from explanatory variable $x$, our predictions will typically be off by about $s$ value with units."*

---

## 4. Coefficient of Determination ($R^2$)

$R^2$ measures the **proportion of total variability** in the response variable ($y$) that is explained by the linear relationship with $x$.

**Derivation & Formula:**

1. **Total Sum of Squares ($SS_{\text{tot}}$):** Error when predicting $y$ using only the mean $\bar{y}$.
   $$SS_{\text{tot}} = \sum (y_i - \bar{y})^2$$

2. **Residual Sum of Squares ($SS_{\text{res}}$):** Error remaining after fitting the regression line.
   $$SS_{\text{res}} = \sum (y_i - \hat{y}_i)^2$$

$$R^2 = 1 - \frac{SS_{\text{res}}}{SS_{\text{tot}}} = \frac{SS_{\text{tot}} - SS_{\text{res}}}{SS_{\text{tot}}}$$

**Required AP Exam Interpretation Template for $R^2$:**
*"About $R^2 \times 100\%$ of the variability in response variable $y$ is accounted for by the linear relationship with explanatory variable $x$."*

---

## 5. Outliers, High Leverage, & Influential Points

Not all extreme points affect the regression line in the same way:

| Type | Definition | Effect |
| :--- | :--- | :--- |
| **Outlier in Y** | Extreme vertical distance from the line | Large residual; pulls line up/down; decreases $R^2$ |
| **High Leverage Point** | Extreme horizontal value ($X$) | Acts like a lever on slope; strong effect on fit line |
| **Influential Point** | Removal drastically changes the model | Can change slope, intercept, or $R^2$ significantly |

---

## 6. Metric Comparison Summary

| Metric | Symbol | What it measures | Range / Units |
| :--- | :--- | :--- | :--- |
| **Correlation** | $r$ | Direction and strength of linear relationship | $-1 \le r \le 1$ (Unitless) |
| **Coeff. of Determination** | $R^2$ | % of variance in $y$ explained by $x$ | $0 \le R^2 \le 1$ ($0\% - 100\%$) |
| **Std Dev of Residuals** | $s$ | Average size of prediction error | Same units as $y$ |

---

## 7. Practice Questions

**Question 1:** A residual plot shows a clear U-shaped pattern. What does this indicate about the linear model?

<details>
<summary>Click for Answer</summary>

The linear model is **inappropriate**. The U-shaped pattern indicates a non-linear relationship between the variables. Consider using a quadratic or other non-linear transformation.
</details>

**Question 2:** The standard deviation of residuals for a model predicting house prices (in thousands of dollars) is $s = 25.3$. Interpret this value.

<details>
<summary>Click for Answer</summary>

When using the least-squares regression line to predict house prices from the explanatory variable, our predictions will typically be off by about **$25,300**.
</details>

**Question 3:** A regression model has $R^2 = 0.84$. Interpret this value.

<details>
<summary>Click for Answer</summary>

About **84%** of the variability in the response variable is accounted for by the linear relationship with the explanatory variable.
</details>

**Question 4:** What is the difference between an outlier and a high leverage point?

<details>
<summary>Click for Answer</summary>

An **outlier** has an extreme value in the $y$-direction (large residual), while a **high leverage point** has an extreme value in the $x$-direction. A high leverage point can significantly influence the regression line, but an outlier may not if it's near the center of the $x$-values. An **influential point** is one whose removal significantly changes the regression model.
</details>

---

## 8. Key Formulas Quick Reference

| Concept | Formula |
| :--- | :--- |
| Residual | $e_i = y_i - \hat{y}_i$ |
| Std Dev of Residuals | $s = \sqrt{\frac{\sum (y_i - \hat{y}_i)^2}{n - 2}}$ |
| Coefficient of Determination | $R^2 = 1 - \frac{SS_{\text{res}}}{SS_{\text{tot}}}$ |
| Total Sum of Squares | $SS_{\text{tot}} = \sum (y_i - \bar{y})^2$ |
| Residual Sum of Squares | $SS_{\text{res}} = \sum (y_i - \hat{y}_i)^2$ |

---

## See Also

- Correlation Coefficient $r$
- Least-Squares Regression Equations
- Transforming Non-Linear Data
- Confidence Intervals for Slope
- Hypothesis Testing for Regression

---

*Last Updated: 2026-07-21*