
## Definition

Decompose a proper rational function then integrate term by term:

$$\frac{P(x)}{Q(x)} = \sum \text{partial fractions} \Rightarrow \int \text{(linear)} = A\ln|ax+b|, \quad \int \text{(quadratic)} = \text{arctan/log forms}$$

Requires $\deg P < \deg Q$ — divide first if not.

## The Intuition

The decomposition turns one hard integral into several easy ones: $\ln$ for linear factors, $\tan^{-1}$ for irreducible quadratics. "Un-mixing" the fraction is the only real work.

## The Toolkit

| Factor of $Q$ | Integral result |
|---------------|-----------------|
| $(ax+b)$ | $\frac{A}{a}\ln|ax+b|$ |
| $(ax+b)^n$ | $\frac{A}{(1-n)a}(ax+b)^{1-n}$ (for $n>1$) |
| $ax^2+bx+c$ (irreducible) | $\tan^{-1}$ form (complete the square) |
| repeated quadratic | combine ln + arctan |

## Derivation

The decomposition is an algebraic identity; integrating each term uses $\int \frac{1}{u}du = \ln|u|$ and $\int \frac{du}{u^2+1} = \tan^{-1}u$. [Full derivations: 05-Partial-Fractions]

## Method

1. If improper, divide first.
2. Factor $Q$; write the template.
3. Find constants (cover-up / equating coefficients).
4. Integrate each term.

## Worked Examples

**Setup:** $\int \frac{5x+1}{(x-1)(x+2)}\,dx$.

**Solution:** $\frac{2}{x-1}+\frac{3}{x+2}$ (cover-up). Integral $= 2\ln|x-1| + 3\ln|x+2| + C$.

**Key insight:** Cover-up finds constants fast; logs follow.

---

**Setup:** $\int \frac{dx}{x^2+1}$.

**Solution:** $\tan^{-1}x + C$ directly — the prototype quadratic.

**Key insight:** Irreducible quadratics always lead to arctangents after completing the square.

## Common Traps

- Integrating an improper fraction without dividing
- Missing a repeated-factor term in the template
- Forgetting the absolute value in $\ln$
- Irreducible quadratic: complete the square before integrating

## Connections

- Substitution · Integration-by-Parts · 05-Partial-Fractions
- Definite-Integrals-and-FTC
