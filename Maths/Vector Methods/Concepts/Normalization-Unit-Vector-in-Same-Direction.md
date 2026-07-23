---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-2-vectors, normalization]
parent: [[Vector Methods/12.2-Vectors.md]]
---

# Normalization (Unit Vector in Same Direction)

> **Stewart Calculus, Chapter 12, Section 12.2**

## Definition

Given any nonzero vector $\mathbf{a}$, the **unit vector in the same direction** as $\mathbf{a}$ is:

$$\hat{\mathbf{a}} = \frac{\mathbf{a}}{\|\mathbf{a}\|}$$

The process of dividing a vector by its magnitude to obtain a unit vector is called **normalization**.

## Key Properties

- $\hat{\mathbf{a}}$ always points in the same direction as $\mathbf{a}$
- $\|\hat{\mathbf{a}}\| = 1$ by construction
- $\mathbf{a} = \|\mathbf{a}\|\,\hat{\mathbf{a}}$ — any nonzero vector equals its magnitude times its unit direction vector
- Normalization is undefined for the zero vector (division by zero)

## Worked Example

Find the unit vector in the same direction as $\mathbf{a} = \langle 3, 4, 0 \rangle$.

**Solution:**

Step 1: Find the magnitude.
$$\|\mathbf{a}\| = \sqrt{3^2 + 4^2 + 0^2} = \sqrt{25} = 5$$

Step 2: Divide by the magnitude.
$$\hat{\mathbf{a}} = \frac{\mathbf{a}}{\|\mathbf{a}\|} = \frac{1}{5}\langle 3, 4, 0 \rangle = \left\langle \frac{3}{5},\; \frac{4}{5},\; 0 \right\rangle$$

Check: $\|\hat{\mathbf{a}}\| = \sqrt{(3/5)^2 + (4/5)^2} = \sqrt{9/25 + 16/25} = \sqrt{25/25} = 1$. $\checkmark$

## Related Concepts

- [[Vector Methods/Concepts/Unit-Vector.md|Unit Vector]]
- [[Vector Methods/Concepts/Magnitude-of-a-Vector.md|Magnitude of a Vector]]

---

*Part of [[Vector Methods/12.2-Vectors.md|12.2 Vectors]]*
