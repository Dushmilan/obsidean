# 1.1 Real Numbers & Surds

A surd is an irrational root of a rational number, written as $\sqrt[n]{a}$. They are the precise bookmarks for irrational values on the real number line — $\sqrt{2}$, $\sqrt{3}$, $\sqrt{5}$ — and the three core operations (product law, quotient law, rationalisation) form the backbone of algebraic simplification. Without surds, we could not express exact values, and without rationalising denominators, we could not compute limits in calculus or distances in geometry.

**The Intuition:** Think of a surd like a fraction that can never fully simplify. $\sqrt{2}$ is the number that, when multiplied by itself, gives exactly 2 — but no fraction of integers can achieve that. On the number line, $\sqrt{2}$ sits just past 1.4, permanently stuck between any two fractions you try to pin it with.

**The Math:** A **surd** is $\sqrt[n]{a}$ where $a \in \mathbb{Q}$, $n \in \mathbb{N}$, $n \ge 2$, and $\sqrt[n]{a} \notin \mathbb{Q}$. The core laws:

- **Product law:** $\sqrt[n]{a} \cdot \sqrt[n]{b} = \sqrt[n]{ab}$ (same index only)
- **Quotient law:** $\frac{\sqrt[n]{a}}{\sqrt[n]{b}} = \sqrt[n]{\frac{a}{b}}$
- **Power law:** $(\sqrt[n]{a})^m = a^{m/n}$
- **Rationalise** $\frac{1}{\sqrt{a}}$: multiply by $\frac{\sqrt{a}}{\sqrt{a}}$ to get $\frac{\sqrt{a}}{a}$
- **Rationalise** $\frac{1}{\sqrt{a}+\sqrt{b}}$: multiply by conjugate to get $\frac{\sqrt{a}-\sqrt{b}}{a-b}$
- **Nested surd:** $\sqrt{a \pm 2\sqrt{b}} = \sqrt{x} \pm \sqrt{y}$ where $x+y=a$, $xy=b$
- **Critical:** $\sqrt{x^2} = |x|$, not $x$. The principal root is always non-negative.

**What does this mean for Pure Mathematics?** Surds are essential for calculus (rationalising limits), geometry (exact distances via the distance formula), and trigonometry (exact values of special angles). Nested surds and surd equations extend this further, requiring pattern recognition and the discipline to check for extraneous solutions introduced by squaring.

### Example 1: Rationalise $\frac{\sqrt{5}+\sqrt{3}}{\sqrt{5}-\sqrt{3}}$

**Setup:** A fraction with surd denominator.

**Solution:** Multiply numerator and denominator by conjugate $\sqrt{5}+\sqrt{3}$:
$\frac{(\sqrt{5}+\sqrt{3})^2}{(\sqrt{5})^2-(\sqrt{3})^2} = \frac{5 + 2\sqrt{15} + 3}{5-3} = \frac{8 + 2\sqrt{15}}{2} = 4 + \sqrt{15}$

**Key insight:** Multiply by the conjugate to create a difference of squares in the denominator.

### Example 2: Solve $\sqrt{x+3} + \sqrt{x-1} = 2$

**Setup:** A surd equation with two radical terms.

**Solution:** Domain: $x \ge 1$. Isolate: $\sqrt{x+3} = 2 - \sqrt{x-1}$. Square both sides: $x+3 = 4 + (x-1) - 4\sqrt{x-1}$. Simplify: $4\sqrt{x-1} = 0 \Rightarrow x = 1$. Check: $\sqrt{4} + \sqrt{0} = 2$ (valid).

**Key insight:** After isolating and squaring, the equation simplifies dramatically. Always check the solution.

### Example 3: Denest $\sqrt{11-6\sqrt{2}}$

**Setup:** A nested surd of the form $\sqrt{a - 2\sqrt{b}}$.

**Solution:** Identify $a = 11$, $2\sqrt{b} = 6\sqrt{2}$ so $b = 18$. Find $x, y$ with $x+y=11$, $xy=18$: $x=9$, $y=2$. $\sqrt{11 - 6\sqrt{2}} = \sqrt{9} - \sqrt{2} = 3 - \sqrt{2}$

**Key insight:** Match the nested surd to the pattern $\sqrt{a \pm 2\sqrt{b}}$, then solve the quadratic.

---
