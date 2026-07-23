---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-2-vectors, unit-vector]
parent: [[Vector Methods/12.2-Vectors.md]]
---

# Unit Vector

> **Stewart Calculus, Chapter 12, Section 12.2**

## Definition

A **unit vector** is any vector whose magnitude equals 1. A vector $\mathbf{u}$ is a unit vector if and only if:

$$\|\mathbf{u}\| = 1$$

Unit vectors specify direction only, with no scaling.

## Key Properties

- Any nonzero vector can be scaled to a unit vector (see [[Vector Methods/Concepts/Normalization-Unit-Vector-in-Same-Direction.md|Normalization]])
- The standard basis vectors $\mathbf{i}, \mathbf{j}, \mathbf{k}$ are unit vectors
- A unit vector $\mathbf{u}$ in the direction of $\mathbf{a}$ satisfies $\mathbf{a} = \|\mathbf{a}\|\,\mathbf{u}$
- There are infinitely many unit vectors in 3D (all points on the unit sphere)

## Worked Example

Verify that $\mathbf{u} = \frac{1}{\sqrt{2}}\langle 1, 1, 0 \rangle$ is a unit vector.

**Solution:**

$$\|\mathbf{u}\| = \left\| \frac{1}{\sqrt{2}}\langle 1, 1, 0 \rangle \right\| = \frac{1}{\sqrt{2}} \sqrt{1^2 + 1^2 + 0^2} = \frac{1}{\sqrt{2}} \cdot \sqrt{2} = 1 \checkmark$$

## Related Concepts

- [[Vector Methods/Concepts/Normalization-Unit-Vector-in-Same-Direction.md|Normalization]]
- [[Vector Methods/Concepts/Standard-Basis-Vectors.md|Standard Basis Vectors]]

---

*Part of [[Vector Methods/12.2-Vectors.md|12.2 Vectors]]*
