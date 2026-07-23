---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-1-3d-coordinates, distance-formula]
parent: [[Vector Methods/12.1-3D-Coordinate-Systems.md]]
---

# Distance Formula in Three Dimensions

> **Stewart Calculus, Chapter 12, Section 12.1**

## Definition
The **distance** between two points $P_1(x_1, y_1, z_1)$ and $P_2(x_2, y_2, z_2)$ in three-dimensional space is given by:

$$|P_1 P_2| = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2 + (z_2 - z_1)^2}$$

This is a direct generalization of the 2D distance formula, obtained by applying the Pythagorean theorem twice.

## Key Properties
- Derived from the Pythagorean theorem applied in two perpendicular directions.
- The distance is always non-negative: $|P_1 P_2| \geq 0$, with equality iff $P_1 = P_2$.
- Symmetric: $|P_1 P_2| = |P_2 P_1|$.
- Satisfies the triangle inequality: $|P_1 P_3| \leq |P_1 P_2| + |P_2 P_3|$.

## Worked Example
**Find the distance between $P(1, 2, 3)$ and $Q(4, 6, 8)$.**

1. Compute the differences: $\Delta x = 4 - 1 = 3$, $\Delta y = 6 - 2 = 4$, $\Delta z = 8 - 3 = 5$.
2. Square each difference: $3^2 = 9$, $4^2 = 16$, $5^2 = 25$.
3. Sum the squares: $9 + 16 + 25 = 50$.
4. Take the square root: $|PQ| = \sqrt{50} = 5\sqrt{2}$.

## Related Concepts
- [[Standard-Equation-of-a-Sphere]]
- [[Ordered-Triples]]
- [[Three-Dimensional-Rectangular-Coordinate-System]]

---

*Part of [[Vector Methods/12.1-3D-Coordinate-Systems.md|12.1 Three-Dimensional Coordinate Systems]]*
