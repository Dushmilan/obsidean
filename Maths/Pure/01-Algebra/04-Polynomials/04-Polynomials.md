
## Definition

A **polynomial in $x$** (degree $n$):

$$P(x) = a_n x^n + a_{n-1} x^{n-1} + \cdots + a_1 x + a_0, \qquad a_n \neq 0$$

Powers are whole numbers (non-negative integers). Polynomials are the simplest "smooth" functions — differentiable everywhere, factorable completely over the complex numbers, and their roots encode the structure of the systems they model.

## The Intuition

A polynomial is a recipe with ingredients at different "levels" (powers). The Factor Theorem says: if you plug in $x = a$ and get zero, then $(x-a)$ is a perfect ingredient that can be factored out. Dividing $P(x)$ by $(x-a)$ gives $P(x) = (x-a)Q(x) + R$; setting $x = a$ kills the $(x-a)$ term, leaving $P(a) = R$ — the Remainder Theorem in one line.

## The Toolkit

| Result | Formula | Valid when |
|--------|---------|-----------|
| Remainder Theorem | $P(x) = (x-a)Q(x) + R \Rightarrow P(a) = R$ | $a$ real |
| Factor Theorem | $(x-a) \mid P(x) \iff P(a) = 0$ | — |
| Vieta (quadratic) | $\alpha + \beta = -\dfrac{b}{a}$, $\alpha\beta = \dfrac{c}{a}$ | $ax^2+bx+c$ |
| Vieta (cubic) | $\sum\alpha = -\dfrac{b}{a}$, $\sum\alpha\beta = \dfrac{c}{a}$, $\alpha\beta\gamma = -\dfrac{d}{a}$ | $ax^3+bx^2+cx+d$ |
| Discriminant | $\Delta = b^2 - 4ac$ | determines root type |
| Difference of squares | $a^2 - b^2 = (a-b)(a+b)$ | — |
| Sum/diff of cubes | $a^3 \pm b^3 = (a \pm b)(a^2 \mp ab + b^2)$ | — |

**Root types by $\Delta$:** $\Delta > 0$ → 2 distinct real; $\Delta = 0$ → 1 repeated; $\Delta < 0$ → 2 complex conjugates.

## Derivation

Remainder Theorem: write $P(x) = (x-a)Q(x) + R$ (division algorithm, remainder has degree < 1), substitute $x = a$: $P(a) = 0 \cdot Q(a) + R = R$. Factor Theorem is the case $R = 0$. Vieta's formulas come from expanding $a(x-\alpha)(x-\beta) = ax^2 - a(\alpha+\beta)x + a\alpha\beta$ and matching coefficients. [Full derivations: 04-Polynomials-Proofs]


**Finding roots of a cubic/quartic:**
1. Use the Factor Theorem: test the divisors of the constant term ($x = \pm 1, \pm 2, \dots$) until $P(a) = 0$.
2. Divide by $(x-a)$ — synthetic division is fastest.
3. Solve the reduced quadratic (factor, formula, or discriminant).

**Symmetric expressions in roots:** convert to Vieta combinations — e.g. $\alpha^3 + \beta^3 = (\alpha+\beta)^3 - 3\alpha\beta(\alpha+\beta)$.

**Polynomial inequalities:**
1. Factor completely.
2. Mark critical points (roots) on a number line.
3. Test the sign in each interval (sign chart).
4. Even multiplicity → sign unchanged; odd → sign flips.

## Worked Examples

**Setup:** Find all roots of $2x^3 - 3x^2 - 11x + 6 = 0$.

**Solution:** Try divisors of 6: $P(3) = 54 - 27 - 33 + 6 = 0$, so $x = 3$ is a root. Synthetic division by 3 gives $2x^2 + 3x - 2 = (2x-1)(x+2)$. Roots: $x = 3,\ \tfrac12,\ -2$.

**Key insight:** Use the Factor Theorem to find one root, then divide to reduce the degree.

---

**Setup:** If $\alpha, \beta$ are roots of $2x^2 - 5x + 3 = 0$, find $\alpha^3 + \beta^3$.

**Solution:** Vieta: $\alpha + \beta = \tfrac52$, $\alpha\beta = \tfrac32$. Identity: $\alpha^3 + \beta^3 = (\alpha+\beta)^3 - 3\alpha\beta(\alpha+\beta) = \tfrac{125}{8} - \tfrac{45}{4} = \tfrac{35}{8}$.

**Key insight:** Vieta's formulas compute symmetric expressions directly from coefficients — no solving needed.

---

**Setup:** Solve $x^3 - 6x^2 + 11x - 6 > 0$.

**Solution:** Factor: $(x-1)(x-2)(x-3) > 0$. Critical points $1, 2, 3$; sign chart gives $(1,2) \cup (3,\infty)$.

**Key insight:** Factor completely, find critical points, test signs in each interval.

## Common Traps

- $\Delta = 0$ means a **repeated** root, not "no roots"
- Complex roots of real polynomials come in conjugate pairs
- Forgetting the leading coefficient in Vieta's formulas (divide by $a$ first)
- Losing a root when a factor is repeated — multiplicity matters for inequalities
- Assuming every cubic has an integer root — if the rational-root test fails, use other methods

## Connections

- 02-Indices — exponent arithmetic in expansion
- 05-Partial-Fractions — factoring $Q(x)$ is the first step
- 04.2-Differentiation — polynomial rules
- 04.7-Series-Expansions — Taylor polynomials
- 02.4-Parabola — quadratic geometry
