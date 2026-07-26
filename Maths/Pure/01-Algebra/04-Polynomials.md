# 1.4 Polynomials

A polynomial is an expression of the form $a_n x^n + a_{n-1} x^{n-1} + \cdots + a_1 x + a_0$ where the powers are whole numbers. They are the simplest "smooth" functions you can write down — differentiable everywhere, factorable completely over the complex numbers, and their roots encode fundamental information about the systems they model. The Factor Theorem and Remainder Theorem give you direct access to roots and remainders without performing long division.

**The Intuition:** A polynomial is like a recipe with ingredients at different "levels" (powers). The Factor Theorem says: if you plug in $x = a$ and get zero, then $(x-a)$ is a perfect ingredient that can be factored out. When you divide $P(x)$ by $(x-a)$, you get $P(x) = (x-a)Q(x) + R$. Setting $x = a$ kills the $(x-a)$ term, leaving $P(a) = R$.

**The Math:** A **polynomial in $x$:** $P(x) = a_n x^n + a_{n-1} x^{n-1} + \cdots + a_0$, $a_n \neq 0$.

- **Remainder Theorem:** $P(x) = (x-a)Q(x) + R$, so $P(a) = R$
- **Factor Theorem:** $(x-a) \mid P(x) \iff P(a) = 0$
- **Vieta's (quadratic):** $\alpha+\beta = -\frac{b}{a}$, $\alpha\beta = \frac{c}{a}$
- **Vieta's (cubic):** $\sum\alpha = -\frac{b}{a}$, $\sum\alpha\beta = \frac{c}{a}$, $\alpha\beta\gamma = -\frac{d}{a}$
- **Factorisations:** $a^2 - b^2 = (a-b)(a+b)$; $a^3 \pm b^3 = (a \pm b)(a^2 \mp ab + b^2)$
- **Discriminant:** $\Delta = b^2-4ac$ determines root type ($\Delta > 0$: 2 real; $\Delta = 0$: 1 repeated; $\Delta < 0$: 2 complex)

**What does this mean for Pure Mathematics?** Vieta's formulas connect roots to coefficients — a powerful tool for forming equations with prescribed roots or finding symmetric expressions without solving. Polynomial inequalities require a systematic approach: factor completely, find critical points, test signs in each interval. The multiplicity of a root determines whether the sign changes or stays the same.

### Example 1: Find all roots of $2x^3 - 3x^2 - 11x + 6 = 0$

**Setup:** Cubic with at least one integer root.

**Solution:** Try divisors of 6: $P(3) = 54 - 27 - 33 + 6 = 0$ — so $x = 3$ is a root. Synthetic division by 3 gives quotient $2x^2 + 3x - 2 = (2x-1)(x+2)$. Roots: $x = 3$, $x = \frac{1}{2}$, $x = -2$.

**Key insight:** Use the Factor Theorem to find one root, then divide to reduce the degree.

### Example 2: If $\alpha, \beta$ are roots of $2x^2 - 5x + 3 = 0$, find $\alpha^3 + \beta^3$

**Setup:** Symmetric expression in roots.

**Solution:** Vieta's: $\alpha+\beta = \frac{5}{2}$, $\alpha\beta = \frac{3}{2}$. Identity: $\alpha^3+\beta^3 = (\alpha+\beta)^3 - 3\alpha\beta(\alpha+\beta) = \frac{125}{8} - \frac{45}{4} = \frac{35}{8}$.

**Key insight:** Vieta's formulas let you compute symmetric expressions directly from coefficients.

### Example 3: Solve $x^3 - 6x^2 + 11x - 6 > 0$

**Setup:** Polynomial inequality.

**Solution:** Factor: $(x-1)(x-2)(x-3) > 0$. Critical points: $1, 2, 3$. Sign chart gives solution $(1,2) \cup (3,\infty)$.

**Key insight:** Factor completely, find critical points, test signs in each interval.

---
