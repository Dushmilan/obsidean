---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-13, 13-1-vector-functions, limits-and-continuity-of-vector-functions]
parent: [[Vector Methods/13.1-Vector-Functions-and-Space-Curves.md]]
---

# Limits and Continuity of Vector Functions

> **Stewart Calculus, Chapter 13, Section 13.1**

## Definition
The limit of a vector function is evaluated component-wise:
$$\lim_{t \to a} \mathbf{r}(t) = \left\langle \lim_{t \to a} f(t),\, \lim_{t \to a} g(t),\, \lim_{t \to a} h(t) \right\rangle$$

$\mathbf{r}(t)$ is **continuous at** $a$ if and only if each component function is continuous at $a$:
$$\lim_{t \to a} \mathbf{r}(t) = \mathbf{r}(a)$$

## Key Properties
- Existence of the limit requires the component limits to exist (as real numbers)
- $\mathbf{r}(t)$ is continuous on its domain if every component function is continuous on that domain
- Limits obey the usual algebraic laws (sum, scalar multiple, dot/cross product of limits)

## Worked Example
Evaluate $\lim_{t \to 0} \left\langle \dfrac{\sin t}{t},\, e^t,\, \cos t \right\rangle$.

- $\lim_{t \to 0} \dfrac{\sin t}{t} = 1$
- $\lim_{t \to 0} e^t = 1$
- $\lim_{t \to 0} \cos t = 1$

$$\lim_{t \to 0} \mathbf{r}(t) = \langle 1, 1, 1 \rangle$$

## Related Concepts
- [[Vector-Valued-Functions]]
- [[Domain-of-a-Vector-Function]]

---

*Part of [[Vector Methods/13.1-Vector-Functions-and-Space-Curves.md|13.1 Vector Functions and Space Curves]]*
