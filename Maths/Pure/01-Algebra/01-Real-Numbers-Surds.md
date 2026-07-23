---
date: 2026-07-19
type: concept
tags: [maths, pure, a-level, algebra, surds, real-numbers]
parent: [[01-Algebra]]
proofs: [[Pure/Proofs/01-Algebra/01-Real-Numbers-Surds-Proofs]]
---

# 1.1 Real Numbers & Surds

## Definition
A **surd** is an irrational number of the form $\sqrt[n]{a}$ where $a \in \mathbb{Q}$, $n \in \mathbb{N}$, $n \ge 2$, and $\sqrt[n]{a} \notin \mathbb{Q}$.

**Pure surd:** $\sqrt[n]{a}$ (no rational factor)
**Mixed surd:** $b\sqrt[n]{a}$ ($b \in \mathbb{Q}$)
**Like surds:** Same order and radicand (e.g., $3\sqrt{2}, 5\sqrt{2}$)
**Unlike surds:** Different radicands or orders

The real number system: $\mathbb{N} \subset \mathbb{Z} \subset \mathbb{Q} \subset \mathbb{R}$, where $\mathbb{R} = \mathbb{Q} \cup \mathbb{Q}'$ (rationals + irrationals).

## Formulae

| Formula | Expression |
|---------|------------|
| Product law | $\sqrt[n]{a} \cdot \sqrt[n]{b} = \sqrt[n]{ab}$ |
| Quotient law | $\frac{\sqrt[n]{a}}{\sqrt[n]{b}} = \sqrt[n]{\frac{a}{b}}$ |
| Power law | $(\sqrt[n]{a})^m = \sqrt[n]{a^m} = a^{m/n}$ |
| Change of order | $\sqrt[m]{\sqrt[n]{a}} = \sqrt[mn]{a}$ |
| Rationalise $\frac{1}{\sqrt{a}}$ | $\frac{\sqrt{a}}{a}$ |
| Rationalise $\frac{1}{\sqrt{a}+\sqrt{b}}$ | $\frac{\sqrt{a}-\sqrt{b}}{a-b}$ |
| Nested surd | $\sqrt{a + 2\sqrt{b}} = \sqrt{x} + \sqrt{y}$ where $x+y=a$, $xy=b$ |
| Nested surd | $\sqrt{a - 2\sqrt{b}} = \sqrt{x} - \sqrt{y}$ where $x+y=a$, $xy=b$ |
| Absolute value | $\|x\| = \begin{cases} x & x \ge 0 \\ -x & x < 0 \end{cases}$ |

**Critical:** $\sqrt[n]{a} \cdot \sqrt[m]{b} \neq \sqrt[nm]{ab}$ unless orders match!

## Properties / Key Concepts

### Real Number System
- **Density:** Between any two reals, there exists a rational and an irrational
- **Completeness:** Every non-empty set bounded above has a supremum (least upper bound)

### Simplifying Surds
1. **Reduce radicand:** Extract perfect powers
   $\sqrt{72} = \sqrt{36 \cdot 2} = 6\sqrt{2}$
   $\sqrt[3]{54} = \sqrt[3]{27 \cdot 2} = 3\sqrt[3]{2}$

2. **Rationalise denominator:** Multiply by conjugate

3. **Equalise orders (for unlike surds):**
   $\sqrt{2} \cdot \sqrt[3]{2} = 2^{1/2} \cdot 2^{1/3} = 2^{5/6} = \sqrt[6]{32}$

### Nested Surds
**Form:** $\sqrt{a \pm 2\sqrt{b}}$. Find $x, y$ such that $x+y=a$, $xy=b$.
**Condition:** $a^2 - 4b \ge 0$ for real $x, y$.

### Surd Equations
1. Isolate one surd term
2. Square both sides
3. Repeat until all surds eliminated
4. **Check for extraneous roots** (squaring introduces spurious solutions)

### Absolute Value Properties
- $|x| \ge 0$, $|x| = 0 \iff x = 0$
- $|xy| = |x||y|$
- $|x+y| \le |x| + |y|$ (Triangle inequality)
- $\sqrt{x^2} = |x|$ (NOT $x$ unless $x \ge 0$)

### Absolute Value Inequalities
- $|x| < a \iff -a < x < a$ ($a > 0$)
- $|x| > a \iff x < -a \text{ or } x > a$ ($a > 0$)
- $|x-a| < b \iff a-b < x < a+b$ ($b > 0$)

### Rational & Irrational Numbers
- **Rational:** $\frac{p}{q}$, $p,q \in \mathbb{Z}, q \neq 0$. Decimal terminates or repeats.
- **Irrational:** Not rational. Decimal non-terminating, non-repeating.
- **Surd as irrational:** If $a$ is not a perfect $n$th power, $\sqrt[n]{a}$ is irrational.

### Problem Patterns

| Pattern | Strategy |
|---------|----------|
| Simplify surd expression | Reduce radicands, combine like surds |
| Rationalise denominator | Multiply by conjugate |
| Compare surds | Square/cube to compare without calculator |
| Solve surd equation | Isolate, square, check extraneous |
| Solve absolute value equation | Split cases: $x \ge 0$ and $x < 0$ |
| Solve absolute value inequality | Use interval definition or graph |
| Nested surd $\sqrt{a \pm 2\sqrt{b}}$ | Find $x,y$: sum $a$, product $b$ |

## Worked Examples

### Example 1: Simplify $\frac{\sqrt{5}+\sqrt{3}}{\sqrt{5}-\sqrt{3}}$
Multiply by conjugate:
$\frac{(\sqrt{5}+\sqrt{3})^2}{5-3} = \frac{5+3+2\sqrt{15}}{2} = 4+\sqrt{15}$

### Example 2: Solve $\sqrt{x+3} + \sqrt{x-1} = 2$
Domain: $x \ge 1$
$\sqrt{x+3} = 2 - \sqrt{x-1}$
Square: $x+3 = 4 + (x-1) - 4\sqrt{x-1} = x+3 - 4\sqrt{x-1}$
$4\sqrt{x-1} = 0 \Rightarrow x = 1$
Check: $\sqrt{4} + \sqrt{0} = 2$ ✓

### Example 3: $\sqrt{11-6\sqrt{2}} = ?$
$x+y=11$, $xy=18 \Rightarrow x=9, y=2$ (or $x=2, y=9$)
$\sqrt{11-6\sqrt{2}} = \sqrt{9} - \sqrt{2} = 3-\sqrt{2}$ (take positive root)

### Example 4: Solve $|2x-5| < 3$
$-3 < 2x-5 < 3 \Rightarrow 2 < 2x < 8 \Rightarrow 1 < x < 4$

### Example 5: Compare $\sqrt{5}+\sqrt{7}$ vs $\sqrt{6}+\sqrt{6}$
Square both:
$(\sqrt{5}+\sqrt{7})^2 = 12 + 2\sqrt{35} \approx 12 + 11.83 = 23.83$
$(2\sqrt{6})^2 = 24$
$\sqrt{5}+\sqrt{7} < 2\sqrt{6}$

## Common Traps
- ❌ $\sqrt{a+b} = \sqrt{a} + \sqrt{b}$
- ❌ $\sqrt{x^2} = x$ (correct: $|x|$)
- ❌ Not checking extraneous roots after squaring
- ❌ Forgetting domain restrictions ($x \ge 0$ for $\sqrt{x}$)
- ❌ $\frac{1}{\sqrt{a}} = \frac{1}{\sqrt{a}}$ not rationalised (should be $\frac{\sqrt{a}}{a}$)
- ❌ Not simplifying fully (e.g., $\sqrt{50} = 5\sqrt{2}$, not left as $\sqrt{50}$)

## Cross-References
- [[Pure/01-Algebra/02-Indices|Indices]] — $a^{1/n} = \sqrt[n]{a}$
- [[Pure/01-Algebra/03-Logarithms|Logarithms]] — log of surds
- [[Pure/02-Analytical-Geometry/02-Straight-Lines|Straight Lines]] — distance formula involves $\sqrt{\cdot}$
- [[Pure/04-Calculus/01-Limits-Continuity|Limits & Continuity]] — limits with surds (rationalise)
- [[Physics/02-Mechanics/01-Kinematics|Kinematics]] — distance, velocity bounds

## Quick Reference
**Surd laws:** $\sqrt[n]{a}\sqrt[n]{b}=\sqrt[n]{ab}$, $\frac{\sqrt[n]{a}}{\sqrt[n]{b}}=\sqrt[n]{\frac{a}{b}}$, $(\sqrt[n]{a})^m=a^{m/n}$
**Rationalise:** $\frac{1}{\sqrt{a}\pm\sqrt{b}} = \frac{\sqrt{a}\mp\sqrt{b}}{a-b}$
**Nested:** $\sqrt{a+2\sqrt{b}}=\sqrt{x}+\sqrt{y}$ where $x+y=a, xy=b$
**Absolute value:** $|x| < a \iff -a < x < a$, $|x| > a \iff x < -a \text{ or } x > a$
**Check:** Always verify solutions to surd equations!

---

*Status: Done*