---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-13, 13-3-arc-length-curvature, arc-length-function]
parent: [[Vector Methods/13.3-Arc-Length-and-Curvature.md]]
---

# Arc Length Function

> **Stewart Calculus, Chapter 13, Section 13.3**

## Definition

The arc length function measures distance along a curve from a fixed starting point:

$$s(t) = \int_a^t \|\mathbf{r}'(u)\|\,du$$

where $a$ is the starting parameter value.

## Key Properties

- $s(t)$ is always non-decreasing (arc length only accumulates)
- By the Fundamental Theorem of Calculus: $s'(t) = \|\mathbf{r}'(t)\|$ (the speed)
- The function is invertible when $\mathbf{r}'(t) \neq \mathbf{0}$ for all $t$
- **Reparameterization by arc length:** Given $\mathbf{r}(t)$, substituting $t = t(s)$ yields $\mathbf{r}(s)$ which moves at unit speed: $\|\mathbf{r}'(s)\| = 1$
- Arc length parameterization removes dependence on how fast the curve is traced

## Worked Example

Find the arc length function for the helix $\mathbf{r}(t) = \langle \cos t, \sin t, t \rangle$ starting at $t=0$, and express $\mathbf{r}$ in terms of arc length.

**Step 1:** We know $\|\mathbf{r}'(t)\| = \sqrt{2}$.

**Step 2:** Compute $s(t)$:
$$s(t) = \int_0^t \sqrt{2}\,du = t\sqrt{2}$$

**Step 3:** Invert: $t = s/\sqrt{2}$, so:
$$\mathbf{r}(s) = \left\langle \cos\frac{s}{\sqrt{2}},\, \sin\frac{s}{\sqrt{2}},\, \frac{s}{\sqrt{2}} \right\rangle$$

Verify: $\|\mathbf{r}'(s)\| = 1$ (unit speed).

## Related Concepts

- [[Arc-Length-of-a-Space-Curve]]
- [[Curvature-Definition]]

---

*Part of [[Vector Methods/13.3-Arc-Length-and-Curvature.md|13.3 Arc Length and Curvature]]*
