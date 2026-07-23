---
date: 2026-07-23
type: stats-concept
source: manual
status: reviewed
tags: [stats, confidence-intervals, inference]
---

# Introduction to Confidence Intervals

## 1. Core Concepts & Formulas

* **Sample Proportion ($\hat{p}$):** The proportion observed in a sample.
* **Standard Error ($\text{SE}$):** The estimated standard deviation of the sampling distribution of $\hat{p}$.
  $$\text{SE}(\hat{p}) = \sqrt{\frac{\hat{p}(1-\hat{p})}{n}}$$
* **Margin of Error ($\text{MoE}$):** The distance added and subtracted from the sample statistic. For a **95% confidence level**, we use approximately 2 Standard Errors (specifically $z^* \approx 1.96$):
  $$\text{MoE} = 2 \times \text{SE}(\hat{p})$$
* **Confidence Interval ($\text{CI}$):** 
  $$\text{CI} = \hat{p} \pm \text{MoE}$$

---

## 2. Worked Example

Suppose a poll shows Candidate A has a sample proportion of **$\hat{p} = 0.54$**, with a calculated **$\text{SE} = 0.05$**.

1. **Margin of Error:**
   $$\text{MoE} = 2 \times 0.05 = 0.10$$

2. **95% Confidence Interval:**
   $$\text{CI} = 0.54 \pm 0.10 \implies [0.44, 0.64]$$

---

## 3. Interpretation

* **Correct Interpretation:** We are **95% confident** that the true population proportion $p$ of voters supporting Candidate A lies between **0.44 and 0.64** (44% to 64%).
* **Technical Nuance:** The 95% probability describes the *method*, not the specific interval once calculated. If we took 100 repeated random samples, about 95 of the resulting confidence intervals would successfully capture the true population parameter $p$.