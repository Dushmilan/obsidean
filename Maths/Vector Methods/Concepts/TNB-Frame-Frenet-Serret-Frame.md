---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-13, 13-3-arc-length-curvature, tnb-frame-frenet-serret]
parent: [[Vector Methods/13.3-Arc-Length-and-Curvature.md]]
---

# TNB Frame (Frenet-Serret Frame)

> **Stewart Calculus, Chapter 13, Section 13.3**

## Definition

The TNB frame is a moving right-handed orthonormal frame $\{\mathbf{T}, \mathbf{N}, \mathbf{B}\}$ attached to a space curve at each point:

- **Tangent** $\mathbf{T}$: direction of motion
- **Normal** $\mathbf{N}$: direction the curve is turning
- **Binormal** $\mathbf{B} = \mathbf{T} \times \mathbf{N}$: perpendicular to both

## Key Properties

- The frame is orthonormal: $\|\mathbf{T}\| = \|\mathbf{N}\| = \|\mathbf{B}\| = 1$, all pairwise orthogonal
- Right-handed: $\mathbf{T} \times \mathbf{N} = \mathbf{B}$, $\mathbf{N} \times \mathbf{B} = \mathbf{T}$, $\mathbf{B} \times \mathbf{T} = \mathbf{N}$
- Describes the local geometry of the curve at each point
- The **osculating plane** is the plane spanned by $\mathbf{T}$ and $\mathbf{N}$ (the "best-fit plane")
- The **normal plane** is the plane spanned by $\mathbf{N}$ and $\mathbf{B}$ (perpendicular to the tangent)
- The **rectifying plane** is the plane spanned by $\mathbf{T}$ and $\mathbf{B}$

## Worked Example

For the helix $\mathbf{r}(t) = \langle \cos t, \sin t, t \rangle$, compute the full TNB frame.

**Step 1:** Tangent:
$$\mathbf{T} = \frac{1}{\sqrt{2}}\langle -\sin t, \cos t, 1 \rangle$$

**Step 2:** Normal (from Curvature-Definition):
$$\mathbf{N} = \langle -\cos t, -\sin t, 0 \rangle$$

**Step 3:** Binormal:
$$\mathbf{B} = \frac{1}{\sqrt{2}}\langle \sin t, -\cos t, 1 \rangle$$

The helix has constant $\kappa = 1/2$ and constant torsion $\tau = 1/2$, so the TNB frame rotates uniformly as the helix is traversed.

## Related Concepts

- [[Curvature-Definition]]
- [[Normal-and-Osculating-Planes]]

---

*Part of [[Vector Methods/13.3-Arc-Length-and-Curvature.md|13.3 Arc Length and Curvature]]*
