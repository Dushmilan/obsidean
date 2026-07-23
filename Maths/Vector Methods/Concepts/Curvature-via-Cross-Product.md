---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-13, 13-3-arc-length-curvature, curvature-cross-product]
parent: [[Vector Methods/13.3-Arc-Length-and-Curvature.md]]
---

# Curvature via Cross Product

> **Stewart Calculus, Chapter 13, Section 13.3**

## Definition

A practical formula for curvature that avoids computing $\mathbf{T}'(t)$ directly:

$$\kappa(t) = \frac{\|\mathbf{r}'(t) \times \mathbf{r}''(t)\|}{\|\mathbf{r}'(t)\|^3}$$

## Key Properties

- Only requires first and second derivatives of $\mathbf{r}(t)$
- Equivalent to the definition $\kappa = \|\mathbf{T}'(t)\| / \|\mathbf{r}'(t)\|$ but often easier to compute
- The numerator measures how much $\mathbf{r}'$ deviates from being parallel to $\mathbf{r}''$
- The denominator cubed comes from normalizing by speed three times (once for each factor)

## Worked Example

Find the curvature of $\mathbf{r}(t) = \langle t, t^2, 0 \rangle$ (a parabola in the $xy$-plane).

**Step 1:** Compute derivatives:
$$\mathbf{r}'(t) = \langle 1, 2t, 0 \rangle, \quad \mathbf{r}''(t) = \langle 0, 2, 0 \rangle$$

**Step 2:** Compute the cross product:
$$\mathbf{r}' \times \mathbf{r}'' = \begin{vmatrix} \mathbf{i} & \mathbf{j} & \mathbf{k} \\ 1 & 2t & 0 \\ 0 & 2 & 0 \end{vmatrix} = \langle 0, 0, 2 \rangle$$

So $\|\mathbf{r}' \times \mathbf{r}''\| = 2$.

**Step 3:** Compute the denominator:
$$\|\mathbf{r}'\|^3 = (1 + 4t^2)^{3/2}$$

**Step 4:** Apply the formula:
$$\kappa(t) = \frac{2}{(1 + 4t^2)^{3/2}}$$

At the vertex ($t=0$): $\kappa = 2$ (sharpest point of the parabola).

## Related Concepts

- [[Curvature-Definition]]
- [[Cross-Product-Definition]]

---

*Part of [[Vector Methods/13.3-Arc-Length-and-Curvature.md|13.3 Arc Length and Curvature]]*
