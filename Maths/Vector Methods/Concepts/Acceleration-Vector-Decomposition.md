---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-13, 13-4-motion, acceleration-decomposition]
parent: [[Vector Methods/13.4-Motion-in-Space.md]]
---

# Acceleration Vector Decomposition

> **Stewart Calculus, Chapter 13, Section 13.4**

## Definition
Any acceleration vector decomposes into tangential and normal components along the Frenet frame:

$$\mathbf{a} = a_T \mathbf{T} + a_N \mathbf{N}$$

where:
- $a_T = \frac{\mathbf{v} \cdot \mathbf{a}}{\|\mathbf{v}\|}$ (tangential — changes speed)
- $a_N = \frac{\|\mathbf{v} \times \mathbf{a}\|}{\|\mathbf{v}\|}$ (normal — changes direction)

## Key Properties
- $\mathbf{T}$ and $\mathbf{N}$ are orthogonal, so $\|\mathbf{a}\|^2 = a_T^2 + a_N^2$ (Pythagorean relation)
- $a_T$ captures how fast the object speeds up or slows down
- $a_N$ captures how sharply the path curves
- $a_N = 0$ implies straight-line motion; $a_T = 0$ implies constant speed

## Worked Example
For $\mathbf{r}(t) = \langle \cos t, \sin t, t \rangle$:

$$\mathbf{v} = \langle -\sin t, \cos t, 1 \rangle, \quad \|\mathbf{v}\| = \sqrt{2}$$
$$\mathbf{a} = \langle -\cos t, -\sin t, 0 \rangle$$

$$a_T = \frac{\mathbf{v} \cdot \mathbf{a}}{\sqrt{2}} = \frac{\sin t \cos t - \sin t \cos t}{\sqrt{2}} = 0$$

Speed is constant, so $\mathbf{a}$ is entirely normal: $a_N = 1$, and $\mathbf{a} = -\cos t\,\mathbf{N}$.

## Related Concepts
- [[Tangential-Component-of-Acceleration]]
- [[Normal-Component-of-Acceleration]]

---

*Part of [[Vector Methods/13.4-Motion-in-Space.md|13.4 Motion in Space]]*
