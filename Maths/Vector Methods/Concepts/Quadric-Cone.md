---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-6-quadric-surfaces, cone]
parent: [[Vector Methods/12.6-Cylinders-and-Quadratic-Surfaces.md]]
---

# Quadric Cone
> Stewart Calculus, Chapter 12, Section 12.6

## Definition
The **elliptic cone** is given by

$$\frac{z^2}{c^2} = \frac{x^2}{a^2} + \frac{y^2}{b^2}$$

This is the degenerate case between a hyperboloid of one sheet and a hyperboloid of two sheets — replace the $= 1$ with $= 0$.

## Key Properties
- The surface touches the origin (a single point where all traces meet).
- Horizontal traces ($z = k \neq 0$): ellipses $\frac{x^2}{a^2} + \frac{y^2}{b^2} = \frac{k^2}{c^2}$, growing linearly with $|k|$.
- Vertical traces through the axis: **pairs of intersecting lines** through the origin.
- If $a = b$, the horizontal traces are circles and the surface is a **circular cone**.

## Worked Example
**Circular cone:** $z^2 = x^2 + y^2$

- At $z = 1$: $x^2 + y^2 = 1$ — unit circle.
- At $z = 2$: $x^2 + y^2 = 4$ — circle of radius 2.
- At $z = 0$: $x^2 + y^2 = 0$ — just the origin $(0, 0, 0)$.
- In the $xz$-plane ($y=0$): $z^2 = x^2$ → $z = \pm x$ — two lines through the origin.

The surface opens upward and downward with a $45°$ angle from the $z$-axis (slope = 1 since $a = b = c = 1$).

## Related Concepts
- [[Maths/Vector Methods/Concepts/Hyperboloid-One-Two-Sheets|Hyperboloid of One and Two Sheets]]
- [[Maths/Vector Methods/Concepts/Identifying-Quadratic-Surfaces|Identifying Quadratic Surfaces]]

---
*Part of [[Vector Methods/12.6-Cylinders-and-Quadratic-Surfaces.md|12.6 Cylinders and Quadratic Surfaces]]*
