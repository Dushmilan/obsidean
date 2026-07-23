---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-13, 13-1-vector-functions, space-curves]
parent: [[Vector Methods/13.1-Vector-Functions-and-Space-Curves.md]]
---

# Space Curves

> **Stewart Calculus, Chapter 13, Section 13.1**

## Definition
A **space curve** is the set of all points $(x, y, z)$ traced by a vector function $\mathbf{r}(t)$ as $t$ varies over its domain. It is a one-dimensional path in three-dimensional space, not confined to a single plane.

$$C = \{\, \mathbf{r}(t) \mid t \in \text{dom}(\mathbf{r}) \,\}$$

## Key Properties
- A space curve may be closed (the start and end points coincide), simple (no self-intersections), or both
- If two components are constant, the curve lies in a plane parallel to a coordinate plane
- The curve is **smooth** if $\mathbf{r}'(t)$ is continuous and $\mathbf{r}'(t) \ne \mathbf{0}$ (except possibly at isolated points)

## Worked Example
The vector function $\mathbf{r}(t) = \langle \cos t,\, \sin t,\, t \rangle$ traces a **helix** winding upward around the $z$-axis.

- At $t = 0$: $(1, 0, 0)$
- At $t = \pi$: $(-1, 0, \pi)$
- At $t = 2\pi$: $(1, 0, 2\pi)$

The curve lies on the cylinder $x^2 + y^2 = 1$ and ascends at a constant rate.

## Related Concepts
- [[Parametric-Equations-of-Space-Curves]]
- [[Vector-Valued-Functions]]

---

*Part of [[Vector Methods/13.1-Vector-Functions-and-Space-Curves.md|13.1 Vector Functions and Space Curves]]*
