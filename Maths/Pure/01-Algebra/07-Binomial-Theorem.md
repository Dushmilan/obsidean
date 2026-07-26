# 1.7 Binomial Theorem

The Binomial Theorem gives a formula for expanding $(x+y)^n$ as a sum of terms with binomial coefficients. For integer exponents, it produces a finite expansion with $n+1$ terms. For rational (fractional) exponents, it produces an infinite series that converges when $|x| < 1$. The binomial coefficients $\binom{n}{r}$ appear everywhere — in Pascal's Triangle, in combinatorics, in probability distributions, and in the expansion of polynomials. This theorem connects counting to algebra.

**The Intuition:** When you expand $(x+y)^n$, you are choosing for each of the $n$ brackets whether to take $x$ or $y$. The term $\binom{n}{r} x^{n-r} y^r$ counts the number of ways to choose $y$ from exactly $r$ of the $n$ brackets. Imagine $n$ boxes, each containing either $x$ or $y$ — the coefficient $\binom{n}{r}$ counts configurations with exactly $r$ copies of $y$.

**The Math:**

- **For $n \in \mathbb{N}$:** $(x+y)^n = \sum_{r=0}^n \binom{n}{r} x^{n-r} y^r$ (finite, valid for all $x, y$)
- **For $|x| < 1$ and $n \in \mathbb{Q}$:** $(1+x)^n = \sum_{r=0}^\infty \binom{n}{r} x^r$ where $\binom{n}{r} = \frac{n(n-1)\cdots(n-r+1)}{r!}$
- **General term (integer):** $T_{r+1} = \binom{n}{r} x^{n-r} y^r$, $r = 0, 1, \ldots, n$
- **Sum of coefficients:** $(1+1)^n = 2^n$; **alternating sum:** $(1-1)^n = 0$
- **Pascal's identity:** $\binom{n}{r} = \binom{n-1}{r-1} + \binom{n-1}{r}$
- **Linear approximation:** $(1+x)^n \approx 1 + nx$ for small $x$

Critical: For rational $n$, $|x| < 1$ is required for convergence. Factor out constants first to get the standard $(1+u)^n$ form. The general term index starts at $r=0$, so the $k$th term uses $r = k-1$.

**What does this mean for Pure Mathematics?** The generalised binomial theorem enables expansion of expressions like $\sqrt{1+x}$, $\frac{1}{(1-x)^2}$, and $(1+x)^{-3}$, and is used extensively in calculus for approximations and power series.

### Example 1: Expand $(2-3x)^5$ up to $x^3$

**Setup:** Binomial with non-unit first term.

**Solution:** Factor out: $2^5(1 - \frac{3}{2}x)^5 = 32\left[1 + 5\left(-\frac{3}{2}x\right) + 10\left(-\frac{3}{2}x\right)^2 + 10\left(-\frac{3}{2}x\right)^3 + \cdots\right] = 32 - 240x + 720x^2 - 1080x^3 + \cdots$

**Key insight:** Factor out the constant to get the standard $(1+u)^n$ form.

### Example 2: Approximate $\sqrt{1.02}$

**Setup:** Square root close to 1.

**Solution:** $\sqrt{1.02} = (1+0.02)^{1/2} \approx 1 + \frac{1}{2}(0.02) - \frac{1}{8}(0.02)^2 = 1 + 0.01 - 0.00005 = 1.00995$

**Key insight:** Use the binomial expansion for small perturbations around 1.

### Example 3: Find coefficient of $x^4$ in $(2x - 3/x)^{10}$

**Setup:** Mixed powers of $x$.

**Solution:** General term: $T_{r+1} = \binom{10}{r}(2x)^{10-r}(-3/x)^r = \binom{10}{r} 2^{10-r} (-3)^r x^{10-2r}$. Set $10-2r = 4 \Rightarrow r = 3$. Coefficient: $\binom{10}{3} 2^7 (-3)^3 = 120 \times 128 \times (-27) = -414720$.

**Key insight:** Match the exponent of $x$ to find which term contributes, then compute.

---
