
## Definition

The chain rule in reverse:

$$\int f(g(x))\,g'(x)\,dx = \int f(u)\,du, \qquad u = g(x), \ du = g'(x)dx$$

For definite integrals, change the limits too: $\int_a^b f(g(x))g'(x)\,dx = \int_{g(a)}^{g(b)} f(u)\,du$.

## The Intuition

Spot a function *and its derivative* in the integrand — that's the signature of substitution. The $du$ "absorbs" the derivative, leaving a simpler integral in $u$.

## The Toolkit

| Pattern | Substitution |
|---------|-------------|
| $f(g(x))g'(x)$ | $u = g(x)$ |
| $\sqrt{a^2 - x^2}$ | $x = a\sin\theta$ |
| $\sqrt{a^2 + x^2}$ | $x = a\tan\theta$ |
| $x^2 - a^2$ | $x = a\sec\theta$ |
| Rational in $\sin/\cos$ | $t = \tan\frac{x}{2}$ |

## Derivation

$u = g(x) \Rightarrow du = g'(x)dx$; substitute and integrate — the FTC guarantees the result matches. [Full derivations: 04.4-Integration-Proofs]

## Method

1. Identify $u$ (the inner function whose derivative appears).
2. Substitute, integrate in $u$, substitute back.
3. Definite integrals: **change the limits** or substitute back before evaluating.

## Worked Examples

**Setup:** $\int \sin^3 x\cos x\,dx$.

**Solution:** $u = \sin x$, $du = \cos x\,dx$: $\int u^3 du = \frac{u^4}{4} + C = \frac{\sin^4 x}{4} + C$.

**Key insight:** $\cos x$ is exactly the derivative of $\sin x$ — the substitution signature.

---

**Setup:** $\int_0^{\pi/2}\sin x\cos x\,dx$ (using $u$).

**Solution:** $u = \sin x$: limits $0 \to 0$, $\frac{\pi}{2} \to 1$: $\int_0^1 u\,du = \frac12$.

**Key insight:** Changing limits avoids substituting back.

## Common Traps

- Missing the $du$ factor — the integrand must contain $g'(x)$
- Forgetting to change limits in definite integrals
- Substituting back inconsistently
- Choosing $u$ with no derivative present

## Connections

- Integration-by-Parts · Trigonometric-Integrals · Partial-Fractions-Integration
- 04.2-Differentiation — the reverse chain rule
- Definite-Integrals-and-FTC
