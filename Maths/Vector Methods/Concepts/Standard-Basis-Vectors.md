---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-2-vectors, basis, unit-vectors]
parent: [[Vector Methods/12.2-Vectors.md]]
---

# Standard Basis Vectors

> **Stewart Calculus, Chapter 12, Section 12.2**

## Definition

The **standard basis vectors** in three dimensions are the unit vectors pointing along the positive coordinate axes:

$$\mathbf{i} = \langle 1, 0, 0 \rangle, \quad \mathbf{j} = \langle 0, 1, 0 \rangle, \quad \mathbf{k} = \langle 0, 0, 1 \rangle$$

Any vector $\mathbf{a} = \langle a_1, a_2, a_3 \rangle$ can be written as a linear combination of the standard basis vectors:

$$\mathbf{a} = a_1\mathbf{i} + a_2\mathbf{j} + a_3\mathbf{k}$$

## Key Properties

- Each standard basis vector has magnitude 1: $\|\mathbf{i}\| = \|\mathbf{j}\| = \|\mathbf{k}\| = 1$
- They are mutually perpendicular (orthogonal): $\mathbf{i} \perp \mathbf{j}$, $\mathbf{j} \perp \mathbf{k}$, $\mathbf{i} \perp \mathbf{k}$
- They form a right-handed coordinate system
- In 2D, the standard basis is just $\mathbf{i} = \langle 1, 0 \rangle$ and $\mathbf{j} = \langle 0, 1 \rangle$

## Worked Example

Write $\mathbf{v} = \langle -5, 8, 0 \rangle$ in terms of standard basis vectors.

**Solution:** $\mathbf{v} = -5\mathbf{i} + 8\mathbf{j} + 0\mathbf{k} = -5\mathbf{i} + 8\mathbf{j}$.

## Related Concepts

- [[Vector Methods/Concepts/Unit-Vector.md|Unit Vector]]
- [[Vector Methods/Concepts/Vector-Components.md|Vector Components]]

---

*Part of [[Vector Methods/12.2-Vectors.md|12.2 Vectors]]*
