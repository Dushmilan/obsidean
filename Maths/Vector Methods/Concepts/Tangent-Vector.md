---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-13, 13-2-derivatives-integrals, tangent-vector]
parent: [[Vector Methods/13.2-Derivatives-and-Integrals.md]]
---

# Tangent Vector

> **Stewart Calculus, Chapter 13, Section 13.2**

## Definition
The derivative $\mathbf{r}'(t)$ is a tangent vector to the space curve traced by $\mathbf{r}(t)$ at the point $\mathbf{r}(t)$. It points in the direction of the tangent line to the curve at parameter $t$.

The tangent line at $\mathbf{r}(a)$ is given by:
$$
\mathbf{r}(a) + t\mathbf{r}'(a)
$$

## Key Properties
- $\mathbf{r}'(t)$ points in the direction of increasing $t$ along the curve
- The tangent vector is not necessarily a unit vector; its magnitude $\|\mathbf{r}'(t)\|$ equals the speed of motion
- If $\mathbf{r}'(t) = \mathbf{0}$, the tangent line is undefined at that point

## Worked Example
Find the tangent vector at $t = \pi$ for $\mathbf{r}(t) = \langle \cos t, \sin t, t \rangle$.

First compute the derivative:
$$
\mathbf{r}'(t) = \langle -\sin t, \cos t, 1 \rangle
$$

Evaluate at $t = \pi$:
$$
\mathbf{r}'(\pi) = \langle -\sin \pi, \cos \pi, 1 \rangle = \langle 0, -1, 1 \rangle
$$

The tangent line at $t = \pi$ is:
$$
\langle -1, 0, \pi \rangle + t\langle 0, -1, 1 \rangle
$$

## Related Concepts
- [[Derivative-of-a-Vector-Function]]
- [[Unit-Tangent-Vector]]

---

*Part of [[Vector Methods/13.2-Derivatives-and-Integrals.md|13.2 Derivatives and Integrals]]*
