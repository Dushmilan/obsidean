# Estimating a Population Proportion

When your data is binary — success or failure, yes or no, 1 or 0 — the sample proportion $\hat{p} = x/n$ is your statistic of interest. But how reliable is it? This is where standard error and confidence intervals for proportions come in.

**The Intuition:** Think of flipping a biased coin 250 times and getting 142 heads. Your $\hat{p} = 0.568$ is an estimate, but if you flipped another 250 times you'd get a slightly different number. The standard error tells you how much that number would wiggle across repeated experiments.

**The Math:** For binary data, the sample variance is $s^2 = \frac{n}{n-1}\hat{p}(1-\hat{p})$ and the standard error is $\text{SE}(\hat{p}) = \sqrt{\frac{\hat{p}(1-\hat{p})}{n}}$. The margin of error scales with $z^*$: use $z^* \approx 1.96$ for 95% confidence, $z^* \approx 2.58$ for 99%. The interval is $\hat{p} \pm z^* \times \text{SE}(\hat{p})$.

**What does this mean for Statistics?** The SE shrinks with $\sqrt{n}$ — quadrupling your sample halves the margin of error. The trade-off between confidence level and interval width is direct: 99% CI is always wider than 95% CI for the same data.

---

**Setup:** $n = 250$, $x = 142$. Construct a 99% CI for $p$.

**Solution:** $\hat{p} = 142/250 = 0.568$. $s^2 = \frac{250}{249}(0.568)(0.432) \approx 0.2464$, $s \approx 0.50$. $\text{SE} = \sqrt{0.568 \times 0.432 / 250} \approx 0.0313$. $\text{MoE} = 2.58 \times 0.0313 \approx 0.081$. $\text{CI} = [0.487,\, 0.649]$.

**Key insight:** The sample standard deviation $s \approx 0.50$ measures variation among individual 0s and 1s. The standard error $\text{SE} \approx 0.031$ measures how much $\hat{p}$ itself varies — a much smaller number because averaging reduces noise.

---

**Setup:** A poll gives $\hat{p} = 0.54$, $\text{SE} = 0.05$. Find the 95% CI.

**Solution:** $\text{MoE} = 2 \times 0.05 = 0.10$. $\text{CI} = [0.44,\, 0.64]$.

**Key insight:** The $z^* \approx 2$ shortcut for 95% is good enough for most purposes — the exact value 1.96 matters only when precision is critical.

---

**Frequentist interpretation:** If we took many random samples of size $n = 250$ and built CIs the same way, about 99% of those intervals would contain the true $p$. The specific interval $[0.487, 0.649]$ either contains $p$ or it doesn't — we just don't know which. The confidence level quantifies our trust in the method, not in any single interval.

---
