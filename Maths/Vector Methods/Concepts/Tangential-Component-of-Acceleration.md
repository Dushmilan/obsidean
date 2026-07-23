---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-13, 13-4-motion, tangential-component]
parent: [[Vector Methods/13.4-Motion-in-Space.md]]
---

# Tangential Component of Acceleration

> **Stewart Calculus, Chapter 13, Section 13.4**

## Definition
The tangential component of acceleration measures the rate of change of speed along the curve:

$$a_T = \frac{\mathbf{v} \cdot \mathbf{a}}{\|\mathbf{v}\|} = \frac{d}{dt}\|\mathbf{v}(t)\|$$

It is the projection of $\mathbf{a}$ onto the unit tangent vector $\mathbf{T}$.

## Key Properties
- $a_T > 0$: object is speeding up
- $a_T < 0$: object is slowing down
- $a_T = 0$: speed is instantaneously constant (turning point or uniform motion)
- Independent of curvature — only captures change in speed

## Worked Example
Given $\mathbf{v} = \langle 3, 0, 0 \rangle$ and $\mathbf{a} = \langle 2, 1, 0 \rangle$, find $a_T$.

$$a_T = \frac{\mathbf{v} \cdot \mathbf{a}}{\|\mathbf{v}\|} = \frac{(3)(2) + (0)(1) + (0)(0)}{3} = \frac{6}{3} = 2$$

The object is speeding up with tangential acceleration $2$ units/s².

## Related Concepts
- [[Normal-Component-of-Acceleration]]
- [[Acceleration-Vector-Decomposition]]

---

*Part of [[Vector Methods/13.4-Motion-in-Space.md|13.4 Motion in Space]]*
