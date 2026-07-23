---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-6-quadric-surfaces, traces]
parent: [[Vector Methods/12.6-Cylinders-and-Quadratic-Surfaces.md]]
---

# Traces and Cross-Sections
> Stewart Calculus, Chapter 12, Section 12.6

## Definition
A **trace** (or cross-section) of a surface is the curve formed by intersecting the surface with a plane. The most useful traces come from the coordinate planes ($x=0$, $y=0$, $z=0$) and planes parallel to them ($x=k$, $y=k$, $z=k$).

## Key Properties
- **Horizontal trace** ($z = k$): shows the cross-sectional shape at height $k$.
- **Vertical traces** ($x = k$ or $y = k$): show the profile of the surface in that direction.
- Traces are the primary tool for **identifying quadric surfaces** — if all traces in one direction are ellipses and all traces in another are hyperbolas, the surface is a hyperboloid.
- Degenerate traces (points, lines) occur at special values (e.g., vertex of a cone).

## Worked Example
**Surface:** $\dfrac{x^2}{4} + \dfrac{y^2}{9} + z^2 = 1$

**Horizontal traces** ($z = k$):
$$\frac{x^2}{4} + \frac{y^2}{9} = 1 - k^2$$
- $k = 0$: $\frac{x^2}{4} + \frac{y^2}{9} = 1$ — ellipse (semi-axes 2, 3).
- $k = \pm 1$: $\frac{x^2}{4} + \frac{y^2}{9} = 0$ — single point $(0, 0)$.
- $|k| > 1$: no real points.

**Vertical trace** ($y = 0$):
$$\frac{x^2}{4} + z^2 = 1$$
- Ellipse with semi-axes 2 (along $x$) and 1 (along $z$).

**Conclusion:** All traces are ellipses (or points) → the surface is an **ellipsoid**.

## Related Concepts
- [[Maths/Vector Methods/Concepts/Identifying-Quadratic-Surfaces|Identifying Quadratic Surfaces]]
- [[Maths/Vector Methods/Concepts/Ellipsoid|Ellipsoid]]

---
*Part of [[Vector Methods/12.6-Cylinders-and-Quadratic-Surfaces.md|12.6 Cylinders and Quadratic Surfaces]]*
