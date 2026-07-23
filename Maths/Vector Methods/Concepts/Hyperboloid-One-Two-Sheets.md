---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-6-quadric-surfaces, hyperboloid]
parent: [[Vector Methods/12.6-Cylinders-and-Quadratic-Surfaces.md]]
---

# Hyperboloid of One and Two Sheets
> Stewart Calculus, Chapter 12, Section 12.6

## Definition
**One sheet:**

$$\frac{x^2}{a^2} + \frac{y^2}{b^2} - \frac{z^2}{c^2} = 1$$

**Two sheets:**

$$-\frac{x^2}{a^2} - \frac{y^2}{b^2} + \frac{z^2}{c^2} = 1$$

The sign pattern distinguishes them: one minus sign → one sheet (connected); two minus signs → two sheets (disconnected).

## Key Properties
- **One sheet:** A single connected surface, roughly tube-shaped. The axis of the unique minus sign is the axis of symmetry (here, $z$).
- **Two sheets:** Two separate bowl-shaped pieces. The axis of the unique positive term is the axis of symmetry (here, $z$).
- Both have **hyperbola** traces in vertical planes and **ellipse** traces in horizontal planes (when the horizontal plane intersects the surface).

## Worked Example
**One sheet:** $x^2 + y^2 - z^2 = 1$

- At $z = 0$: $x^2 + y^2 = 1$ — circle of radius 1 (the "waist").
- At $z = k$: $x^2 + y^2 = 1 + k^2$ — circle of radius $\sqrt{1+k^2}$, widening as $|k|$ grows.
- In the $xz$-plane ($y=0$): $x^2 - z^2 = 1$ — hyperbola.

**Two sheets:** $-x^2 - y^2 + z^2 = 1$

- At $z = 0$: $-x^2 - y^2 = 1$ — no real points (gap between the two pieces).
- At $z = \pm 2$: $x^2 + y^2 = 3$ — circle of radius $\sqrt{3}$.
- The surface splits into an upper bowl ($z \geq 1$) and lower bowl ($z \leq -1$).

## Related Concepts
- [[Maths/Vector Methods/Concepts/Identifying-Quadratic-Surfaces|Identifying Quadratic Surfaces]]
- [[Maths/Vector Methods/Concepts/Ellipsoid|Ellipsoid]]

---
*Part of [[Vector Methods/12.6-Cylinders-and-Quadratic-Surfaces.md|12.6 Cylinders and Quadratic Surfaces]]*
