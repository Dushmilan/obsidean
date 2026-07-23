---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-6-quadric-surfaces, paraboloid]
parent: [[Vector Methods/12.6-Cylinders-and-Quadratic-Surfaces.md]]
---

# Paraboloid (Elliptic and Hyperbolic)
> Stewart Calculus, Chapter 12, Section 12.6

## Definition
**Elliptic paraboloid:**

$$z = \frac{x^2}{a^2} + \frac{y^2}{b^2}$$

**Hyperbolic paraboloid:**

$$z = \frac{x^2}{a^2} - \frac{y^2}{b^2}$$

The key diagnostic: one variable appears **linearly** (not squared) — it determines the opening direction.

## Key Properties
- **Elliptic:** Bowl-shaped, opens along the linear variable's axis. Cross-sections perpendicular to that axis are ellipses.
- **Hyperbolic:** Saddle-shaped. Cross-sections in one direction are parabolas opening up; in the other, parabolas opening down.
- Neither type is bounded — they extend infinitely in the opening direction.
- The vertex (lowest/highest point) sits at the origin.

## Worked Example
**Elliptic:** $z = x^2 + y^2$

- At $z = k > 0$: circle $x^2 + y^2 = k$ — radius grows as $\sqrt{k}$.
- At $z = 0$: single point (the origin).
- At $z < 0$: no real points — the bowl opens upward only.
- Profile in $xz$-plane: parabola $z = x^2$.

**Hyperbolic (saddle):** $z = x^2 - y^2$

- At $x = 0$: $z = -y^2$ — parabola opening **downward**.
- At $y = 0$: $z = x^2$ — parabola opening **upward**.
- At $z = 0$: $x^2 = y^2$ → $y = \pm x$ — two crossing lines.
- At $z = 1$: $x^2 - y^2 = 1$ — hyperbola.

## Related Concepts
- [[Maths/Vector Methods/Concepts/Identifying-Quadratic-Surfaces|Identifying Quadratic Surfaces]]
- [[Maths/Vector Methods/Concepts/Traces-and-Cross-Sections|Traces and Cross-Sections]]

---
*Part of [[Vector Methods/12.6-Cylinders-and-Quadratic-Surfaces.md|12.6 Cylinders and Quadratic Surfaces]]*
