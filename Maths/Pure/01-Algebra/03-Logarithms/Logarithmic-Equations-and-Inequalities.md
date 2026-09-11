
## Definition

**Equations:** combine logs, convert to exponential form, solve, **check the domain**.

**Inequalities:** $\log_b f(x) < c \Rightarrow f(x) < b^c$ (if $b > 1$, direction preserved); if $0 < b < 1$, **direction flips**.

## The Intuition

"At least one" log problems: combine first, then unwrap with the exponential. The domain check is non-negotiable — squaring/combining can introduce extraneous roots. Inequalities are just equations plus a monotonicity decision.

## The Toolkit

| Situation | Move |
|-----------|------|
| Sum of logs | product rule, then convert |
| Log = constant | $\log_a f = c \Rightarrow f = a^c$ |
| Log on both sides | $\log_a f = \log_a g \Rightarrow f = g$ |
| Inequality, base > 1 | preserve direction |
| Inequality, base < 1 | flip direction |
| Always | intersect with domain $f > 0$ |

## Derivation

The equivalence $\log_a f = c \iff f = a^c$ is the definition. Inequality direction: $\log_b$ is increasing for $b>1$, decreasing for $0<b<1$. [Full derivations: 03-Logarithms-Proofs]

## Method

1. State the domain first.
2. Combine logs (product/quotient rules).
3. Convert to exponential (or equate logs).
4. Solve, then **discard roots outside the domain**.

## Worked Examples

**Setup:** $\log_2(x+3) + \log_2(x-1) = 3$.

**Solution:** Domain $x > 1$. Combine: $\log_2((x+3)(x-1)) = 3 \Rightarrow (x+3)(x-1) = 8 \Rightarrow x = -1 \pm 2\sqrt3$. Only $x = -1+2\sqrt3$ is in the domain.

**Key insight:** Always check roots against the domain — one may be extraneous.

---

**Setup:** $\log_2(x-1) < 3$.

**Solution:** Domain $x > 1$. Base $>1$: $x-1 < 8 \Rightarrow 1 < x < 9$.

**Key insight:** For $0 < a < 1$ the inequality flips.

## Common Traps

- Skipping the domain check
- Flipping inequalities only for base $0<a<1$
- Splitting $\log(a+b)$ incorrectly
- Forgetting the argument must stay positive throughout

## Connections

- Log-Laws-and-Identities · Surds
- Computing-Limits — exponential limits


## Cross-Track Connections

*Reconstructed 2026-08-24 after the registry-loss incident — see [[Maths-MOC]].*

- [[05-Applications-of-First-Order-ODEs]] — growth/decay models reduce to log equations
