# Law of Large Numbers

## Definition

For independent $X_1, X_2, \dots$ with common mean $\mu$ and finite variance, the sample mean converges **in probability** to $\mu$:

$$\lim_{n\to\infty} P\left(\left|\frac{X_1+\cdots+X_n}{n} - \mu\right| \ge \varepsilon\right) = 0$$

**Weak law** (Chebyshev bound): $P(|\bar{X}_n - \mu| \ge \varepsilon) \le \frac{\sigma^2}{n\varepsilon^2}$. The strong LLN guarantees convergence with probability 1.

## The Intuition

Flip a coin 10 times and get 8 heads — proportion 0.8, far from 0.5. Flip 10,000 times and the proportion hugs 0.5. The gap shrinks because early wild swings get *diluted* by the mass of later flips — not because the coin "remembers" and compensates. Independent flips have no memory.

## The Toolkit

| Result | Statement |
|--------|-----------|
| Weak LLN | $\bar{X}_n \xrightarrow{p} \mu$ |
| Strong LLN | $\bar{X}_n \xrightarrow{a.s.} \mu$ |
| Chebyshev bound | $P(|\bar{X}_n-\mu|\ge\varepsilon) \le \frac{\sigma^2}{n\varepsilon^2}$ |
| Consistency | $\hat{p} \to p$, $\bar{x} \to \mu$ with enough data |

## Derivation

Chebyshev's inequality $P(|X-\mu|\ge\varepsilon) \le \text{Var}(X)/\varepsilon^2$ applied to $\bar{X}_n$ (whose variance is $\sigma^2/n$) gives the weak-law bound directly. [Full derivations: [[Random Variables]]]

## Method

1. Use the LLN to justify estimating a parameter with a sample statistic.
2. Combine with the CLT for the *shape* of the fluctuation.
3. Spot the gambler's fallacy: past outcomes don't "owe" a correction.

## Worked Examples

**Setup:** Roll a die 600 times. How close to 3.5 is the average?

**Solution:** $\text{SE} = 1.71/\sqrt{600} \approx 0.07$ — the average will be about $3.5 \pm 0.07$.

**Key insight:** LLN says *why* it converges; CLT says *how fast and in what shape*.

---

**Setup:** A gambler says a losing streak is "due" to end.

**Solution:** Gambler's fallacy — independent games have no memory. The LLN dilutes the past; it never compensates it.

**Key insight:** LLN is about *many trials*, not individual outcomes — every flip is still 50/50.

## Common Traps

- Confusing LLN (averages converge to the mean) with CLT (the *shape* of the fluctuation)
- Believing a short streak "must" balance — no compensation mechanism
- Forgetting finite variance is required
- Treating convergence as a guarantee for small samples

## Connections

- [[Central Limit Theorem]] — the companion theorem · [[Random Variables]]
- [[Introduction to Probability]] — long-run frequency
- [[Maths/Pure/04-Calculus/01-Limits-Continuity/04.1-Limits-Continuity]] — limits
