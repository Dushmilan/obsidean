---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-13, 13-4-motion, velocity-vector]
parent: [[Vector Methods/13.4-Motion-in-Space.md]]
---

# Velocity Vector

> **Stewart Calculus, Chapter 13, Section 13.4**

## Definition
The velocity vector is the derivative of the position vector with respect to time:

$$\mathbf{v}(t) = \mathbf{r}'(t) = \langle f'(t), g'(t), h'(t) \rangle$$

It represents the instantaneous rate of change of position and always points in the direction of motion.

## Key Properties
- Points in the direction of motion at each instant
- Its magnitude $\|\mathbf{v}(t)\|$ equals the speed
- Zero velocity implies a stationary point (or change of direction)
- Continuous and differentiable when the path is smooth

## Worked Example
Let $\mathbf{r}(t) = \langle t^2, t^3, t \rangle$. Find the velocity vector.

$$\mathbf{v}(t) = \mathbf{r}'(t) = \langle 2t, 3t^2, 1 \rangle$$

At $t = 1$, the velocity is $\mathbf{v}(1) = \langle 2, 3, 1 \rangle$, pointing in that direction.

## Related Concepts
- [[Speed]]
- [[Acceleration-Vector]]

---

*Part of [[Vector Methods/13.4-Motion-in-Space.md|13.4 Motion in Space]]*
