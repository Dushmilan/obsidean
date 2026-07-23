---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-2-vectors, operations]
parent: [[Vector Methods/12.2-Vectors.md]]
---

# Scalar Multiplication

> **Stewart Calculus, Chapter 12, Section 12.2**

## Definition
**Scalar multiplication** scales a vector by a real number $c$:

$$c\mathbf{v} = \langle cv_1, \, cv_2, \, cv_3 \rangle$$

## Key Properties
- **Magnitude:** $\|c\mathbf{v}\| = |c|\,\|\mathbf{v}\|$
- **Direction:**
  - If $c > 0$: same direction as $\mathbf{v}$
  - If $c < 0$: opposite direction to $\mathbf{v}$
  - If $c = 0$: gives the zero vector $\mathbf{0}$
- **Distributive:** $c(\mathbf{u} + \mathbf{v}) = c\mathbf{u} + c\mathbf{v}$
- **Associative:** $(cd)\mathbf{v} = c(d\mathbf{v})$

## Worked Example
$$3 \cdot \langle 1, -2, 4 \rangle = \langle 3, -6, 12 \rangle$$
$$\|3\mathbf{v}\| = 3\|\mathbf{v}\| = 3\sqrt{1 + 4 + 16} = 3\sqrt{21}$$

$$-2 \cdot \langle 1, -2, 4 \rangle = \langle -2, 4, -8 \rangle$$
The result has twice the magnitude and points in the opposite direction.

## Related Concepts
- [[Vector-Definition]]
- [[Parallel-Vectors]]
- [[Negative-Vector]]

---

*Part of [[Vector Methods/12.2-Vectors.md|12.2 Vectors]]*
