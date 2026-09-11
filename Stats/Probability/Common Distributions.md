# Common Distributions

## Definition

A handful of distributions appear constantly — identify the situation, read off the parameters, use the precomputed mean/variance:

| Distribution | Used for | $E[X]$ | $\text{Var}(X)$ |
|---|---|---|---|
| **Binomial** $\text{Bin}(n,p)$ | # successes in $n$ trials | $np$ | $np(1-p)$ |
| **Geometric** $\text{Geom}(p)$ | trials until first success | $1/p$ | $(1-p)/p^2$ |
| **Poisson** $\text{Pois}(\lambda)$ | events in a fixed interval | $\lambda$ | $\lambda$ |
| **Normal** $N(\mu,\sigma)$ | sums of many small effects | $\mu$ | $\sigma^2$ |
| **Uniform** $U(a,b)$ | equally likely on an interval | $\frac{a+b}{2}$ | $\frac{(b-a)^2}{12}$ |

## The Intuition

A distribution is a fingerprint of a random process. Count successes → binomial; distance from a mean in SE units → normal; time until first success → geometric. Each has a shape and mean/variance you look up once.

## The Toolkit

| Distribution | PMF/PDF |
|--------------|---------|
| Binomial | $P(X=k) = \binom{n}{k}p^k(1-p)^{n-k}$ |
| Poisson | $P(X=k) = \frac{e^{-\lambda}\lambda^k}{k!}$ |
| Normal | $f(x) = \frac{1}{\sigma\sqrt{2\pi}}e^{-(x-\mu)^2/2\sigma^2}$ |
| Standard normal | $z = \frac{x-\mu}{\sigma} \sim N(0,1)$ |

## Derivation

The binomial PMF counts $\binom{n}{k}$ orderings of $k$ successes. Poisson is the limit of $\text{Bin}(n, p)$ as $n\to\infty$, $p\to 0$ with $\lambda = np$ fixed. The normal PDF arises from the CLT. $t$, $\chi^2$, and $F$ are all built from normals and squared normals. [Full derivations: [[Central Limit Theorem]]]

## Method

1. Match the situation to the distribution.
2. Identify parameters ($n, p$; $\lambda$; $\mu, \sigma$).
3. Use the PMF for exact probabilities; the normal for approximations.

## Worked Examples

**Setup:** 10 quiz questions, 4 options each, pure guessing. P(exactly 3 correct)?

**Solution:** $X\sim\text{Bin}(10, 0.25)$: $P(X=3) = \binom{10}{3}(0.25)^3(0.75)^7 \approx 0.25$.

**Key insight:** Mean $np = 2.5$, so 3 correct is near the centre — moderate probability.

---

**Setup:** Call centre, 5 calls/min average. P(exactly 3 next minute)?

**Solution:** $X\sim\text{Pois}(5)$: $P(X=3) = \frac{e^{-5}5^3}{3!} \approx 0.140$.

**Key insight:** Poisson's mean equals its variance — a signature property.

## Common Traps

- Binomial requires *independent* trials — without replacement it's hypergeometric
- Geometric (trials *until* success) vs binomial (count in *fixed* trials)
- Using normal for small samples without CLT justification
- Confusing $p$ (probability) with $\lambda$ (rate) parameterizations

## Connections

- [[Random Variables]] · [[Central Limit Theorem]]
- [[Estimating a population proportion]] — binomial proportions
- [[Maths/Pure/01-Algebra/06-Permutations-Combinations/06-Permutations-Combinations]] — $\binom{n}{k}$
