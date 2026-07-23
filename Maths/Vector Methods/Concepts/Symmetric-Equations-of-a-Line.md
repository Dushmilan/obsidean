---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-5-lines-planes, symmetric-equations-of-a-line]
parent: [[Vector Methods/12.5-Equations-of-Lines-and-Planes.md]]
---

# Symmetric Equations of a Line

> **Stewart Calculus, Chapter 12, Section 12.5**

## Definition
Eliminating $t$ from the parametric equations $x = x_0 + at$, $y = y_0 + bt$, $z = z_0 + ct$ yields:
$$\frac{x - x_0}{a} = \frac{y - y_0}{b} = \frac{z - z_0}{c}$$
Requires all direction components $a, b, c$ to be nonzero.

## Key Properties
- If one component is zero (e.g., $c = 0$), that equation becomes $z = z_0$ — the line lies in that plane
- Two equalities express the same constraint as the parametric form
- Useful for checking whether two lines intersect
- Symmetric form makes the direction ratios visible at a glance

## Worked Example
From $x = 1 + 2t$, $y = 3 - t$, $z = 5 + 3t$:
$$\frac{x - 1}{2} = \frac{y - 3}{-1} = \frac{z - 5}{3}$$
The direction ratios are $2 : -1 : 3$.

If instead $z = 5$ (constant), the symmetric form would be $\frac{x-1}{2} = \frac{y-3}{-1}$, $z = 5$.

## Related Concepts
- [[Vector-Equation-of-a-Line]]
- [[Parametric-Equations-of-a-Line]]

---

*Part of [[Vector Methods/12.5-Equations-of-Lines-and-Planes.md|12.5 Equations of Lines and Planes]]*
