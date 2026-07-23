---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-1-3d-coordinates, inequalities-regions]
parent: [[Vector Methods/12.1-3D-Coordinate-Systems.md]]
---

# Inequalities Representing Regions in $\mathbb{R}^3$

> **Stewart Calculus, Chapter 12, Section 12.1**

## Definition
Inequalities in three variables describe **solid regions** (volumes) in $\mathbb{R}^3$ rather than surfaces. For example, $x^2 + y^2 + z^2 \leq r^2$ describes the solid ball of radius $r$ centered at the origin, including its boundary. The inequality type determines whether the boundary is included:
- $\leq$ or $\geq$: **solid** region (boundary included)
- $<$ or $>$: **hollow** region (boundary excluded)

## Key Properties
- $x^2 + y^2 + z^2 \leq r^2$ is a solid ball (filled sphere) of radius $r$.
- $x^2 + y^2 + z^2 < r^2$ is the interior of the ball (no boundary).
- $r_1^2 \leq x^2 + y^2 + z^2 \leq r_2^2$ is a **spherical shell** between radii $r_1$ and $r_2$.
- Linear inequalities like $z \geq 0$ describe half-spaces.
- Combined inequalities can describe intersections of regions (e.g., a sphere cut by a plane).

## Worked Example
**Describe the region $1 \leq x^2 + y^2 + z^2 \leq 4$.**

1. $x^2 + y^2 + z^2 \geq 1$: all points at distance $\geq 1$ from the origin (outside or on the sphere of radius 1).
2. $x^2 + y^2 + z^2 \leq 4$: all points at distance $\leq 2$ from the origin (inside or on the sphere of radius 2).
3. The intersection is the **spherical shell** between the sphere of radius 1 and the sphere of radius 2, including both boundary spheres.
4. This is a thick hollow ball with inner radius 1 and outer radius 2.

## Related Concepts
- [[Standard-Equation-of-a-Sphere]]
- [[Completing-the-Square-for-Spheres]]
- [[Planes-Parallel-to-Coordinate-Planes]]

---

*Part of [[Vector Methods/12.1-3D-Coordinate-Systems.md|12.1 Three-Dimensional Coordinate Systems]]*
