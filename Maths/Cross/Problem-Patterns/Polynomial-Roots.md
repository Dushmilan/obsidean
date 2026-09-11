---
date: 2026-08-16
type: problem-pattern
tags: [maths, pure, patterns, algebra]
parent: [[01-Algebra_Index]]
---

# Polynomial Roots — Problem Patterns

Most root problems reduce to two tools: the factor/remainder theorems for *finding* roots, and Vieta's relations for *using* roots without finding them.

## Pattern 1: Remainder & factor theorem

**Template:**
- Remainder when $P(x)$ is divided by $(x-a)$ is $P(a)$.
- $(x-a)$ is a factor iff $P(a)=0$.
- For a divisor $ax+b$, evaluate $P(-b/a)$.

**Example:** Given $x^3 - 3x^2 + kx + 4$ has $(x-1)$ as a factor, find $k$.

**Setup:** One unknown coefficient.

**Solution:** $P(1) = 1 - 3 + k + 4 = k+2 = 0$, so $k = -2$.

**Key insight:** The factor theorem turns the whole problem into one substitution.

## Pattern 2: Root-finding ladder

**Template:**
1. Try small integer divisors of the constant term (rational root test).
2. Divide the polynomial (synthetic or long division).
3. Repeat until quadratic, then solve.

**Example:** Solve $x^3 - 6x^2 + 11x - 6 = 0$.

**Setup:** Cubic with integer constant.

**Solution:** Try $x=1$: $1-6+11-6=0$ ✓. Divide by $(x-1)$: $x^2 - 5x + 6 = (x-2)(x-3)$. Roots: $1, 2, 3$.

**Key insight:** Every root found lowers the degree by one; you only need to "guess" one root by hand.

## Pattern 3: Vieta's relations

**Template:** For $ax^3 + bx^2 + cx + d = 0$ with roots $\alpha, \beta, \gamma$:
- $\alpha+\beta+\gamma = -\frac{b}{a}$
- $\alpha\beta+\beta\gamma+\gamma\alpha = \frac{c}{a}$
- $\alpha\beta\gamma = -\frac{d}{a}$

**Example:** Find $\alpha^2+\beta^2+\gamma^2$ for the roots of $x^3 - 2x^2 - 5x + 6 = 0$.

**Setup:** Symmetric function of the roots.

**Solution:** $s_1 = 2$, $s_2 = -5$. Then $\alpha^2+\beta^2+\gamma^2 = s_1^2 - 2s_2 = 4 + 10 = 14$.

**Key insight:** Any symmetric polynomial in the roots is expressible in the elementary symmetric sums — no need to find the roots at all.

## Pattern 4: Forming an equation with transformed roots

**Template:** If $\alpha,\beta,\gamma$ are roots of $P(x)=0$:
- roots $+c$: roots of $P(x-c)=0$
- roots $\times c$: roots of $P(x/c)=0$
- reciprocal roots: roots of $x^n P(1/x)=0$

**Example:** Form a cubic whose roots are the reciprocals of the roots of $x^3 - 6x^2 + 11x - 6 = 0$.

**Setup:** Reciprocal transform.

**Solution:** $x^3 \cdot P(1/x) = x^3\left(\frac{1}{x^3} - \frac{6}{x^2} + \frac{11}{x} - 6\right) = 1 - 6x + 11x^2 - 6x^3$. So $-6x^3 + 11x^2 - 6x + 1 = 0$.

**Key insight:** Reciprocal roots = reverse the coefficients.
