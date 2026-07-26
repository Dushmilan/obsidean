# Introduction to Confidence Intervals

A single poll result — say $\hat{p} = 0.54$ — looks precise, but it's just one snapshot from one sample. How much should you trust it? Confidence intervals answer that question by wrapping a margin of error around your estimate.

**The Intuition:** Imagine fishing in a lake where the true proportion $p$ is a fish you can't see. Each sample is a cast of your net. A 95% confidence interval means if you cast 100 times, about 95 of those nets will capture the fish. You don't know which ones — but you know the method works 95% of the time.

**The Math:** The standard error of a sample proportion is $\text{SE}(\hat{p}) = \sqrt{\frac{\hat{p}(1-\hat{p})}{n}}$. For 95% confidence, multiply by $z^* \approx 1.96$ (or just 2) to get the margin of error: $\text{MoE} = 2 \times \text{SE}(\hat{p})$. The interval is $\text{CI} = \hat{p} \pm \text{MoE}$.

**What does this mean for Statistics?** A confidence interval quantifies how much your estimate would vary if you repeated the study. It's the bridge between a single sample and a claim about the population — as long as you interpret it correctly (the probability is about the method, not the specific interval).

---

**Setup:** A poll shows $\hat{p} = 0.54$ with $\text{SE} = 0.05$. Construct a 95% CI.

**Solution:** $\text{MoE} = 2 \times 0.05 = 0.10$. $\text{CI} = 0.54 \pm 0.10 = [0.44,\, 0.64]$.

**Key insight:** We are 95% confident the true proportion $p$ lies between 44% and 64%. The width of this interval depends entirely on the standard error — smaller SE means tighter estimate.

---

**Interpretation trap:** "There's a 95% probability $p$ is in this interval" is technically wrong. The interval is fixed once calculated; $p$ is fixed too (we just don't know it). The 95% describes the long-run success rate of the procedure. Correct phrasing: "We are 95% confident that $p$ lies between..."

---
