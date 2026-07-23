---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-13, 13-4-motion, acceleration-vector]
parent: [[Vector Methods/13.4-Motion-in-Space.md]]
---

# Acceleration Vector

> **Stewart Calculus, Chapter 13, Section 13.4**

## Definition
The acceleration vector is the derivative of the velocity vector (or second derivative of position):

$$\mathbf{a}(t) = \mathbf{v}'(t) = \mathbf{r}''(t) = \langle f''(t), g''(t), h''(t) \rangle$$

It points toward the "inside" of curved paths.

## Key Properties
- Points toward the concave side of the path
- Decomposes into tangential and normal components: $\mathbf{a} = a_T \mathbf{T} + a_N \mathbf{N}$
- Tangential component changes speed; normal component changes direction
- Zero acceleration means constant velocity (straight-line motion at constant speed)

## Worked Example
Let $\mathbf{r}(t) = \langle \cos t, \sin t, t \rangle$. Find the acceleration vector.

$$\mathbf{v}(t) = \langle -\sin t, \cos t, 1 \rangle$$
$$\mathbf{a}(t) = \langle -\cos t, -\sin t, 0 \rangle$$

This points radially inward toward the $z$-axis, as expected for helical motion.

## Related Concepts
- [[Velocity-Vector]]
- [[Tangential-Component-of-Acceleration]]
- [[Normal-Component-of-Acceleration]]

---

*Part of [[Vector Methods/13.4-Motion-in-Space.md|13.4 Motion in Space]]*
