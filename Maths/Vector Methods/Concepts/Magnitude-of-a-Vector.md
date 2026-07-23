---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-2-vectors, magnitude]
parent: [[Vector Methods/12.2-Vectors.md]]
---

# Magnitude of a Vector

> **Stewart Calculus, Chapter 12, Section 12.2**

## Definition

The **magnitude** (or length or norm) of a vector $\mathbf{a} = \langle a_1, a_2, a_3 \rangle$ is:

$$\|\mathbf{a}\| = \sqrt{a_1^2 + a_2^2 + a_3^2}$$

This is derived directly from the distance formula. For a 2D vector $\langle a_1, a_2 \rangle$, the magnitude is $\sqrt{a_1^2 + a_2^2}$.

## Key Properties

- $\|\mathbf{a}\| \geq 0$ for all vectors $\mathbf{a}$
- $\|\mathbf{a}\| = 0$ if and only if $\mathbf{a} = \mathbf{0}$
- $\|c\mathbf{a}\| = |c|\,\|\mathbf{a}\|$ for any scalar $c$
- The magnitude satisfies the triangle inequality: $\|\mathbf{a} + \mathbf{b}\| \leq \|\mathbf{a}\| + \|\mathbf{b}\|$

## Worked Example

Find the magnitude of $\mathbf{a} = \langle 3, -4, 5 \rangle$.

**Solution:**

$$\|\mathbf{a}\| = \sqrt{3^2 + (-4)^2 + 5^2} = \sqrt{9 + 16 + 25} = \sqrt{50} = 5\sqrt{2}$$

## Related Concepts

- [[Vector Methods/Concepts/Unit-Vector.md|Unit Vector]]
- [[Vector Methods/Concepts/Normalization-Unit-Vector-in-Same-Direction.md|Normalization]]

---

*Part of [[Vector Methods/12.2-Vectors.md|12.2 Vectors]]*
