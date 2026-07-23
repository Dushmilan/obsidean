---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-5-lines-planes, intersections-line-plane]
parent: [[Vector Methods/12.5-Equations-of-Lines-and-Planes.md]]
---

# Intersections: Line–Plane, Line–Line, Plane–Plane

> **Stewart Calculus, Chapter 12, Section 12.5**

## Definition
- **Line–Plane:** Substitute the line's parametric equations into the plane's equation. Solve for $t$, then back-substitute.
- **Line–Line:** Two lines in 3D may not intersect (skew lines). Solve the system; if inconsistent, the lines are skew or parallel.
- **Plane–Plane:** Intersection is a line when normals are not parallel. Direction vector: $\mathbf{v} = \mathbf{n}_1 \times \mathbf{n}_2$.
- **Three planes:** Can intersect at a point, a line, a plane, or not at all.

## Key Properties
- Line–plane intersection always yields exactly one point (unless the line lies in the plane or is parallel to it)
- Two planes are parallel iff $\mathbf{n}_1 \times \mathbf{n}_2 = \mathbf{0}$
- Three planes can have no common solution (inconsistent system)
- Skew lines: same direction check as parallel, but no shared point

## Worked Example
Line $x = t$, $y = 1 + t$, $z = 2 - t$ intersects plane $x + y + z = 5$.
Substitute: $t + (1 + t) + (2 - t) = 5 \Rightarrow t + 3 = 5 \Rightarrow t = 2$.
Intersection point: $(2, 3, 0)$.

## Related Concepts
- [[Vector-Equation-of-a-Line]]
- [[Scalar-Equation-of-a-Plane]]

---

*Part of [[Vector Methods/12.5-Equations-of-Lines-and-Planes.md|12.5 Equations of Lines and Planes]]*
