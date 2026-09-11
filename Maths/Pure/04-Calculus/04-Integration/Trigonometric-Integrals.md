
## Definition

Systematic strategies for $\int \sin^m x\cos^n x\,dx$ and $\int \tan^m x\sec^n x\,dx$:

| Case | Strategy |
|------|----------|
| $m$ odd (sin) | peel one $\sin$, $u = \cos x$ |
| $n$ odd (cos) | peel one $\cos$, $u = \sin x$ |
| both even | half-angle formulas |
| $\tan^m\sec^n$, $n$ even | $u = \tan x$ |
| $\tan^m\sec^n$, $m$ odd | $u = \sec x$ |

## The Intuition

Powers of trig integrate cleanly when you can arrange a derivative: odd powers give you the "spare" factor that becomes $du$. Even powers need double-angle identities to flatten them.

## The Toolkit

| Identity | Use |
|----------|-----|
| $\sin^2 x = \frac{1-\cos 2x}{2}$ | even powers |
| $\cos^2 x = \frac{1+\cos 2x}{2}$ | even powers |
| $\sin^2 x + \cos^2 x = 1$ | peeling odd powers |
| $\tan^2 x = \sec^2 x - 1$ | tan/sec pairs |

## Derivation

All strategies reduce the integrand to a derivative pattern via the Pythagorean identity and $u$-substitution. [Full derivations: 04.4-Integration-Proofs]

## Method

1. Odd power of $\sin$ or $\cos$: peel one factor → substitute the other.
2. Both even: half-angle identities.
3. Products of different angles: product-to-sum formulas.

## Worked Examples

**Setup:** $\int \sin^3 x\cos^2 x\,dx$.

**Solution:** $\sin^3 = \sin(1-\cos^2)$; $u = \cos x$: $\int -(1-u^2)u^2\,du = \frac{u^5}{5}-\frac{u^3}{3}+C$.

**Key insight:** The odd sine provides the $du$.

---

**Setup:** $\int \sin^2 x\,dx$.

**Solution:** $\frac12\int(1-\cos 2x)dx = \frac{x}{2} - \frac{\sin 2x}{4} + C$.

**Key insight:** Even power → half-angle identity.

## Common Traps

- Using half-angle when a simple peel works
- Wrong sign from $du = -\sin x\,dx$
- Mixing $\sin$/$u=\cos$ vs $\cos$/$u=\sin$ pairings
- Forgetting product-to-sum for mixed angles

## Connections

- Substitution · Integration-by-Parts · 03.1-Trigonometric-Functions-Identities
- 04.7-Series-Expansions — trig series
