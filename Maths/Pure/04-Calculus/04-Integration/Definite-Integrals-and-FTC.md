

**Fundamental Theorem of Calculus:**
- **Part 1:** $g(x) = \int_a^x f(t)\,dt \Rightarrow g'(x) = f(x)$.
- **Part 2:** $\int_a^b f(x)\,dx = F(b) - F(a)$ where $F' = f$.

**Properties:** linearity, additivity over intervals, $\int_a^b = -\int_b^a$, $\int_a^a = 0$.

**Improper integrals:** $\int_a^\infty f(x)\,dx = \lim_{b\to\infty}\int_a^b f(x)\,dx$ (check convergence).

## The Intuition

If differentiation reads a speedometer, the definite integral reads the odometer: $\int_a^b v(t)\,dt$ = total displacement. The FTC says "integral of the rate = total change" — the single most important idea in calculus.

## The Toolkit

| Result | Formula |
|--------|---------|
| FTC 2 | $\int_a^b f = F(b) - F(a)$ |
| Average value | $\frac{1}{b-a}\int_a^b f(x)\,dx$ |
| Area | $\int_a^b |f(x)|\,dx$ for total area |
| Improper | $\lim$ definition; converge if finite |

## Derivation

FTC 1: differentiate the area function — the height at $x$ is $f(x)$. FTC 2: $\int_a^b f = g(b) - g(a) = F(b) - F(a)$ (any antiderivative differs by a constant). [Full derivations: 04.4-Integration-Proofs]

## Method

1. Find an antiderivative (techniques).
2. Evaluate $F(b) - F(a)$; change limits when substituting.
3. Total area: split at sign changes and use $|f|$.

## Worked Examples

**Setup:** $\int_0^1 x e^x\,dx$.

**Solution:** $[xe^x - e^x]_0^1 = (e-e) - (0-1) = 1$.

**Key insight:** By parts first, then evaluate at the limits.

---

**Setup:** $\int_1^\infty \frac{1}{x^2}\,dx$.

**Solution:** $\lim_{b\to\infty}[-\frac1x]_1^b = \lim(1 - \frac1b) = 1$ — converges.

**Key insight:** $1/x^p$ converges for $p > 1$ over $[1,\infty)$.

## Common Traps

- Substitution without changing limits
- Improper integrals treated as ordinary definite integrals
- Confusing total area with signed area
- $\int_0^1 1/x\,dx$ diverges — check the singularity

## Connections

- Substitution · Integration-by-Parts · Trigonometric-Integrals
- 04.1-Limits-Continuity — the limit backbone
- 04.5-Applications-of-Integration — what definite integrals compute
