---
date: 2026-07-23
type: stats-concept
source: manual
status: reviewed
tags: [stats, confidence-intervals, proportion]
---

## 1. Core Concepts & Formulas

* **Sample Proportion ($\hat{p}$):** The proportion of successes in a sample, defined as $\hat{p} = \frac{x}{n}$, where $x$ is the number of successes and $n$ is the total sample size.
* **Sample Variance ($s^2$):** For binary Bernoulli-distributed data ($1$s and $0$s):
  $$s^2 = \frac{n}{n-1} \hat{p}(1 - \hat{p})$$
* **Sample Standard Deviation ($s$):** The standard deviation of the individual binary observations:
  $$s = \sqrt{s^2} \approx \sqrt{\hat{p}(1 - \hat{p})}$$
* **Standard Error ($\text{SE}$ or $\sigma_{\hat{p}}$):** The estimated standard deviation of the sampling distribution of $\hat{p}$:
  $$\text{SE}(\hat{p}) = \frac{s}{\sqrt{n}} = \sqrt{\frac{\hat{p}(1 - \hat{p})}{n}}$$
* **Margin of Error ($\text{MoE}$):** The maximum expected distance between the sample statistic and the true population parameter:
  $$\text{MoE} = z^* \times \text{SE}(\hat{p})$$
  * For **95% Confidence Level:** $z^* \approx 1.96$ (or $\approx 2$)
  * For **99% Confidence Level:** $z^* \approx 2.58$
* **Confidence Interval ($\text{CI}$):** 
  $$\text{CI} = \hat{p} \pm \text{MoE}$$

---

## 2. Worked Examples

### Example 1: 99% Confidence Interval for Sample Proportion

**Problem Statement:** A sample of $n = 250$ trials produced $x = 142$ successes (coded as $1$) and $108$ failures (coded as $0$). Construct a **99% Confidence Interval** for the population proportion $p$.

#### Step-by-Step Solution:

1. **Calculate the Sample Proportion ($\hat{p}$):**
   $$\hat{p} = \frac{142}{250} = 0.568$$

2. **Calculate Sample Variance ($s^2$) & Sample Standard Deviation ($s$):**
   $$s^2 = \frac{250}{249} \times 0.568 \times (1 - 0.568) \approx 0.2464$$
   $$s = \sqrt{0.2464} \approx 0.4963 \approx 0.50$$

3. **Calculate the Standard Error ($\text{SE}$):**
   $$\text{SE}(\hat{p}) = \frac{s}{\sqrt{n}} = \frac{0.4963}{\sqrt{250}} = \sqrt{\frac{0.568 \times 0.432}{250}} \approx 0.0313$$

4. **Calculate Margin of Error ($\text{MoE}$) for 99% Confidence ($z^* = 2.58$):**
   $$\text{MoE} = 2.58 \times 0.0313 \approx 0.0808 \approx 0.08$$

5. **Construct the 99% Confidence Interval:**
   $$\text{CI} = 0.568 \pm 0.081 \implies [0.487, 0.649]$$

---

### Example 2: 95% Confidence Interval (Voter Poll)

**Problem Statement:** A sample poll shows Candidate A received $\hat{p} = 0.54$ with a calculated Standard Error $\text{SE} = 0.05$.

1. **Margin of Error ($\text{MoE}$):**
   $$\text{MoE} = 2 \times 0.05 = 0.10$$

2. **95% Confidence Interval:**
   $$\text{CI} = 0.54 \pm 0.10 \implies [0.44, 0.64]$$

---

## 3. Important Statistical Interpretations

* **Correct Interpretation:** We are **99% confident** that the true population proportion $p$ lies within the interval **$[0.487, 0.649]$**.
* **Distinction between SD and SE:**
  * **Sample Standard Deviation ($s \approx 0.50$):** Measures the variation among *individual data points* in the sample ($0$s and $1$s).
  * **Standard Error ($\text{SE} \approx 0.031$):** Measures the uncertainty/variability of the *sample statistic ($\hat{p}$)* across repeated sampling.
* **Frequentist Meaning of Confidence Level:** The $99\%$ confidence level describes the *reliability of the procedure*. If we were to take many random samples of size $n = 250$ and construct intervals in the same way, approximately $99\%$ of those intervals would contain the true parameter $p$.

