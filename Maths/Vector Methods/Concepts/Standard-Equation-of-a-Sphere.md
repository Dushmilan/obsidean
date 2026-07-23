---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-1-3d-coordinates, sphere]
parent: [[Vector Methods/12.1-3D-Coordinate-Systems.md]]
---

# Standard Equation of a Sphere

> **Stewart Calculus, Chapter 12, Section 12.1**

## Definition
The **standard equation of a sphere** with center $(h, k, l)$ and radius $r$ is:

$$(x - h)^2 + (y - k)^2 + (z - l)^2 = r^2$$

A sphere is the set of all points in $\mathbb{R}^3$ that are at a fixed distance $r$ from the center point $(h, k, l)$.

## Key Properties
- The center is $(h, k, l)$ and the radius is $r > 0$.
- All points on the sphere are equidistant from the center.
- The general form expands to: $x^2 + y^2 + z^2 + Dx + Ey + Fz + G = 0$, where $D = -2h$, $E = -2k$, $F = -2l$, and $G = h^2 + k^2 + l^2 - r^2$.
- A sphere centered at the origin has equation $x^2 + y^2 + z^2 = r^2$.

## Worked Example
**Find the center and radius of $(x - 2)^2 + (y + 1)^2 + (z - 3)^2 = 16$.**

1. Compare with the standard form $(x - h)^2 + (y - k)^2 + (z - l)^2 = r^2$.
2. $h = 2$, $k = -1$ (since $y + 1 = y - (-1)$), $l = 3$.
3. $r^2 = 16$, so $r = 4$.
4. **Center:** $(2, -1, 3)$, **Radius:** $4$.

## Related Concepts
- [[Completing-the-Square-for-Spheres]]
- [[Distance-Formula-in-Three-Dimensions]]
- [[Inequalities-Representing-Regions-in-R3]]

---

*Part of [[Vector Methods/12.1-3D-Coordinate-Systems.md|12.1 Three-Dimensional Coordinate Systems]]*
