---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-13, 13-3-arc-length-curvature, binormal-vector]
parent: [[Vector Methods/13.3-Arc-Length-and-Curvature.md]]
---

# Binormal Vector

> **Stewart Calculus, Chapter 13, Section 13.3**

## Definition

The binormal vector $\mathbf{B}$ is the cross product of $\mathbf{T}$ and $\mathbf{N}$:

$$\mathbf{B}(t) = \mathbf{T}(t) \times \mathbf{N}(t)$$

## Key Properties

- Unit vector (cross product of two orthogonal unit vectors is a unit vector)
- Orthogonal to both $\mathbf{T}$ and $\mathbf{N}$
- Completes the right-handed orthonormal TNB frame: $\{\mathbf{T}, \mathbf{N}, \mathbf{B}\}$
- $\mathbf{B}$ is perpendicular to the osculating plane (the plane containing $\mathbf{T}$ and $\mathbf{N}$)
- $\mathbf{B}$ measures how quickly the curve twists out of the osculating plane

## Worked Example

Find $\mathbf{B}$ for the helix $\mathbf{r}(t) = \langle \cos t, \sin t, t \rangle$.

**Step 1:** We have:
$$\mathbf{T} = \frac{1}{\sqrt{2}}\langle -\sin t, \cos t, 1 \rangle, \quad \mathbf{N} = \langle -\cos t, -\sin t, 0 \rangle$$

**Step 2:** Compute the cross product:
$$\mathbf{B} = \mathbf{T} \times \mathbf{N} = \frac{1}{\sqrt{2}} \begin{vmatrix} \mathbf{i} & \mathbf{j} & \mathbf{k} \\ -\sin t & \cos t & 1 \\ -\cos t & -\sin t & 0 \end{vmatrix}$$

$$= \frac{1}{\sqrt{2}}\langle \sin t, -\cos t, 1 \rangle$$

Verify: $\|\mathbf{B}\| = \frac{1}{\sqrt{2}}\sqrt{\sin^2 t + \cos^2 t + 1} = 1$.

## Related Concepts

- [[TNB-Frame-Frenet-Serret-Frame]]
- [[Normal-and-Osculating-Planes]]

---

*Part of [[Vector Methods/13.3-Arc-Length-and-Curvature.md|13.3 Arc Length and Curvature]]*
