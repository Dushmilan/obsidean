---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-13, 13-2-derivatives-integrals, definite-integrals-of-vector-functions]
parent: [[Vector Methods/13.2-Derivatives-and-Integrals.md]]
---

# Definite Integrals of Vector Functions

> **Stewart Calculus, Chapter 13, Section 13.2**

## Definition
The definite integral of a vector function $\mathbf{r}(t) = \langle f(t), g(t), h(t) \rangle$ over $[a, b]$ is computed component-wise:
$$
\int_a^b \mathbf{r}(t)\,dt = \left\langle \int_a^b f(t)\,dt, \;\int_a^b g(t)\,dt, \;\int_a^b h(t)\,dt \right\rangle
$$

## Key Properties
- The result is a vector, not a scalar
- Linearity holds: $\int [c_1 \mathbf{u}(t) + c_2 \mathbf{v}(t)]\,dt = c_1 \int \mathbf{u}(t)\,dt + c_2 \int \mathbf{v}(t)\,dt$
- Each component integral must exist independently
- The triangle inequality extends: $\left\|\int_a^b \mathbf{r}(t)\,dt\right\| \leq \int_a^b \|\mathbf{r}(t)\|\,dt$

## Worked Example
Evaluate $\int_0^1 \langle t, t^2, e^t \rangle\,dt$.

Integrate each component:
- $\int_0^1 t\,dt = \left[\frac{t^2}{2}\right]_0^1 = \frac{1}{2}$
- $\int_0^1 t^2\,dt = \left[\frac{t^3}{3}\right]_0^1 = \frac{1}{3}$
- $\int_0^1 e^t\,dt = \left[e^t\right]_0^1 = e - 1$

$$
\int_0^1 \langle t, t^2, e^t \rangle\,dt = \left\langle \frac{1}{2},\; \frac{1}{3},\; e - 1 \right\rangle
$$

## Related Concepts
- [[Fundamental-Theorem-of-Calculus-for-Vector-Functions]]
- [[Derivative-of-a-Vector-Function]]

---

*Part of [[Vector Methods/13.2-Derivatives-and-Integrals.md|13.2 Derivatives and Integrals]]*
