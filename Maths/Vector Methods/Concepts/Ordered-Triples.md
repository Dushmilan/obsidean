---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-1-3d-coordinates, ordered-triples]
parent: [[Vector Methods/12.1-3D-Coordinate-Systems.md]]
---

# Ordered Triples

> **Stewart Calculus, Chapter 12, Section 12.1**

## Definition
An **ordered triple** $(a, b, c)$ is a representation of a point in three-dimensional space where:
- $a$ is the signed distance from the point to the $yz$-plane ($x$-coordinate)
- $b$ is the signed distance from the point to the $xz$-plane ($y$-coordinate)
- $c$ is the signed distance from the point to the $xy$-plane ($z$-coordinate)

The order matters: $(a, b, c) \neq (b, a, c)$ unless $a = b$.

## Key Properties
- Each coordinate is the signed perpendicular distance from the point to the corresponding coordinate plane.
- Two points are equal if and only if all three corresponding coordinates are equal: $(a, b, c) = (d, e, f) \iff a = d,\; b = e,\; c = f$.
- The sign of each coordinate indicates which side of the corresponding coordinate plane the point lies on.

## Worked Example
**Interpret the point $P(2, -1, 4)$ in terms of distances from coordinate planes.**

1. $x = 2$: $P$ is 2 units from the $yz$-plane, on the positive side.
2. $y = -1$: $P$ is 1 unit from the $xz$-plane, on the negative side.
3. $z = 4$: $P$ is 4 units from the $xy$-plane, on the positive side.
4. So $P$ is 2 units right of the $yz$-plane, 1 unit behind the $xz$-plane, and 4 units above the $xy$-plane.

## Related Concepts
- [[Three-Dimensional-Rectangular-Coordinate-System]]
- [[Projections-onto-Coordinate-Planes]]
- [[Octants]]

---

*Part of [[Vector Methods/12.1-3D-Coordinate-Systems.md|12.1 Three-Dimensional Coordinate Systems]]*
