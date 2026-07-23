---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-13, 13-4-motion, normal-component]
parent: [[Vector Methods/13.4-Motion-in-Space.md]]
---

# Normal Component of Acceleration

> **Stewart Calculus, Chapter 13, Section 13.4**

## Definition
The normal component of acceleration measures the rate of change of direction (turning):

$$a_N = \frac{\|\mathbf{v} \times \mathbf{a}\|}{\|\mathbf{v}\|} = \kappa v^2$$

It is the projection of $\mathbf{a}$ onto the principal unit normal vector $\mathbf{N}$.

## Key Properties
- Always non-negative: $a_N \geq 0$
- Proportional to curvature $\kappa$ and speed squared $v^2$
- Tighter curves (larger $\kappa$) produce larger $a_N$
- Zero $a_N$ means the path is momentarily straight

## Worked Example
For uniform circular motion with constant speed $v$ on a circle of radius $r$:

- $\kappa = \frac{1}{r}$, so $a_N = \frac{v^2}{r}$
- $a_T = 0$ (speed is constant)
- The acceleration points radially inward (centripetal)

This matches the familiar centripetal acceleration formula.

## Related Concepts
- [[Tangential-Component-of-Acceleration]]
- [[Acceleration-Vector-Decomposition]]
- [[Curvature-Definition]]

---

*Part of [[Vector Methods/13.4-Motion-in-Space.md|13.4 Motion in Space]]*
