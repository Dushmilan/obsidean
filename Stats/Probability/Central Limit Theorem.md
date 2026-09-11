# Central Limit Theorem

## Definition

If $X_1,\dots,X_n$ are independent draws from a distribution with mean $\mu$ and SD $\sigma$, then for large $n$:

$$\bar{X} \approx N\left(\mu, \frac{\sigma}{\sqrt{n}}\right), \qquad z = \frac{\bar{X}-\mu}{\sigma/\sqrt{n}} \approx N(0,1)$$

$\frac{\sigma}{\sqrt{n}}$ is the **standard error** of the mean — it shrinks with $\sqrt{n}$.

**Conditions:** random sample, independent observations, $n$ large enough (the more skewed the population, the larger $n$). For proportions: $np \ge 10$, $n(1-p) \ge 10$.

## The Intuition

Take *any* distribution — skewed, lumpy, discrete. Draw samples of size $n$, compute the mean, repeat: the distribution of *sample means* becomes normal as $n$ grows. The data doesn't need to be normal — the *sample mean* is what becomes normal. That's why $n = 30$ is the rule of thumb.

## The Toolkit

| Quantity | Formula |
|----------|---------|
| Standard error | $\text{SE} = \frac{\sigma}{\sqrt{n}}$ |
| Z-statistic | $z = \frac{\bar{X}-\mu}{\sigma/\sqrt{n}}$ |
| Proportion SE | $\text{SE} = \sqrt{\frac{p(1-p)}{n}}$ |
| Continuity correction | use $k \pm 0.5$ for discrete→normal |

## Derivation

The CLT is a theorem about sums: standardised sums converge to the normal distribution (Lindeberg–Lévy). The $\sqrt{n}$ in the SE comes from $\text{Var}(\bar{X}) = \sigma^2/n$ — averaging compresses spread. [Full derivations: [[Random Variables]]]

## Method

1. Check the conditions (random, independent, large $n$).
2. Compute the SE $\sigma/\sqrt{n}$.
3. Standardise and use the normal table; apply the continuity correction for discrete counts.

## Worked Examples

**Setup:** Population $\mu = 100$, $\sigma = 15$; $n = 36$. P(sample mean > 105)?

**Solution:** $\text{SE} = 15/6 = 2.5$; $z = 2$; $P(Z>2) = 0.0228$.

**Key insight:** Values spread widely ($\sigma=15$) but means of 36 concentrate ($\text{SE}=2.5$).

---

**Setup:** A fair coin flipped 400 times. P(220 or more heads)?

**Solution:** $X\sim\text{Bin}(400, 0.5)$: $\mu = 200$, $\sigma = 10$. $z = (219.5-200)/10 = 1.95$; $P \approx 0.026$.

**Key insight:** The continuity correction ($219.5$) nudges the discrete count into the continuous normal world.

## Common Traps

- The CLT says the *mean's* distribution is normal — not the data
- CLT doesn't apply to every statistic (medians converge slower; $R^2$ has its own distribution)
- SE vs SD — SE is the SD of the *sampling distribution*
- Using normal approximation without checking $np, n(1-p)$

## Connections

- [[Law of Large Numbers]] — why estimates converge · [[Common Distributions]]
- [[Estimating a population proportion]] · [[Confidence Intervals/Confidence Intervals_Index]]
- [[Maths/Pure/04-Calculus/01-Limits-Continuity/04.1-Limits-Continuity]] — limits
