---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-5-lines-planes, vector-equation-of-a-line]
parent: [[Vector Methods/12.5-Equations-of-Lines-and-Planes.md]]
---

# Vector Equation of a Line

> **Stewart Calculus, Chapter 12, Section 12.5**

## Definition
A line through point $\mathbf{r}_0$ with direction vector $\mathbf{v}$ is given by:
$$\mathbf{r}(t) = \mathbf{r}_0 + t\mathbf{v}$$
where $t \in \mathbb{R}$, $\mathbf{r}_0$ is the position vector of a known point on the line, and $\mathbf{v}$ is a direction vector parallel to the line.

## Key Properties
- $t = 0$ gives the point $\mathbf{r}_0$
- $t > 0$ moves in the direction of $\mathbf{v}$; $t < 0$ moves opposite
- Any nonzero scalar multiple of $\mathbf{v}$ yields the same line
- The parameterisation is not unique — different points or direction vectors can describe the same line

## Worked Example
Find the vector equation of the line through $P(1,2,3)$ with direction $\mathbf{v} = \langle 2,1,-1 \rangle$.

$$\mathbf{r}(t) = \langle 1,2,3 \rangle + t\langle 2,1,-1 \rangle$$

At $t = 1$: $\mathbf{r}(1) = \langle 3,3,2 \rangle$. At $t = -1$: $\mathbf{r}(-1) = \langle -1,1,4 \rangle$. Both points lie on the line.

## Related Concepts
- [[Parametric-Equations-of-a-Line]]
- [[Symmetric-Equations-of-a-Line]]

---

*Part of [[Vector Methods/12.5-Equations-of-Lines-and-Planes.md|12.5 Equations of Lines and Planes]]*
