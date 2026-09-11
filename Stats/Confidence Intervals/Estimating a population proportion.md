# Estimating a Population Proportion

## Definition

For binary data (yes/no, 1/0), the sample proportion $\hat p = x/n$ estimates the population proportion $p$:

$$\text{SE}(\hat p) = \sqrt{\frac{\hat p(1-\hat p)}{n}}, \qquad \text{CI} = \hat p \pm z^*\,\text{SE}$$

$z^* \approx 1.96$ (95%), $2.58$ (99%). Sample variance $s^2 = \frac{n}{n-1}\hat p(1-\hat p)$.

## The Intuition

Flip a biased coin 250 times, get 142 heads: $\hat p = 0.568$. Flip another 250 and you'd get a slightly different number — the SE tells you how much it wiggles across repeated experiments.

## The Toolkit

| Quantity | Formula |
|----------|---------|
| Sample proportion | $\hat p = \frac{x}{n}$ |
| SE | $\sqrt{\frac{\hat p(1-\hat p)}{n}}$ |
| Margin of error | $z^*\sqrt{\frac{\hat p(1-\hat p)}{n}}$ |
| Sample SD | $\sqrt{\frac{n}{n-1}\hat p(1-\hat p)}$ |

## Derivation

$\hat p$ is a sample mean of 0/1 values; the CLT applied to the binomial count gives approximate normality, with the checks $np \ge 10$, $n(1-p) \ge 10$. [Full derivations: [[Central Limit Theorem]]]

## Method

1. Check the CLT conditions ($np$, $n(1-p) \ge 10$) and randomness.
2. Compute $\hat p$, SE, then MoE with the right $z^*$.
3. Report and interpret as a *method* statement.

## Worked Examples

**Setup:** $n = 250$, $x = 142$. 99% CI for $p$?

**Solution:** $\hat p = 0.568$; $\text{SE} = \sqrt{0.568\times0.432/250} \approx 0.031$; $\text{MoE} = 2.58\times0.031 \approx 0.081$; CI $= [0.487, 0.649]$.

**Key insight:** $s \approx 0.50$ (individual 0/1s) vs $\text{SE} \approx 0.031$ (the statistic) — averaging crushes the noise.

---

**Setup:** Why quadruple the sample to halve the MoE?

**Solution:** $\text{SE} \propto 1/\sqrt n$ — $n \to 4n$ halves the SE.

**Key insight:** The $\sqrt n$ trade-off: precision costs quadratically in sample size.

## Common Traps

- Confidence level vs interval width — 99% is always wider than 95% for the same data
- Forgetting the CLT checks for proportions
- Interpreting a single interval probabilistically
- Using the sample SD where the SE belongs

## Connections

- [[Introduction]] · [[Central Limit Theorem]] — why it's normal
- [[Common Distributions]] — the binomial origin
- [[Maths/Pure/01-Algebra/02-Indices/02-Indices]] — $\sqrt n$ scaling
