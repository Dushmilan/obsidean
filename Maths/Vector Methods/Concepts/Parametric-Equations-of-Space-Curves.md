---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-13, 13-1-vector-functions, parametric-equations-of-space-curves]
parent: [[Vector Methods/13.1-Vector-Functions-and-Space-Curves.md]]
---

# Parametric Equations of Space Curves

> **Stewart Calculus, Chapter 13, Section 13.1**

## Definition
A space curve can be defined by three parametric equations:
$$x = f(t), \quad y = g(t), \quad z = h(t)$$
These are equivalent to the vector form $\mathbf{r}(t) = \langle f(t), g(t), h(t) \rangle$.

**Symmetric equations** are obtained by eliminating $t$ (when each component can be solved for $t$):
$$\frac{x - x_0}{a} = \frac{y - y_0}{b} = \frac{z - z_0}{c}$$

## Key Properties
- Different parametrizations can describe the same curve (different speed or direction)
- A line is the simplest space curve, with symmetric equations $\frac{x - x_0}{a} = \frac{y - y_0}{b} = \frac{z - z_0}{c}$
- Eliminating $t$ is not always possible (e.g., when components involve transcendental functions)

## Worked Example
Find parametric and symmetric equations for the line through $(1, 2, 3)$ with direction vector $\langle 2, 1, -1 \rangle$.

**Parametric form:**
$$x = 1 + 2t, \quad y = 2 + t, \quad z = 3 - t$$

**Symmetric form** (solve each for $t$):
$$\frac{x - 1}{2} = \frac{y - 2}{1} = \frac{z - 3}{-1}$$

## Related Concepts
- [[Space-Curves]]
- [[Vector-Valued-Functions]]

---

*Part of [[Vector Methods/13.1-Vector-Functions-and-Space-Curves.md|13.1 Vector Functions and Space Curves]]*
