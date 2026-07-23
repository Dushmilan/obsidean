---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-13, 13-1-vector-functions, helix]
parent: [[Vector Methods/13.1-Vector-Functions-and-Space-Curves.md]]
---

# Helix

> **Stewart Calculus, Chapter 13, Section 13.1**

## Definition
A **helix** is a space curve that winds uniformly around a cylinder. Its standard parametrization is:
$$\mathbf{r}(t) = \langle a\cos t,\, a\sin t,\, bt \rangle$$
where $a > 0$ is the radius of the cylinder and $b$ controls the pitch (vertical spacing between successive coils).

## Key Properties
- Lies on the cylinder $x^2 + y^2 = a^2$
- Pitch (vertical rise per full revolution): $2\pi |b|$
- The curve is smooth everywhere since $\mathbf{r}'(t) = \langle -a\sin t,\, a\cos t,\, b \rangle \ne \mathbf{0}$ for all $t$
- If $b > 0$ the helix ascends; if $b < 0$ it descends; if $b = 0$ it reduces to a circle

## Worked Example
Consider $\mathbf{r}(t) = \langle \cos t,\, \sin t,\, t/2 \rangle$.

- $a = 1$: unit cylinder $x^2 + y^2 = 1$
- $b = 1/2$: pitch is $2\pi \cdot \tfrac{1}{2} = \pi$
- At $t = 0$: $(1, 0, 0)$
- At $t = 2\pi$: $(1, 0, \pi)$ — one full revolution, rose $\pi$ units

## Related Concepts
- [[Space-Curves]]
- [[Parametric-Equations-of-Space-Curves]]

---

*Part of [[Vector Methods/13.1-Vector-Functions-and-Space-Curves.md|13.1 Vector Functions and Space Curves]]*
