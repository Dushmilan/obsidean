---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-13, 13-2-derivatives-integrals, unit-tangent-vector]
parent: [[Vector Methods/13.2-Derivatives-and-Integrals.md]]
---

# Unit Tangent Vector

> **Stewart Calculus, Chapter 13, Section 13.2**

## Definition
The unit tangent vector is the normalised tangent vector, defined by:
$$
\mathbf{T}(t) = \frac{\mathbf{r}'(t)}{\|\mathbf{r}'(t)\|}
$$

## Key Properties
- Always has magnitude 1: $\|\mathbf{T}(t)\| = 1$
- Points in the direction of motion along the curve
- Defined only where $\mathbf{r}'(t) \neq \mathbf{0}$
- Used as the first vector in the Frenet-Serret frame for curvature and torsion calculations

## Worked Example
For $\mathbf{r}(t) = \langle t^3, \sin t, e^{2t} \rangle$, find $\mathbf{T}(t)$.

From earlier, $\mathbf{r}'(t) = \langle 3t^2, \cos t, 2e^{2t} \rangle$. Compute the magnitude:
$$
\|\mathbf{r}'(t)\| = \sqrt{9t^4 + \cos^2 t + 4e^{4t}}
$$

Therefore:
$$
\mathbf{T}(t) = \frac{1}{\sqrt{9t^4 + \cos^2 t + 4e^{4t}}} \langle 3t^2, \cos t, 2e^{2t} \rangle
$$

## Related Concepts
- [[Derivative-of-a-Vector-Function]]
- [[Tangent-Vector]]
- [[Curvature-Definition]]

---

*Part of [[Vector Methods/13.2-Derivatives-and-Integrals.md|13.2 Derivatives and Integrals]]*
