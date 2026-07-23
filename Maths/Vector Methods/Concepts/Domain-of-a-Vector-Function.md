---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-13, 13-1-vector-functions, domain-of-a-vector-function]
parent: [[Vector Methods/13.1-Vector-Functions-and-Space-Curves.md]]
---

# Domain of a Vector Function

> **Stewart Calculus, Chapter 13, Section 13.1**

## Definition
The domain of $\mathbf{r}(t) = \langle f(t), g(t), h(t) \rangle$ is the intersection of the domains of the individual component functions:
$$\text{dom}(\mathbf{r}) = \text{dom}(f) \cap \text{dom}(g) \cap \text{dom}(h)$$
If no domain is specified, the largest subset of $\mathbb{R}$ on which all components are defined is assumed.

## Key Properties
- The domain can be a single point, an interval, a union of intervals, or the empty set
- Domain restrictions arise from square roots, logarithms, rational expressions, trigonometric denominators, etc.
- The domain may be a closed interval $[a, b]$, an open interval, or unbounded

## Worked Example
Find the domain of $\mathbf{r}(t) = \langle \sqrt{t},\, \ln t,\, \tfrac{1}{t} \rangle$.

- $\sqrt{t}$ requires $t \ge 0$
- $\ln t$ requires $t > 0$
- $\tfrac{1}{t}$ requires $t \ne 0$

Intersection: $t > 0$, so the domain is $(0, \infty)$.

## Related Concepts
- [[Vector-Valued-Functions]]
- [[Limits-and-Continuity-of-Vector-Functions]]

---

*Part of [[Vector Methods/13.1-Vector-Functions-and-Space-Curves.md|13.1 Vector Functions and Space Curves]]*
