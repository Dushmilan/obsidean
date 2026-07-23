---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-13, 13-4-motion, projectile-motion]
parent: [[Vector Methods/13.4-Motion-in-Space.md]]
---

# Projectile Motion

> **Stewart Calculus, Chapter 13, Section 13.4**

## Definition
Motion under gravity with no air resistance. The acceleration is constant:

$$\mathbf{a}(t) = -g\mathbf{k} \quad \text{where } g \approx 9.8 \text{ m/s}^2$$

Integrating twice gives position:

$$\mathbf{r}(t) = \mathbf{r}_0 + \mathbf{v}_0 t - \frac{1}{2}gt^2\mathbf{k}$$
$$\mathbf{v}(t) = \mathbf{v}_0 - gt\mathbf{k}$$

## Key Properties
- Horizontal velocity components remain constant (no horizontal force)
- Vertical velocity changes linearly: $v_z(t) = v_{0z} - gt$
- The trajectory is a parabola in the vertical plane containing $\mathbf{v}_0$
- At the apex, $v_z = 0$ while horizontal components persist

## Worked Example
Launched from the origin with $\mathbf{v}_0 = \langle 10, 0, 20 \rangle$ m/s.

$$\mathbf{v}(t) = \langle 10, 0, 20 - 9.8t \rangle$$
$$\mathbf{r}(t) = \langle 10t, 0, 20t - 4.9t^2 \rangle$$

The trajectory lies in the $xz$-plane. The projectile reaches apex when $v_z = 0$: $t = \frac{20}{9.8} \approx 2.04$ s.

## Related Concepts
- [[Velocity-Vector]]
- [[Acceleration-Vector]]

---

*Part of [[Vector Methods/13.4-Motion-in-Space.md|13.4 Motion in Space]]*
