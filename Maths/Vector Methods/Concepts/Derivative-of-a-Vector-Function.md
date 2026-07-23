---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-13, 13-2-derivatives-integrals, derivative-of-a-vector-function]
parent: [[Vector Methods/13.2-Derivatives-and-Integrals.md]]
---

# Derivative of a Vector Function

> **Stewart Calculus, Chapter 13, Section 13.2**

## Definition
The derivative of a vector function $\mathbf{r}(t) = \langle f(t), g(t), h(t) \rangle$ is defined by the limit:

$$
\mathbf{r}'(t) = \lim_{h \to 0} \frac{\mathbf{r}(t+h) - \mathbf{r}(t)}{h} = \langle f'(t), g'(t), h'(t) \rangle
$$

The derivative is computed component-wise.

## Key Properties
- Differentiation is applied independently to each component of the vector function
- $\mathbf{r}'(t)$ exists if and only if $f'(t)$, $g'(t)$, and $h'(t)$ all exist
- Geometrically, $\mathbf{r}'(t)$ represents the velocity vector when $\mathbf{r}(t)$ describes motion

## Worked Example
Given $\mathbf{r}(t) = \langle t^3, \sin t, e^{2t} \rangle$, compute $\mathbf{r}'(t)$.

Differentiate each component:
- $\frac{d}{dt}[t^3] = 3t^2$
- $\frac{d}{dt}[\sin t] = \cos t$
- $\frac{d}{dt}[e^{2t}] = 2e^{2t}$

$$
\mathbf{r}'(t) = \langle 3t^2, \cos t, 2e^{2t} \rangle
$$

## Related Concepts
- [[Tangent-Vector]]
- [[Unit-Tangent-Vector]]

---

*Part of [[Vector Methods/13.2-Derivatives-and-Integrals.md|13.2 Derivatives and Integrals]]*
