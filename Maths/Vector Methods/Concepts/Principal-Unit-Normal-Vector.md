---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-13, 13-3-arc-length-curvature, principal-unit-normal]
parent: [[Vector Methods/13.3-Arc-Length-and-Curvature.md]]
---

# Principal Unit Normal Vector

> **Stewart Calculus, Chapter 13, Section 13.3**

## Definition

The principal unit normal vector $\mathbf{N}$ is the unit vector perpendicular to $\mathbf{T}$, pointing toward the center of curvature:

$$\mathbf{N}(t) = \frac{\mathbf{T}'(t)}{\|\mathbf{T}'(t)\|}$$

## Key Properties

- Always a unit vector: $\|\mathbf{N}\| = 1$
- Always perpendicular to $\mathbf{T}$: $\mathbf{T} \cdot \mathbf{N} = 0$
- $\mathbf{N}$ points in the direction the curve is turning (toward the concave side)
- Exists whenever $\mathbf{T}'(t) \neq \mathbf{0}$ (i.e., the curve is actually bending)
- For a straight line, $\mathbf{N}$ is undefined (no curvature)

## Worked Example

Find $\mathbf{N}$ for the circle $\mathbf{r}(t) = \langle \cos t, \sin t \rangle$.

**Step 1:** Compute $\mathbf{T}$:
$$\mathbf{r}'(t) = \langle -\sin t, \cos t \rangle, \quad \|\mathbf{r}'(t)\| = 1$$
$$\mathbf{T}(t) = \langle -\sin t, \cos t \rangle$$

**Step 2:** Compute $\mathbf{T}'(t)$:
$$\mathbf{T}'(t) = \langle -\cos t, -\sin t \rangle, \quad \|\mathbf{T}'(t)\| = 1$$

**Step 3:** Normalize:
$$\mathbf{N}(t) = \frac{\langle -\cos t, -\sin t \rangle}{1} = \langle -\cos t, -\sin t \rangle$$

This points inward toward the center of the circle, as expected.

## Related Concepts

- [[Unit-Tangent-Vector]]
- [[Binormal-Vector]]
- [[TNB-Frame-Frenet-Serret-Frame]]

---

*Part of [[Vector Methods/13.3-Arc-Length-and-Curvature.md|13.3 Arc Length and Curvature]]*
