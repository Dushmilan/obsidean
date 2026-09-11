
## Definition

A **surd** is an irrational root of a rational number:

$$\sqrt[n]{a} \quad \text{where} \quad a \in \mathbb{Q},\ n \in \mathbb{N},\ n \ge 2,\ \text{and}\ \sqrt[n]{a} \notin \mathbb{Q}$$

Surds are the exact bookmarks for irrational values on the real number line — $\sqrt{2}, \sqrt{3}, \sqrt{5}$ are the standard examples. Critical: $\sqrt{x^2} = |x|$, never $x$ — the principal root is always non-negative.

## The Intuition

A surd is like a fraction that can never fully simplify. $\sqrt{2}$ is the number that, when multiplied by itself, gives exactly 2 — but no fraction of integers can achieve that. On the number line it sits just past 1.4, permanently stuck between any two fractions you try to pin it with. Surds are how we write those "stuck" values exactly.

## The Toolkit

| Result | Formula | Valid when |
|--------|---------|-----------|
| Product law | $\sqrt[n]{a} \cdot \sqrt[n]{b} = \sqrt[n]{ab}$ | same index $n$ only |
| Quotient law | $\dfrac{\sqrt[n]{a}}{\sqrt[n]{b}} = \sqrt[n]{\dfrac{a}{b}}$ | $b \neq 0$ |
| Power law | $(\sqrt[n]{a})^m = a^{m/n}$ | $a \ge 0$ if $n$ even |
| Rationalise (simple) | $\dfrac{1}{\sqrt{a}} = \dfrac{\sqrt{a}}{a}$ | $a > 0$ |
| Rationalise (sum) | $\dfrac{1}{\sqrt{a}+\sqrt{b}} = \dfrac{\sqrt{a}-\sqrt{b}}{a-b}$ | $a, b \ge 0$, $a \neq b$ |
| Nested surd | $\sqrt{a \pm 2\sqrt{b}} = \sqrt{x} \pm \sqrt{y}$, $x+y=a$, $xy=b$ | $x \ge y \ge 0$ |
| Principal root | $\sqrt{x^2} = |x|$ | all real $x$ |

## Derivation

Rationalising a denominator uses the difference of squares: $(\sqrt{a}+\sqrt{b})(\sqrt{a}-\sqrt{b}) = a - b$, which is rational. The nested-surd formula comes from squaring: $(\sqrt{x}\pm\sqrt{y})^2 = x + y \pm 2\sqrt{xy} = a \pm 2\sqrt{b}$. [Full derivations: 01-Real-Numbers-Surds-Proofs]

## Method

**Rationalising** $\frac{p + q\sqrt{a}}{r + s\sqrt{b}}$:
1. Multiply numerator and denominator by the conjugate of the denominator.
2. Expand the denominator as a difference of squares (becomes rational).
3. Simplify and collect like surd terms.

**Solving surd equations**:
1. State the domain (each radicand $\ge 0$).
2. Isolate one radical, square both sides, simplify.
3. Repeat until no radicals remain.
4. **Check every candidate** — squaring introduces extraneous solutions.

**Denesting** $\sqrt{a \pm 2\sqrt{b}}$:
1. Solve $x + y = a$, $xy = b$ (a quadratic).
2. Write $\sqrt{x} \pm \sqrt{y}$.

## Worked Examples

**Setup:** Rationalise $\frac{\sqrt{5}+\sqrt{3}}{\sqrt{5}-\sqrt{3}}$.

**Solution:** Multiply by conjugate $\frac{\sqrt{5}+\sqrt{3}}{\sqrt{5}+\sqrt{3}}$:
$$\frac{(\sqrt{5}+\sqrt{3})^2}{(\sqrt{5})^2-(\sqrt{3})^2} = \frac{5 + 2\sqrt{15} + 3}{5-3} = \frac{8+2\sqrt{15}}{2} = 4 + \sqrt{15}$$

**Key insight:** Multiply by the conjugate to create a difference of squares in the denominator.

---

**Setup:** Solve $\sqrt{x+3} + \sqrt{x-1} = 2$.

**Solution:** Domain: $x \ge 1$. Isolate: $\sqrt{x+3} = 2 - \sqrt{x-1}$. Square: $x+3 = 4 + (x-1) - 4\sqrt{x-1}$, so $4\sqrt{x-1} = 0 \Rightarrow x = 1$. Check: $\sqrt{4} + \sqrt{0} = 2$ — valid.

**Key insight:** After isolating and squaring, the equation simplifies dramatically. Always check the solution.

---

**Setup:** Denest $\sqrt{11 - 6\sqrt{2}}$.

**Solution:** Match the pattern $\sqrt{a - 2\sqrt{b}}$: $a = 11$, $2\sqrt{b} = 6\sqrt{2} \Rightarrow b = 18$. Find $x, y$ with $x + y = 11$, $xy = 18$: $x = 9$, $y = 2$. Thus $\sqrt{11-6\sqrt{2}} = \sqrt{9} - \sqrt{2} = 3 - \sqrt{2}$.

**Key insight:** Match the nested surd to the pattern, then solve the quadratic for $x, y$.

## Common Traps

- $\sqrt{x^2} = |x|$, not $x$ — the principal root is never negative
- Product law requires the **same index** — $\sqrt{2} \cdot \sqrt[3]{2}$ can't combine directly
- Squaring surd equations creates extraneous roots — always verify
- Denominator must be fully rationalised (no surd left downstairs)
- $\sqrt{a \pm \sqrt{b}}$ (not $2\sqrt{b}$) needs a different technique — check the pattern first

## Connections

- 02-Indices — $a^{m/n} = \sqrt[n]{a^m}$ is the surd in exponent form
- 03-Logarithms — logs of surd arguments
- 04.1-Limits-Continuity — rationalising limits
- 02.1-Cartesian-Coordinates-Distance — exact distances
- 03.1-Trigonometric-Functions-Identities — exact values of special angles
