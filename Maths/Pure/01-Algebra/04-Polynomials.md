---
date: 2026-07-19
type: concept
tags: [maths, pure, a-level, algebra, polynomials]
parent: [[Pure/01-Algebra.md]]
proofs: [[Pure/Proofs/01-Algebra/04-Polynomials-Proofs.md]]
prerequisites: [[Pure/01-Algebra/01-Real-Numbers-Surds.md]]
---

# Polynomials

## Definitions
- **Polynomial in $x$:** $P(x) = a_n x^n + a_{n-1} x^{n-1} + \cdots + a_1 x + a_0$, $a_n \neq 0$
- **Degree:** $n$ (highest power with non-zero coefficient)
- **Zero polynomial:** $P(x) = 0$ (degree undefined or $-\infty$)
- **Root/Zero:** $\alpha$ such that $P(\alpha) = 0$

## Remainder Theorem
When $P(x)$ is divided by $(x-a)$, remainder is $P(a)$.

**Proof:** $P(x) = (x-a)Q(x) + R$, $R$ constant. Set $x=a$: $P(a) = R$.

## Factor Theorem
$(x-a)$ is a factor of $P(x)$ $\iff$ $P(a) = 0$.

**Corollary:** If $P(x)$ has integer coefficients and $a$ is an integer root, $a$ divides the constant term $a_0$.

## Factorisation Strategy
1. **Check integer roots** using factor theorem (divisors of constant term)
2. **Polynomial long division** or **synthetic division** to reduce degree
3. **Repeat** until quadratic or cubic
4. **Quadratic formula** for remaining quadratic

## Relationship Between Roots & Coefficients

For $ax^3 + bx^2 + cx + d = 0$ with roots $\alpha, \beta, \gamma$:
- Sum: $\alpha + \beta + \gamma = -\frac{b}{a}$
- Pairwise sum: $\alpha\beta + \beta\gamma + \gamma\alpha = \frac{c}{a}$
- Product: $\alpha\beta\gamma = -\frac{d}{a}$

For $ax^4 + bx^3 + cx^2 + dx + e = 0$ with roots $\alpha, \beta, \gamma, \delta$:
- $\sum\alpha = -b/a$
- $\sum\alpha\beta = c/a$
- $\sum\alpha\beta\gamma = -d/a$
- $\alpha\beta\gamma\delta = e/a$

## Symmetric Polynomials in Roots
Any symmetric expression in roots can be written in terms of elementary symmetric polynomials (coefficients).

**Common identities:**
- $\alpha^2 + \beta^2 + \gamma^2 = (\sum\alpha)^2 - 2\sum\alpha\beta$
- $\alpha^3 + \beta^3 + \gamma^3 = (\sum\alpha)^3 - 3\sum\alpha\sum\alpha\beta + 3\alpha\beta\gamma$
- $\alpha^2\beta + \alpha\beta^2 + \beta^2\gamma + \beta\gamma^2 + \gamma^2\alpha + \gamma\alpha^2 = \sum\alpha\sum\alpha\beta - 3\alpha\beta\gamma$

## Polynomial Inequalities

**Strategy:**
1. Factorise polynomial completely
2. Find roots (critical points)
3. Determine sign in each interval (sign chart or test points)
4. Consider multiplicity:
   - Odd multiplicity: sign changes
   - Even multiplicity: sign does not change

## Common Factorisations

| Form | Factorisation |
|------|---------------|
| $a^2 - b^2$ | $(a-b)(a+b)$ |
| $a^3 \pm b^3$ | $(a \pm b)(a^2 \mp ab + b^2)$ |
| $a^3 + b^3 + c^3 - 3abc$ | $(a+b+c)(a^2+b^2+c^2-ab-bc-ca)$ |
| $x^n - y^n$ | $(x-y)(x^{n-1}+x^{n-2}y+\cdots+y^{n-1})$ |
| $x^n + y^n$ ($n$ odd) | $(x+y)(x^{n-1}-x^{n-2}y+\cdots+y^{n-1})$ |

## Problem Patterns (A/L)

| Pattern | Approach |
|---------|----------|
| Find roots given factor | Substitute root $\to$ equation in coefficients |
| Common root of two polynomials | Eliminate $x$ or use Euclidean algorithm |
| Roots in AP/GP/HP | Use Vieta + pattern constraints |
| Form equation with transformed roots | If $\alpha$ root of $P(x)=0$, then $\alpha+k$ root of $P(x-k)=0$ etc. |
| Range of polynomial | Complete square or calculus (derivative) |

## Worked Examples

### Example 1: $2x^3 - 3x^2 - 11x + 6 = 0$ has an integer root. Find all roots.
Try divisors of 6: $\pm 1, \pm 2, \pm 3, \pm 6$
$P(3) = 54 - 27 - 33 + 6 = 0$ $\Rightarrow$ $x=3$ is root
Synthetic division by 3:
```
  2  -3  -11   6
3 |    6   9  -6
  2   3   -2   0
```
$2x^2 + 3x - 2 = 0 \Rightarrow (2x-1)(x+2) = 0 \Rightarrow x = \frac{1}{2}, -2$
Roots: $3, \frac{1}{2}, -2$

### Example 2: If $\alpha, \beta$ are roots of $2x^2 - 5x + 3 = 0$, find $\alpha^3 + \beta^3$.
$\alpha+\beta = 5/2$, $\alpha\beta = 3/2$
$\alpha^3+\beta^3 = (\alpha+\beta)^3 - 3\alpha\beta(\alpha+\beta) = (5/2)^3 - 3(3/2)(5/2) = 125/8 - 45/4 = 125/8 - 90/8 = 35/8$

### Example 3: Solve $x^3 - 6x^2 + 11x - 6 > 0$
Factor: $(x-1)(x-2)(x-3) > 0$
Sign chart:
- $x < 1$: $(-)(-)(-) = -$ 
- $1 < x < 2$: $(+)(-)(-) = +$
- $2 < x < 3$: $(+)(+)(-) = -$
- $x > 3$: $(+)(+)(+) = +$
Solution: $(1,2) \cup (3,\infty)$

## Cross-References
- [[Pure/01-Algebra/05-Partial-Fractions.md]] — needs factorised denominator
- [[Pure/02-Analytical-Geometry/]] — conic equations are polynomials
- [[Pure/04-Calculus/02-Differentiation-Rules.md]] — polynomials differentiable everywhere
- [[Physics/02-Mechanics/01-Kinematics.md]] — displacement polynomials

## Common Traps
- ❌ Forgetting to check multiplicity for inequalities
- ❌ Synthetic division errors (sign errors)
- ❌ Not checking if found root actually works
- ❌ Confusing $P(a)=0$ (factor) with $P(a)=R$ (remainder)
- ❌ Missing complex conjugate roots (coefficients real $\Rightarrow$ roots in conjugate pairs)

## Quick Reference
**Remainder Theorem:** $P(x) = (x-a)Q(x) + P(a)$
**Factor Theorem:** $(x-a) \mid P(x) \iff P(a)=0$
**Vieta's (quadratic $ax^2+bx+c=0$):** $\sum\alpha = -b/a$, $\alpha\beta = c/a$
**Vieta's (cubic):** $\sum\alpha = -b/a$, $\sum\alpha\beta = c/a$, $\alpha\beta\gamma = -d/a$