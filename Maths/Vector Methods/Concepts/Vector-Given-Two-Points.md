---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-2-vectors, two-points]
parent: [[Vector Methods/12.2-Vectors.md]]
---

# Vector Given Two Points

> **Stewart Calculus, Chapter 12, Section 12.2**

## Definition

The vector from point $A(x_1, y_1, z_1)$ to point $B(x_2, y_2, z_2)$ is found by subtracting the coordinates of the initial point from the terminal point:

$$\overrightarrow{AB} = \langle x_2 - x_1,\; y_2 - y_1,\; z_2 - z_1 \rangle$$

The direction is always from the first point (initial) to the second point (terminal).

## Key Properties

- $\overrightarrow{AB} \neq \overrightarrow{BA}$ in general; in fact $\overrightarrow{BA} = -\overrightarrow{AB}$
- $\overrightarrow{AA} = \mathbf{0}$ (vector from a point to itself is the zero vector)
- $\overrightarrow{AB} = \mathbf{r}_B - \mathbf{r}_A$ where $\mathbf{r}_A, \mathbf{r}_B$ are position vectors
- The formula works identically in 2D: from $A(x_1,y_1)$ to $B(x_2,y_2)$ gives $\langle x_2-x_1, y_2-y_1 \rangle$

## Worked Example

Find the vector from $P(1, 2, 3)$ to $Q(4, 6, 8)$.

**Solution:**

$$\overrightarrow{PQ} = \langle 4-1,\; 6-2,\; 8-3 \rangle = \langle 3, 4, 5 \rangle$$

## Related Concepts

- [[Vector Methods/Concepts/Position-Vector.md|Position Vector]]
- [[Vector Methods/Concepts/Vector-Components.md|Vector Components]]

---

*Part of [[Vector Methods/12.2-Vectors.md|12.2 Vectors]]*
