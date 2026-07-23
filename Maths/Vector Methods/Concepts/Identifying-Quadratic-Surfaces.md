---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-6-quadric-surfaces, identification]
parent: [[Vector Methods/12.6-Cylinders-and-Quadratic-Surfaces.md]]
---

# Identifying Quadratic Surfaces
> Stewart Calculus, Chapter 12, Section 12.6

## Definition
A **quadric surface** is the graph of a second-degree equation in $x, y, z$. After rotating and translating to standard position, the surface is identified by its sign pattern and structure.

## Key Properties — Identification Guide
| Sign pattern | Constant | Surface |
|---|---|---|
| All positive | $= 1$ | **Ellipsoid** |
| One negative | $= 1$ | **Hyperboloid of one sheet** (axis = negative term) |
| Two negatives | $= 1$ | **Hyperboloid of two sheets** (axis = positive term) |
| Mixed signs | $= 0$ | **Cone** |
| One linear term = sum of squares | — | **Paraboloid** |

**Quick recipe:**
1. Put the equation in standard form (complete the square if needed).
2. Count the minus signs on the squared terms.
3. Check if the right side is $1$ (hyperboloid/ellipsoid), $0$ (cone), or if one side is linear (paraboloid).

## Worked Example
**Equation:** $4x^2 - y^2 + 2z^2 = 1$

Rewrite: $\frac{x^2}{(1/2)^2} - \frac{y^2}{1^2} + \frac{z^2}{(1/\sqrt{2})^2} = 1$

- Squared terms: all three present.
- Signs: $+$, $-$, $+$ → **one minus sign**.
- Right side: $1$.

**Conclusion:** Hyperboloid of one sheet, with axis along the $y$-axis (the negative term).

## Related Concepts
- [[Maths/Vector Methods/Concepts/Traces-and-Cross-Sections|Traces and Cross-Sections]]
- [[Maths/Vector Methods/Concepts/Ellipsoid|Ellipsoid]]
- [[Maths/Vector Methods/Concepts/Hyperboloid-One-Two-Sheets|Hyperboloid of One and Two Sheets]]

---
*Part of [[Vector Methods/12.6-Cylinders-and-Quadratic-Surfaces.md|12.6 Cylinders and Quadratic Surfaces]]*
