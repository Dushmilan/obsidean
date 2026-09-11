
## Definition

$\lim_{x\to a}f(x) = L$ means $f(x)$ gets arbitrarily close to $L$ as $x \to a$ (with $x \neq a$). Computation toolkit:

1. **Substitute** first.
2. **Factor and cancel** for $0/0$ rational forms.
3. **Isolate standard limits** ($\frac{\sin x}{x} \to 1$, $\frac{e^x-1}{x}\to 1$).
4. **L'Hôpital** for $0/0$ or $\infty/\infty$.

## The Intuition

Guessing where a function heads without arriving. A cancelled factor $(x-a)$ removes the "hole"; L'Hôpital swaps the problem for the ratio of derivatives — a faster way to see the same limit.

## The Toolkit

| Limit | Value |
|-------|-------|
| $\lim_{x\to0}\frac{\sin x}{x}$ | 1 (radians) |
| $\lim_{x\to0}\frac{e^x-1}{x}$ | 1 |
| $\lim_{x\to\infty}(1+\frac1x)^x$ | $e$ |
| L'Hôpital | $\lim\frac{f}{g} = \lim\frac{f'}{g'}$ for $0/0$, $\infty/\infty$ |
| Squeeze | $g\le f\le h$, limits equal → limit equal |

## Derivation

$\frac{\sin x}{x}\to 1$ from the unit-circle sector area argument; L'Hôpital from the Mean Value Theorem. [Full derivations: 04.1-Limits-Continuity-Proofs]

## Method

1. Substitute — if you get a number, done.
2. $0/0$ rational: factor, cancel, resubstitute.
3. Trig/exp: multiply/divide to expose standard limits.
4. L'Hôpital: only for indeterminate forms; re-apply while indeterminate.

## Worked Examples

**Setup:** $\lim_{x\to2}\frac{x^3-8}{x^2-4}$.

**Solution:** Factor: $\frac{(x-2)(x^2+2x+4)}{(x-2)(x+2)} \to \frac{12}{4} = 3$.

**Key insight:** Cancellation first — often avoids L'Hôpital.

---

**Setup:** $\lim_{x\to0}\frac{\sin 5x}{\sin 2x}$.

**Solution:** $\frac52\cdot\frac{\sin5x}{5x}\cdot\frac{2x}{\sin2x} \to \frac52$.

**Key insight:** Isolate the standard limits.

## Common Traps

- L'Hôpital on non-indeterminate forms
- $\frac{\sin x}{x}\to1$ needs radians
- Forgetting one-sided limits at corners
- Cancelling before checking the form is really $0/0$

## Connections

- Continuity · Definite-Integrals-and-FTC — improper limits
- 04.7-Series-Expansions — limits via series
- 07-Binomial-Theorem — $(1+1/x)^x \to e$
