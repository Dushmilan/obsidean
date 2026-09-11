# Introduction to Confidence Intervals

## Definition

A confidence interval wraps a **margin of error** around an estimate:

$$\text{CI} = \hat{p} \pm \text{MoE}, \qquad \text{MoE} = z^*\times\text{SE}, \qquad \text{SE}(\hat{p}) = \sqrt{\frac{\hat p(1-\hat p)}{n}}$$

For 95% confidence, $z^* \approx 1.96 \approx 2$.

## The Intuition

Fishing in a lake where the true proportion $p$ is a fish you can't see: each sample is a cast. A 95% CI means ~95 of 100 casts capture the fish — you don't know which ones, but the *method* works 95% of the time.

## The Toolkit

| Quantity | Formula |
|----------|---------|
| SE of proportion | $\sqrt{\frac{\hat p(1-\hat p)}{n}}$ |
| Margin of error | $z^*\times\text{SE}$ |
| 95% CI | $\hat p \pm 1.96\,\text{SE}$ |
| 99% CI | $\hat p \pm 2.58\,\text{SE}$ |

## Derivation

The interval follows from the CLT: $\hat p$ is approximately normal, so ±2 SE captures ~95% of the sampling distribution. [Full derivations: [[Central Limit Theorem]]]

## Method

1. Check the sample is random and unbiased — otherwise the interval is centred wrong.
2. Compute SE, then MoE = $z^*\times\text{SE}$.
3. Report $\hat p \pm \text{MoE}$ with the correct interpretation.

## Worked Examples

**Setup:** $\hat p = 0.54$, $\text{SE} = 0.05$. 95% CI?

**Solution:** $\text{MoE} = 0.10$; CI $= [0.44, 0.64]$.

**Key insight:** Width depends entirely on the SE — tighter estimates need smaller SE.

## Common Traps

- "95% probability $p$ is in this interval" is wrong — the interval is fixed; 95% is the method's long-run success rate
- A biased sample invalidates the interval — no formula repairs a bad sample
- Confusing SE with SD
- Interpreting the interval as containing individual data points

## Connections

- [[Estimating a population proportion]] · [[Central Limit Theorem]]
- [[Sampling Methods]] · [[Bias in Sampling]] — the hidden assumptions
- [[Maths/Pure/04-Calculus/01-Limits-Continuity/04.1-Limits-Continuity]] — convergence
