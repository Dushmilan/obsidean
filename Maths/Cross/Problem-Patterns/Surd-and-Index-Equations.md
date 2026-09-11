---
date: 2026-08-16
type: problem-pattern
tags: [maths, pure, patterns, algebra]
parent: [[01-Algebra_Index]]
---

# Surd & Index Equations — Problem Patterns

Equations with radicals or powers follow one recipe: isolate the awkward term, invert the operation, then verify — because squaring (or raising to any power) can create fake solutions that don't satisfy the original equation.

## Pattern 1: Single surd term

**When you see:** one $\sqrt{f(x)}$ (or a cube root) plus ordinary terms.

**Template:**
1. Isolate the surd on one side.
2. Raise both sides to the index (square for square roots).
3. Solve the resulting polynomial equation.
4. **Always** substitute back — squaring can introduce extraneous roots.

**Example:** Solve $\sqrt{2x+5} = x - 5$.

**Setup:** A single square root.

**Solution:** Square: $2x+5 = (x-5)^2 = x^2 - 10x + 25$. Rearrange: $x^2 - 12x + 20 = 0$, so $x = 2$ or $x = 10$. Check $x=2$: $\sqrt{9} = -3$? No. Check $x=10$: $\sqrt{25} = 5$ ✓.

**Key insight:** The discarded root fails because $x-5$ must be non-negative for the right-hand side to match the non-negative principal root. Domain-check first.

## Pattern 2: Two surd terms

**Template:**
1. Move one surd to the other side.
2. Square, leaving one surd on one side.
3. Square again if needed.
4. Verify.

**Example:** Solve $\sqrt{x+3} + \sqrt{x-1} = 2$.

**Setup:** Two radicals.

**Solution:** Domain $x \ge 1$. Isolate: $\sqrt{x+3} = 2 - \sqrt{x-1}$. Square: $x+3 = 4 + x - 1 - 4\sqrt{x-1}$, i.e. $4\sqrt{x-1} = 0$, so $x = 1$. Check: $\sqrt{4} + \sqrt{0} = 2$ ✓.

**Key insight:** The cross-term $2\sqrt{a}\sqrt{b}$ is exactly what you solve for after the first squaring.

## Pattern 3: Exponential equations

**Template:**
- Same base: $a^{f(x)} = a^{g(x)} \implies f(x)=g(x)$.
- Different bases: take logs (any base).
- Equations quadratic in $a^x$: substitute $u = a^x$.

**Example:** Solve $4^x - 2^{x+1} - 8 = 0$.

**Setup:** Reducible to a quadratic in $2^x$.

**Solution:** $4^x = (2^x)^2$ and $2^{x+1} = 2\cdot 2^x$. Let $u = 2^x$: $u^2 - 2u - 8 = 0$, so $u = 4$ or $u = -2$. Since $u>0$, $u=4 \Rightarrow x=2$.

**Key insight:** Writing $4^x$ as $(2^x)^2$ exposes the quadratic; reject negative $u$ without hesitation.

## Common traps

- $\sqrt{x^2} = |x|$, not $x$.
- After any squaring, verify every candidate.
- $a^{f(x)}a^{g(x)} = a^{f(x)+g(x)}$, never $a^{f(x)g(x)}$.
