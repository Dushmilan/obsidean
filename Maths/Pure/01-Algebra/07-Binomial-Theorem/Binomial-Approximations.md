
## Definition

For $n \in \mathbb{Q}$ and $|x| < 1$:

$$(1+x)^n = \sum_{r=0}^{\infty}\binom{n}{r}x^r, \qquad \binom{n}{r} = \frac{n(n-1)\cdots(n-r+1)}{r!}$$

**Linear approximation:** $(1+x)^n \approx 1 + nx$ for small $x$.

## The Intuition

Near 1, $(1+x)^n$ is almost linear — the first-order term $1 + nx$ captures most of the change. Keep the quadratic term for better accuracy: $\approx 1 + nx + \frac{n(n-1)}{2}x^2$.

## The Toolkit

| Approximation | Form | Valid |
|---------------|------|-------|
| First order | $1 + nx$ | $|x|$ small |
| Second order | $1 + nx + \frac{n(n-1)}{2}x^2$ | $|x|$ small |
| $\sqrt{1+x}$ | $1 + \frac12 x - \frac18 x^2 + \cdots$ | $|x|<1$ |
| $\frac{1}{1-x}$ | $1 + x + x^2 + \cdots$ | $|x|<1$ |
| $\frac{1}{1+x}$ | $1 - x + x^2 - \cdots$ | $|x|<1$ |

## Derivation

The coefficients come from Taylor expansion at $x = 0$; the radius of convergence is 1. [Full derivations: 04.7-Series-Expansions]

## Method

1. Factor the expression into the form $a^n(1+u)^n$.
2. Check $|u| < 1$.
3. Expand to the needed order; keep terms up to the required power of $x$.

## Worked Examples

**Setup:** Approximate $\sqrt{1.02}$.

**Solution:** $(1+0.02)^{1/2} \approx 1 + \frac12(0.02) - \frac18(0.02)^2 = 1.00995$.

**Key insight:** Second order is often enough for sensible accuracy.

---

**Setup:** Expand $\frac{1}{(2-x)}$ up to $x^2$ (small $x$).

**Solution:** $\frac12(1 - \frac{x}{2})^{-1} = \frac12\left(1 + \frac{x}{2} + \frac{x^2}{4} + \cdots\right) = \frac12 + \frac{x}{4} + \frac{x^2}{8} + \cdots$.

**Key insight:** Factor out the constant to hit the standard $(1+u)^n$ form.

## Common Traps

- Forgetting $|x| < 1$ for rational $n$
- Truncating too early (keep one term past the target order)
- Using the integer formula's $\frac{n!}{r!(n-r)!}$ for rational $n$
- Neglecting the $a^n$ factor when the base isn't 1

## Connections

- General-Term-and-Coefficients · 04.7-Series-Expansions
- Computing-Limits — limits via expansions


## Cross-Track Connections

*Reconstructed 2026-08-24 after the registry-loss incident — see [[Maths-MOC]].*

> Original links unrecoverable; topic-level mappings live in [[Maths-Cross_Index]].
