---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-6-quadric-surfaces, ellipsoid]
parent: [[Vector Methods/12.6-Cylinders-and-Quadratic-Surfaces.md]]
---

# Ellipsoid
> Stewart Calculus, Chapter 12, Section 12.6

## Definition
The **ellipsoid** is given by

$$\frac{x^2}{a^2} + \frac{y^2}{b^2} + \frac{z^2}{c^2} = 1$$

where $a, b, c > 0$ are the semi-axis lengths.

## Key Properties
- All intercepts are $(\pm a, 0, 0)$, $(0, \pm b, 0)$, $(0, 0, \pm c)$.
- Every trace (horizontal and vertical) is an **ellipse**.
- If $a = b = c$, the surface is a **sphere** of radius $a$.
- Bounded surface — fits inside a box $[-a,a] \times [-b,b] \times [-c,c]$.

## Worked Example
**Equation:** $\dfrac{x^2}{4} + \dfrac{y^2}{9} + z^2 = 1$

Identify $a^2 = 4$, $b^2 = 9$, $c^2 = 1$, so $a = 2$, $b = 3$, $c = 1$.

- $x$-intercepts: $(\pm 2, 0, 0)$
- $y$-intercepts: $(0, \pm 3, 0)$
- $z$-intercepts: $(0, 0, \pm 1)$
- Trace in $xy$-plane ($z=0$): $\frac{x^2}{4} + \frac{y^2}{9} = 1$ — ellipse with semi-axes 2 and 3.
- Trace in $xz$-plane ($y=0$): $\frac{x^2}{4} + z^2 = 1$ — ellipse with semi-axes 2 and 1.

## Related Concepts
- [[Maths/Vector Methods/Concepts/Identifying-Quadratic-Surfaces|Identifying Quadratic Surfaces]]
- [[Maths/Vector Methods/Concepts/Traces-and-Cross-Sections|Traces and Cross-Sections]]

---
*Part of [[Vector Methods/12.6-Cylinders-and-Quadratic-Surfaces.md|12.6 Cylinders and Quadratic Surfaces]]*
