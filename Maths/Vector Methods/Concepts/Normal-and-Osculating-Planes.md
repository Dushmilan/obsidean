---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-13, 13-3-arc-length-curvature, normal-osculating-planes]
parent: [[Vector Methods/13.3-Arc-Length-and-Curvature.md]]
---

# Normal and Osculating Planes

> **Stewart Calculus, Chapter 13, Section 13.3**

## Definition

Two important planes associated with a space curve at a point $\mathbf{r}(a)$:

**Normal plane:** Perpendicular to the tangent line. Contains $\mathbf{N}$ and $\mathbf{B}$.
$$\mathbf{r}'(a) \cdot (\mathbf{r} - \mathbf{r}(a)) = 0$$

**Osculating plane:** The best-fitting plane to the curve. Contains $\mathbf{T}$ and $\mathbf{N}$.
$$\mathbf{B}(a) \cdot (\mathbf{r} - \mathbf{r}(a)) = 0$$

## Key Properties

- The normal plane has $\mathbf{r}'(a)$ (or equivalently $\mathbf{T}(a)$) as its normal vector
- The osculating plane has $\mathbf{B}(a)$ as its normal vector
- For planar curves, the osculating plane is simply the plane of the curve
- The osculating circle (circle of curvature) lies in the osculating plane, with radius $1/\kappa$
- The curve is locally tangent to the osculating plane — it stays closest to this plane near the point

## Worked Example

Find the normal and osculating planes for the helix $\mathbf{r}(t) = \langle \cos t, \sin t, t \rangle$ at $t=0$.

**At $t=0$:** $\mathbf{r}(0) = \langle 1, 0, 0 \rangle$.

**Step 1:** Compute the frame at $t=0$:
$$\mathbf{T}(0) = \frac{1}{\sqrt{2}}\langle 0, 1, 1 \rangle, \quad \mathbf{B}(0) = \frac{1}{\sqrt{2}}\langle 0, -1, 1 \rangle$$

**Step 2:** Normal plane (normal = $\mathbf{T}(0) = \langle 0, 1, 1 \rangle$):
$$0(x-1) + 1(y-0) + 1(z-0) = 0 \quad\Longrightarrow\quad y + z = 0$$

**Step 3:** Osculating plane (normal = $\mathbf{B}(0) = \langle 0, -1, 1 \rangle$):
$$0(x-1) + (-1)(y-0) + 1(z-0) = 0 \quad\Longrightarrow\quad -y + z = 0 \quad\Longrightarrow\quad y = z$$

## Related Concepts

- [[TNB-Frame-Frenet-Serret-Frame]]
- [[Binormal-Vector]]

---

*Part of [[Vector Methods/13.3-Arc-Length-and-Curvature.md|13.3 Arc Length and Curvature]]*
