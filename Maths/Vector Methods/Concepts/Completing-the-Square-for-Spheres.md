---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-1-3d-coordinates, completing-the-square]
parent: [[Vector Methods/12.1-3D-Coordinate-Systems.md]]
---

# Completing the Square for Spheres

> **Stewart Calculus, Chapter 12, Section 12.1**

## Definition
**Completing the square** is the algebraic technique used to convert the general second-degree equation of a sphere

$$x^2 + y^2 + z^2 + Dx + Ey + Fz + G = 0$$

into the standard form $(x - h)^2 + (y - k)^2 + (z - l)^2 = r^2$, from which the center and radius can be read directly.

## Key Properties
- Group terms by variable: $(x^2 + Dx) + (y^2 + Ey) + (z^2 + Fz) = -G$.
- For each group, add $\left(\frac{\text{coefficient}}{2}\right)^2$ to both sides.
- The constant added to the right side is $h^2 + k^2 + l^2 - G$, which equals $r^2$.
- If $r^2 > 0$, the equation represents a sphere; if $r^2 = 0$, it is a point; if $r^2 < 0$, there is no real surface.

## Worked Example
**Convert $x^2 + y^2 + z^2 - 4x + 6y - 8z = 0$ to standard form.**

1. Group by variable: $(x^2 - 4x) + (y^2 + 6y) + (z^2 - 8z) = 0$.
2. Complete the square for each:
   - $x$: $\left(\frac{-4}{2}\right)^2 = 4$
   - $y$: $\left(\frac{6}{2}\right)^2 = 9$
   - $z$: $\left(\frac{-8}{2}\right)^2 = 16$
3. Add $4 + 9 + 16 = 29$ to both sides:
$$(x^2 - 4x + 4) + (y^2 + 6y + 9) + (z^2 - 8z + 16) = 29$$
4. Factor: $(x - 2)^2 + (y + 3)^2 + (z - 4)^2 = 29$.
5. **Center:** $(2, -3, 4)$, **Radius:** $\sqrt{29}$.

## Related Concepts
- [[Standard-Equation-of-a-Sphere]]
- [[Distance-Formula-in-Three-Dimensions]]
- [[Inequalities-Representing-Regions-in-R3]]

---

*Part of [[Vector Methods/12.1-3D-Coordinate-Systems.md|12.1 Three-Dimensional Coordinate Systems]]*
