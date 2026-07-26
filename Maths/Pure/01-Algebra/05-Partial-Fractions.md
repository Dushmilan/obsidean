# 1.5 Partial Fractions

Partial fractions decompose a rational function $\frac{P(x)}{Q(x)}$ into a sum of simpler fractions. Think of it as "un-mixing" — if you have a combined fraction, partial fractions separates it into individual components that are each easy to work with. The technique is indispensable in calculus: integrating $\frac{5x+1}{(x-1)(x+2)}$ directly is hard; integrating $\frac{2}{x-1} + \frac{3}{x+2}$ is trivial. The key requirement is that the fraction must be proper ($\deg(P) < \deg(Q)$); if not, perform polynomial long division first.

**The Intuition:** Every polynomial can be factorised into linear and irreducible quadratic factors over the reals. Once factorised, the algebraic identity equating the original fraction to the sum of partial fractions must hold for ALL values of $x$. This gives enough equations to solve for all constants.

**The Math:** Decompose $\frac{P(x)}{Q(x)}$ based on the factors of $Q(x)$:

- **Linear factor** $(ax+b)$: contributes $\frac{A}{ax+b}$
- **Repeated linear** $(ax+b)^n$: contributes $\frac{A_1}{ax+b} + \cdots + \frac{A_n}{(ax+b)^n}$
- **Irreducible quadratic** $(ax^2+bx+c)$ with $b^2-4ac < 0$: contributes $\frac{Ax+B}{ax^2+bx+c}$
- **Cover-up formula:** For distinct linear $(x-a)$, $A = \frac{P(a)}{Q'(a)}$ where $Q(x) = (x-a)R(x)$

**What does this mean for Pure Mathematics?** Partial fractions are the essential first step for integrating rational functions, expanding binomial series, and solving certain differential equations. After finding constants, always verify by recombining the fractions — it takes 30 seconds and catches most errors.

### Example 1: Decompose $\frac{5x+1}{(x-1)(x+2)}$

**Setup:** Distinct linear factors.

**Solution:** Form: $\frac{A}{x-1} + \frac{B}{x+2}$. Cover-up for $A$: $A = \frac{5(1)+1}{1+2} = 2$. Cover-up for $B$: $B = \frac{5(-2)+1}{-2-1} = 3$. Result: $\frac{2}{x-1} + \frac{3}{x+2}$.

**Key insight:** Cover-up is the fastest method for distinct linear factors.

### Example 2: Decompose $\frac{3x^2+5x+2}{(x+1)^2(x-2)}$

**Setup:** Repeated linear factor.

**Solution:** Form: $\frac{A}{x+1} + \frac{B}{(x+1)^2} + \frac{C}{x-2}$. Set $x = -1$: $B = 0$. Set $x = 2$: $C = \frac{8}{3}$. Set $x = 0$: $A = \frac{1}{3}$. Result: $\frac{1}{3(x+1)} + \frac{8}{3(x-2)}$.

**Key insight:** For repeated factors, find the highest power constant first using cover-up, then equate coefficients for the rest.

### Example 3: Decompose $\frac{2x^3+3x^2+4x+5}{(x^2+1)^2}$

**Setup:** Repeated irreducible quadratic.

**Solution:** Form: $\frac{Ax+B}{x^2+1} + \frac{Cx+D}{(x^2+1)^2}$. Multiply and equate: $A=2$, $B=3$, $C=2$, $D=2$. Result: $\frac{2x+3}{x^2+1} + \frac{2x+2}{(x^2+1)^2}$.

**Key insight:** For irreducible quadratics, equate coefficients systematically by comparing powers.

---
