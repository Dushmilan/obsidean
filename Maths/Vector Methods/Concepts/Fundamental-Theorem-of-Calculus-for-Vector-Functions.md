---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-13, 13-2-derivatives-integrals, fundamental-theorem-of-calculus-for-vector-functions]
parent: [[Vector Methods/13.2-Derivatives-and-Integrals.md]]
---

# Fundamental Theorem of Calculus for Vector Functions

> **Stewart Calculus, Chapter 13, Section 13.2**

## Definition
The fundamental theorem of calculus extends directly to vector functions. If $\mathbf{r}'(t)$ is continuous on $[a, b]$, then:
$$
\int_a^b \mathbf{r}'(t)\,dt = \mathbf{r}(b) - \mathbf{r}(a)
$$

This is the exact analogue of the scalar fundamental theorem of calculus.

## Key Properties
- Provides a method for computing definite integrals when the antiderivative is known
- The result $\mathbf{r}(b) - \mathbf{r}(a)$ is the net displacement of the curve from $t = a$ to $t = b$
- Can be rearranged: $\mathbf{r}(b) = \mathbf{r}(a) + \int_a^b \mathbf{r}'(t)\,dt$
- Applies to velocity-velocity relationships: displacement equals the integral of velocity

## Worked Example
A particle has velocity $\mathbf{v}(t) = \langle 2t, \cos t, 1 \rangle$. Find the displacement from $t = 0$ to $t = \pi$.

By the fundamental theorem:
$$
\int_0^\pi \mathbf{v}(t)\,dt = \mathbf{r}(\pi) - \mathbf{r}(0)
$$

Compute the integral:
- $\int_0^\pi 2t\,dt = \left[t^2\right]_0^\pi = \pi^2$
- $\int_0^\pi \cos t\,dt = \left[\sin t\right]_0^\pi = 0$
- $\int_0^\pi 1\,dt = \pi$

$$
\text{Displacement} = \langle \pi^2, 0, \pi \rangle
$$

## Related Concepts
- [[Definite-Integrals-of-Vector-Functions]]
- [[Velocity-Vector]]

---

*Part of [[Vector Methods/13.2-Derivatives-and-Integrals.md|13.2 Derivatives and Integrals]]*
