---
date: 2026-07-22
type: stats-concept
source: manual
status: reviewed
tags: [stats, regression, inference]
---

# Lesson 6: More on Regression

Building on our understanding of least-squares regression, this lesson dives deeper into **inference for regression**, **confidence intervals**, **hypothesis testing**, and **prediction intervals**. We'll explore how to make statistical conclusions about the true relationship between variables.

---

## 1. The Population Regression Model

Recall that the regression line we calculate from sample data is just an estimate. There is a **true population regression line**:

$$y = \alpha + \beta x + \varepsilon$$

Where:
- **$\alpha$ (alpha)** = True population y-intercept
- **$\beta$ (beta)** = True population slope
- **$\varepsilon$ (epsilon)** = Random error term (normally distributed with mean 0)

Our sample gives us estimates:
- **$a$** estimates $\alpha$
- **$b$** estimates $\beta$

---

## 2. Conditions for Inference in Regression

Before performing inference, we must verify these conditions:

| Condition | Description | How to Check |
| :--- | :--- | :--- |
| **Linearity** | The relationship between $x$ and $y$ is linear | Residual plot shows random scatter |
| **Independence** | Observations are independent | Random sampling/experimental design |
| **Normality** | Residuals are normally distributed | Histogram or Normal probability plot of residuals |
| **Equal Variance** | Constant variance of residuals | Residual plot shows no fan/funnel shape |

**Note:** These are often referred to as the **LINE** conditions (Linearity, Independence, Normality, Equal Variance).

---

## 3. Standard Error of the Slope ($SE_b$)

Just like we have a standard error for a sample mean, we have a standard error for the sample slope $b$.

$$SE_b = \frac{s}{\sqrt{\sum (x_i - \bar{x})^2}}$$

Where:
- $s$ = Standard deviation of residuals
- $\sum (x_i - \bar{x})^2$ = Sum of squared deviations of $x$

**Interpretation:** $SE_b$ measures the typical distance between the sample slope $b$ and the true population slope $\beta$.

---

## 4. Confidence Interval for the Slope ($\beta$)

A confidence interval gives a range of plausible values for the true population slope.

**Formula:**
$$b \pm t^* \times SE_b$$

**Degrees of Freedom:** $df = n - 2$

**Required AP Exam Interpretation Template:**
*"We are [C%] confident that the true population slope of the relationship between [explanatory variable] and [response variable] is between [lower bound] and [upper bound] [units of y per unit of x]."*

**Example:**
A 95% confidence interval for the slope is $(1.2, 3.8)$. We are 95% confident that for each additional unit of $x$, the true mean $y$ increases by between 1.2 and 3.8 units.

---

## 5. Hypothesis Testing for the Slope ($\beta$)

We often test whether there is a **significant linear relationship** between $x$ and $y$.

### Null and Alternative Hypotheses

| Test Type | $H_0$ | $H_a$ |
| :--- | :--- | :--- |
| **Two-tailed** | $\beta = 0$ | $\beta \neq 0$ |
| **Left-tailed** | $\beta = 0$ | $\beta < 0$ |
| **Right-tailed** | $\beta = 0$ | $\beta > 0$ |

**Interpretation of $\beta = 0$:** There is **no linear relationship** between $x$ and $y$.

### Test Statistic

$$t = \frac{b - 0}{SE_b} = \frac{b}{SE_b}$$

**Degrees of Freedom:** $df = n - 2$

**Required AP Exam Interpretation Template:**
*"If there were truly no linear relationship between [explanatory variable] and [response variable] ($\beta = 0$), the probability of observing a sample slope as extreme as $b$ or more extreme is [p-value]."*

---

## 6. Confidence vs. Prediction Intervals

It's important to distinguish between these two types of intervals:

| Feature | Confidence Interval for Mean Response | Prediction Interval for Individual Response |
| :--- | :--- | :--- |
| **What it estimates** | Mean $y$ for a given $x$ | Single $y$ value for a given $x$ |
| **Width** | Narrower | Wider |
| **Formula** | $\hat{y} \pm t^* \times SE_{\hat{y}}$ | $\hat{y} \pm t^* \times \sqrt{SE_{\hat{y}}^2 + s^2}$ |
| **Used for** | Predicting the mean | Predicting an individual |

**Key Concept:** Prediction intervals are always **wider** than confidence intervals because they account for both the uncertainty in the regression line AND the natural variability of individual observations.

---

## 7. Standard Error of the Predicted Mean ($SE_{\hat{y}}$)

For a given $x$-value ($x^*$), the standard error of the predicted mean is:

$$SE_{\hat{y}} = s \sqrt{\frac{1}{n} + \frac{(x^* - \bar{x})^2}{\sum (x_i - \bar{x})^2}}$$

**Notice:** $SE_{\hat{y}}$ is **smallest** when $x^* = \bar{x}$ and gets **larger** as $x^*$ moves away from the mean.

---

## 8. Comparison Summary

| Concept | Symbol | Formula | What it measures |
| :--- | :--- | :--- | :--- |
| **Std Dev of Residuals** | $s$ | $\sqrt{\frac{\sum (y_i - \hat{y}_i)^2}{n - 2}}$ | Typical prediction error for individuals |
| **Std Error of Slope** | $SE_b$ | $\frac{s}{\sqrt{\sum (x_i - \bar{x})^2}}$ | Variability of sample slope |
| **Std Error of Predicted Mean** | $SE_{\hat{y}}$ | $s \sqrt{\frac{1}{n} + \frac{(x^* - \bar{x})^2}{\sum (x_i - \bar{x})^2}}$ | Variability of predicted mean |

---

## 9. Practice Questions

**Question 1:** A 95% confidence interval for the slope of a regression line is $(0.45, 0.82)$. Interpret this interval.

<details>
<summary>Click for Answer</summary>

We are 95% confident that the true population slope is between 0.45 and 0.82. This means that for each additional unit of the explanatory variable, the true mean response variable increases by between 0.45 and 0.82 units.
</details>

**Question 2:** A hypothesis test for the slope yields $p = 0.032$ at $\alpha = 0.05$. What conclusion should we draw?

<details>
<summary>Click for Answer</summary>

We are 95% confident that the true population slope is between 0.45 and 0.82. This means that for each additional unit of the explanatory variable, the true mean response variable increases by between 0.45 and 0.82 units.

</details>

**Question 3:** Explain why prediction intervals are wider than confidence intervals.

<details>
<summary>Click for Answer</summary>

Prediction intervals are wider because they must account for two sources of uncertainty: the variability in estimating the regression line itself, and the natural variability of individual observations around the line (the residual standard deviation $s$). Confidence intervals only account for the first source of uncertainty.
</details>

**Question 4:** A regression analysis has $n = 25$ data points. What degrees of freedom should be used for inference?

<details>
<summary>Click for Answer</summary>

$df = n - 2 = 25 - 2 = 23$ degrees of freedom.
</details>

**Question 5:** What is the relationship between the test statistic for slope and the confidence interval for slope?

<details>
<summary>Click for Answer</summary>

The test statistic t=b/SEbt=b/SEb​ and the confidence interval b±t∗×SEbb±t∗×SEb​ are directly related. If the confidence interval does not contain 0, then the test will be significant (reject H0:β=0H0​:β=0). Similarly, if the interval contains 0, we fail to reject the null hypothesis.
</details>

---

## 10. Key Formulas Quick Reference

| Concept | Formula |
| :--- | :--- |
| Standard Error of Slope | $SE_b = \frac{s}{\sqrt{\sum (x_i - \bar{x})^2}}$ |
| Confidence Interval for Slope | $b \pm t^* \times SE_b$ |
| Test Statistic for Slope | $t = \frac{b}{SE_b}$ |
| Standard Error of Predicted Mean | $SE_{\hat{y}} = s \sqrt{\frac{1}{n} + \frac{(x^* - \bar{x})^2}{\sum (x_i - \bar{x})^2}}$ |
| Confidence Interval for Mean Response | $\hat{y} \pm t^* \times SE_{\hat{y}}$ |
| Prediction Interval for Individual Response | $\hat{y} \pm t^* \times \sqrt{SE_{\hat{y}}^2 + s^2}$ |

---

## 11. Connection to Previous Lesson

| Topic | Lesson 5 (Assessing Fit) | Lesson 6 (More on Regression) |
| :--- | :--- | :--- |
| **Focus** | Describing the fitted model | Making inferences about the population |
| **Key Metrics** | $s$, $R^2$, residuals | $SE_b$, $t$-statistic, p-values |
| **Questions Answered** | How well does the line fit? | Is there a significant relationship? |
| **Tools** | Residual plots, $R^2$ | Confidence intervals, hypothesis tests |

---

## See Also

- [[Lesson 5: Assessing the Fit in Least-Squares Regression]]
- [[Correlation Coefficient r]]
- [[Confidence Intervals Review]]
- [[Hypothesis Testing Review]]
- [[Transforming Non-Linear Data]]

---

*Last Updated: 2026-07-22*