---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-1-3d-coordinates, projections]
parent: [[Vector Methods/12.1-3D-Coordinate-Systems.md]]
---

# Projections onto Coordinate Planes

> **Stewart Calculus, Chapter 12, Section 12.1**

## Definition
Given a point $P(a, b, c)$ in $\mathbb{R}^3$, the **projections** of $P$ onto the coordinate planes are obtained by setting the coordinate corresponding to the perpendicular axis to zero:
- **$xy$-projection:** $(a, b, 0)$ — drop perpendicular to the $xy$-plane by setting $z = 0$
- **$yz$-projection:** $(0, b, c)$ — drop perpendicular to the $yz$-plane by setting $x = 0$
- **$xz$-projection:** $(a, 0, c)$ — drop perpendicular to the $xz$-plane by setting $y = 0$

## Key Properties
- A projection is the foot of the perpendicular from the point to the plane.
- The distance from $P$ to a coordinate plane equals the absolute value of the corresponding coordinate.
- Projections reduce 3D points to 2D points, making them useful for visualization and diagramming.
- Each projection lies in exactly one coordinate plane.

## Worked Example
**Find all three projections of $P(3, -2, 5)$.**

1. **$xy$-projection:** Set $z = 0$: $(3, -2, 0)$
2. **$yz$-projection:** Set $x = 0$: $(0, -2, 5)$
3. **$xz$-projection:** Set $y = 0$: $(3, 0, 5)$

Each projection drops a perpendicular from $P$ to the respective plane.

## Related Concepts
- [[Ordered-Triples]]
- [[Coordinate-Planes]]
- [[Three-Dimensional-Rectangular-Coordinate-System]]

---

*Part of [[Vector Methods/12.1-3D-Coordinate-Systems.md|12.1 Three-Dimensional Coordinate Systems]]*
