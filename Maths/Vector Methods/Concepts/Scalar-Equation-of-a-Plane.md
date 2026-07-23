---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-5-lines-planes, scalar-equation-of-a-plane]
parent: [[Vector Methods/12.5-Equations-of-Lines-and-Planes.md]]
---

# Scalar Equation of a Plane

> **Stewart Calculus, Chapter 12, Section 12.5**

## Definition
A plane through point $(x_0, y_0, z_0)$ with normal vector $\mathbf{n} = \langle a, b, c \rangle$ has equation:
$$a(x - x_0) + b(y - y_0) + c(z - z_0) = 0$$
Or equivalently in expanded form:
$$ax + by + cz = d \quad \text{where } d = ax_0 + by_0 + cz_0$$
A single linear equation in $x, y, z$ always defines a plane.

## Key Properties
- The coefficients $a, b, c$ are the components of the normal vector
- Parallel planes share the same $\langle a, b, c \rangle$ (possibly different $d$)
- Setting any variable to zero gives the trace on that coordinate plane
- Two planes are parallel iff their normal vectors are scalar multiples

## Worked Example
Plane through $(1, 2, 3)$ with $\mathbf{n} = \langle 2, -1, 4 \rangle$:
$$2(x - 1) - 1(y - 2) + 4(z - 3) = 0$$
$$2x - y + 4z = 12$$
Check: $2(1) - 2 + 4(3) = 2 - 2 + 12 = 12$ ✓

## Related Concepts
- [[Normal-Vector-to-a-Plane]]
- [[Distance-from-Point-to-Plane]]

---

*Part of [[Vector Methods/12.5-Equations-of-Lines-and-Planes.md|12.5 Equations of Lines and Planes]]*
