---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-5-lines-planes, distance-from-point-to-plane]
parent: [[Vector Methods/12.5-Equations-of-Lines-and-Planes.md]]
---

# Distance from a Point to a Plane

> **Stewart Calculus, Chapter 12, Section 12.5**

## Definition
The distance from $P_0(x_0, y_0, z_0)$ to the plane $ax + by + cz = d$ is:
$$D = \frac{|ax_0 + by_0 + cz_0 - d|}{\sqrt{a^2 + b^2 + c^2}} = \frac{|\mathbf{n} \cdot \overrightarrow{P_0P}|}{\|\mathbf{n}\|}$$
where $P$ is any point on the plane. This is the scalar projection of $\overrightarrow{P_0P}$ onto the normal vector $\mathbf{n}$.

## Key Properties
- $D = 0$ iff the point lies on the plane
- The formula works for any point, whether on one side or the other
- Distance is always non-negative (absolute value)
- Equivalent to projecting any in-plane vector onto $\mathbf{n}$

## Worked Example
Distance from $(1, 2, 3)$ to $2x - y + 4z = 12$:
$$D = \frac{|2(1) - 1(2) + 4(3) - 12|}{\sqrt{4 + 1 + 16}} = \frac{|2 - 2 + 12 - 12|}{\sqrt{21}} = 0$$
The point lies on the plane.

Distance from $(0, 0, 0)$ to $x + y + z = 1$:
$$D = \frac{|0 + 0 + 0 - 1|}{\sqrt{1 + 1 + 1}} = \frac{1}{\sqrt{3}}$$

## Related Concepts
- [[Scalar-Equation-of-a-Plane]]
- [[Scalar-Projection]]

---

*Part of [[Vector Methods/12.5-Equations-of-Lines-and-Planes.md|12.5 Equations of Lines and Planes]]*
