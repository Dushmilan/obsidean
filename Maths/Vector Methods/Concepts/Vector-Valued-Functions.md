---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-13, 13-1-vector-functions, vector-valued-functions]
parent: [[Vector Methods/13.1-Vector-Functions-and-Space-Curves.md]]
---

# Vector-Valued Functions

> **Stewart Calculus, Chapter 13, Section 13.1**

## Definition
A vector-valued function of one real variable $t$ assigns a vector to each scalar input:
$$\mathbf{r}(t) = \langle f(t),\, g(t),\, h(t) \rangle = f(t)\,\mathbf{i} + g(t)\,\mathbf{j} + h(t)\,\mathbf{k}$$
where $f, g, h$ are scalar component functions of the parameter $t$.

## Key Properties
- Each component $f(t), g(t), h(t)$ is a real-valued (scalar) function of $t$
- The output is a vector in $\mathbb{R}^3$ (or $\mathbb{R}^2$ if only two components)
- The parameter $t$ typically represents time or an independent variable

## Worked Example
Let $\mathbf{r}(t) = \langle t^2,\, \sin t,\, e^t \rangle$.

- At $t = 0$: $\mathbf{r}(0) = \langle 0,\, 0,\, 1 \rangle$
- At $t = \pi$: $\mathbf{r}(\pi) = \langle \pi^2,\, 0,\, e^\pi \rangle$

## Related Concepts
- [[Domain-of-a-Vector-Function]]
- [[Space-Curves]]

---

*Part of [[Vector Methods/13.1-Vector-Functions-and-Space-Curves.md|13.1 Vector Functions and Space Curves]]*
