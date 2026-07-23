---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-13, 13-3-arc-length-curvature, arc-length-space-curve]
parent: [[Vector Methods/13.3-Arc-Length-and-Curvature.md]]
---

# Arc Length of a Space Curve

> **Stewart Calculus, Chapter 13, Section 13.3**

## Definition

The length $L$ of a smooth curve from $t=a$ to $t=b$ is:

$$L = \int_a^b \|\mathbf{r}'(t)\|\,dt = \int_a^b \sqrt{\left(\frac{dx}{dt}\right)^2 + \left(\frac{dy}{dt}\right)^2 + \left(\frac{dz}{dt}\right)^2}\,dt$$

The curve must be smooth: $\mathbf{r}'(t)$ is continuous and never zero on $[a, b]$.

## Key Properties

- The integrand $\|\mathbf{r}'(t)\|$ is the speed of the particle tracing the curve
- For a curve in 2D, the formula reduces to $L = \int_a^b \sqrt{(dx/dt)^2 + (dy/dt)^2}\,dt$
- Arc length is always non-negative
- Reversing the orientation of the curve does not change $L$

## Worked Example

Find the length of the helix $\mathbf{r}(t) = \langle \cos t, \sin t, t \rangle$ from $t=0$ to $t=2\pi$.

**Step 1:** Compute $\mathbf{r}'(t)$:
$$\mathbf{r}'(t) = \langle -\sin t, \cos t, 1 \rangle$$

**Step 2:** Compute the speed:
$$\|\mathbf{r}'(t)\| = \sqrt{\sin^2 t + \cos^2 t + 1} = \sqrt{2}$$

**Step 3:** Integrate:
$$L = \int_0^{2\pi} \sqrt{2}\,dt = 2\pi\sqrt{2}$$

## Related Concepts

- [[Arc-Length-Function]]
- [[Curvature-Definition]]

---

*Part of [[Vector Methods/13.3-Arc-Length-and-Curvature.md|13.3 Arc Length and Curvature]]*
