---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-5-lines-planes, parametric-equations-of-a-line]
parent: [[Vector Methods/12.5-Equations-of-Lines-and-Planes.md]]
---

# Parametric Equations of a Line

> **Stewart Calculus, Chapter 12, Section 12.5**

## Definition
From the vector equation $\mathbf{r}(t) = \langle x_0 + at,\; y_0 + bt,\; z_0 + ct \rangle$, the parametric equations are:
$$x = x_0 + at, \quad y = y_0 + bt, \quad z = z_0 + ct$$
Each coordinate is expressed as a linear function of the parameter $t$, where $(x_0, y_0, z_0)$ is a point on the line and $\langle a, b, c \rangle$ is the direction vector.

## Key Properties
- Each equation describes the motion of one coordinate independently
- Eliminating $t$ between equations yields symmetric equations
- Setting $z = 0$ (or any coordinate) gives the trace on that coordinate plane
- Equivalent to the vector equation — just component-wise

## Worked Example
Line through $(1, -2, 4)$ with direction $\langle 3, 1, 2 \rangle$:
$$x = 1 + 3t, \quad y = -2 + t, \quad z = 4 + 2t$$
At $t = 0$: $(1, -2, 4)$. At $t = 1$: $(4, -1, 6)$.

## Related Concepts
- [[Vector-Equation-of-a-Line]]
- [[Symmetric-Equations-of-a-Line]]

---

*Part of [[Vector Methods/12.5-Equations-of-Lines-and-Planes.md|12.5 Equations of Lines and Planes]]*
