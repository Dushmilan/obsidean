---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-13, 13-3-arc-length-curvature, curvature-definition]
parent: [[Vector Methods/13.3-Arc-Length-and-Curvature.md]]
---

# Curvature Definition

> **Stewart Calculus, Chapter 13, Section 13.3**

## Definition

Curvature $\kappa$ measures how fast a curve changes direction at a given point:

$$\kappa = \left\|\frac{d\mathbf{T}}{ds}\right\| = \frac{\|\mathbf{T}'(t)\|}{\|\mathbf{r}'(t)\|}$$

where $\mathbf{T}$ is the unit tangent vector and $s$ is the arc length parameter.

## Key Properties

- For a circle of radius $r$: $\kappa = \dfrac{1}{r}$ (constant curvature)
- For a straight line: $\kappa = 0$ (no turning)
- $\kappa \geq 0$ always (curvature is non-negative)
- High curvature = sharp bend; low curvature = gentle curve
- Curvature is independent of parameterization

## Worked Example

Find the curvature of the helix $\mathbf{r}(t) = \langle \cos t, \sin t, t \rangle$.

**Step 1:** Compute $\mathbf{T}(t)$:
$$\mathbf{r}'(t) = \langle -\sin t, \cos t, 1 \rangle, \quad \|\mathbf{r}'(t)\| = \sqrt{2}$$
$$\mathbf{T}(t) = \frac{1}{\sqrt{2}}\langle -\sin t, \cos t, 1 \rangle$$

**Step 2:** Compute $\mathbf{T}'(t)$:
$$\mathbf{T}'(t) = \frac{1}{\sqrt{2}}\langle -\cos t, -\sin t, 0 \rangle, \quad \|\mathbf{T}'(t)\| = \frac{1}{\sqrt{2}}$$

**Step 3:** Apply formula:
$$\kappa = \frac{\|\mathbf{T}'(t)\|}{\|\mathbf{r}'(t)\|} = \frac{1/\sqrt{2}}{\sqrt{2}} = \frac{1}{2}$$

## Related Concepts

- [[Unit-Tangent-Vector]]
- [[Curvature-via-Cross-Product]]
- [[TNB-Frame-Frenet-Serret-Frame]]

---

*Part of [[Vector Methods/13.3-Arc-Length-and-Curvature.md|13.3 Arc Length and Curvature]]*
