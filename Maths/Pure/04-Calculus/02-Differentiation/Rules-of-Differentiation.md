
## Definition

The derivative is the instantaneous rate of change:

$$f'(x) = \lim_{h\to 0}\frac{f(x+h)-f(x)}{h}$$

The rules compute it without the limit every time.

## The Intuition

Your speedometer shows speed *right now* — the derivative of position. The rules are recipes: power rule for monomials, product rule for products, chain rule for nested functions. Pick the rule by the *outermost operation*.

## The Toolkit

| Rule | Formula | Use when |
|------|---------|----------|
| Power | $\frac{d}{dx}x^n = nx^{n-1}$ | monomials |
| Product | $(fg)' = f'g + fg'$ | products |
| Quotient | $\left(\frac{f}{g}\right)' = \frac{f'g - fg'}{g^2}$ | fractions |
| Chain | $\frac{d}{dx}f(g(x)) = f'(g(x))g'(x)$ | composition |
| Exp | $\frac{d}{dx}e^x = e^x$ | — |
| Log | $\frac{d}{dx}\ln x = \frac1x$ | — |
| Trig | $(\sin x)' = \cos x$; $(\cos x)' = -\sin x$; $(\tan x)' = \sec^2 x$ | — |

## Derivation

Power rule: expand $(x+h)^n$ and cancel $x^n$. Product rule: add and subtract $f(x+h)g(x)$ in the quotient. Chain rule: cancel $g(x+h)-g(x)$. All follow from the limit definition. [Full derivations: 04.2-Differentiation-Proofs]

## Method

1. Identify the outermost operation → first rule.
2. Product → product rule, then differentiate each factor (chain rule inside if needed).
3. Quotient → quotient rule (order: $f'g - fg'$).

## Worked Examples

**Setup:** Differentiate $y = x^2\sin(x^3)$.

**Solution:** Product: $y' = 2x\sin(x^3) + x^2\cos(x^3)\cdot3x^2 = 2x\sin(x^3) + 3x^4\cos(x^3)$.

**Key insight:** Product rule first; the composite factor gets the chain rule.

## Common Traps

- Chain rule forgotten on composites — the #1 mistake
- Quotient rule sign: $f'g - fg'$, not $f'g + fg'$
- $\frac{d}{dx}x^x \neq x\cdot x^{x-1}$ — the exponent is a variable; use logs
- Derivative of $|x|$ doesn't exist at 0

## Connections

- Implicit-and-Logarithmic-Differentiation · Higher-Derivatives
- 04.1-Limits-Continuity — where the rules come from
- 04.3-Applications-of-Differentiation — what they're for
