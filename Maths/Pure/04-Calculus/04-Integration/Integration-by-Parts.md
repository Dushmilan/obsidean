
## Definition

The product rule in reverse:

$$\int u\,dv = uv - \int v\,du$$

Choose $u$ and $dv$ so that $\int v\,du$ is *simpler* than $\int u\,dv$.

**LIATE priority for $u$:** Logarithmic, Inverse trig, Algebraic, Trigonometric, Exponential.

## The Intuition

The integral of a product splits into an evaluated term minus a simpler integral. LIATE picks $u$ as the function that "simplifies on differentiation" — logs and inverse trig get simpler; exponentials don't.

## The Toolkit

| Pick $u$ as | Because |
|-------------|---------|
| $\ln x$, $\tan^{-1}x$ | derivative is algebraic |
| $x^n$ | derivative lowers the power |
| $\sin x$, $\cos x$ | cycles (repeat if needed) |
| $e^x$ | never changes (use as $dv$) |

## Derivation

Integrate $(uv)' = u'v + uv'$: $uv = \int v\,du + \int u\,dv$. Rearrange. [Full derivations: 04.4-Integration-Proofs]

## Method

1. Pick $u$ by LIATE; everything else is $dv$.
2. Differentiate $u$ → $du$; integrate $dv$ → $v$.
3. Apply $\int u\,dv = uv - \int v\,du$; repeat if the new integral is still a product.

## Worked Examples

**Setup:** $\int x e^x\,dx$.

**Solution:** $u = x$, $dv = e^xdx$: $= xe^x - \int e^x\,dx = xe^x - e^x + C$.

**Key insight:** LIATE: algebraic $x$ before exponential — one application suffices.

---

**Setup:** $\int \ln x\,dx$.

**Solution:** $u = \ln x$, $dv = dx$: $= x\ln x - \int 1\,dx = x\ln x - x + C$.

**Key insight:** Even with a single function, by parts works ($dv = dx$).

## Common Traps

- Choosing $u, dv$ so the new integral is *harder*
- Forgetting the minus sign: $uv - \int v\,du$
- Missing $+C$ on the final indefinite integral
- Loop integrals ($e^x\sin x$) need two passes then algebra

## Connections

- Substitution · Trigonometric-Integrals · Partial-Fractions-Integration
- 03-Logarithms — $\ln$ as $u$
- Definite-Integrals-and-FTC
