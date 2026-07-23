---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-2-vectors, operations]
parent: [[Vector Methods/12.2-Vectors.md]]
---

# Algebraic Vector Operations

> **Stewart Calculus, Chapter 12, Section 12.2**

## Definition

Vector addition, subtraction, and scalar multiplication are performed **component-wise**.

Given $\mathbf{a} = \langle a_1, a_2, a_3 \rangle$ and $\mathbf{b} = \langle b_1, b_2, b_3 \rangle$:

**Addition:**
$$\mathbf{a} + \mathbf{b} = \langle a_1 + b_1,\; a_2 + b_2,\; a_3 + b_3 \rangle$$

**Subtraction:**
$$\mathbf{a} - \mathbf{b} = \langle a_1 - b_1,\; a_2 - b_2,\; a_3 - b_3 \rangle$$

**Scalar multiplication** (for scalar $c$):
$$c\mathbf{a} = \langle ca_1,\; ca_2,\; ca_3 \rangle$$

## Key Properties

- Addition is commutative and associative (see [[Vector Methods/Concepts/Properties-of-Vectors.md|Properties of Vectors]])
- The negative of a vector is $-\mathbf{a} = \langle -a_1, -a_2, -a_3 \rangle$
- Scalar multiplication distributes over vector addition
- These operations make $\mathbb{R}^3$ a vector space

## Worked Example

Let $\mathbf{a} = \langle 2, -1, 3 \rangle$ and $\mathbf{b} = \langle 4, 0, -2 \rangle$. Find $2\mathbf{a} - 3\mathbf{b}$.

**Solution:**

$$2\mathbf{a} = \langle 4, -2, 6 \rangle$$
$$3\mathbf{b} = \langle 12, 0, -6 \rangle$$
$$2\mathbf{a} - 3\mathbf{b} = \langle 4-12,\; -2-0,\; 6-(-6) \rangle = \langle -8, -2, 12 \rangle$$

## Related Concepts

- [[Vector Methods/Concepts/Properties-of-Vectors.md|Properties of Vectors]]
- [[Vector Methods/Concepts/Vector-Components.md|Vector Components]]

---

*Part of [[Vector Methods/12.2-Vectors.md|12.2 Vectors]]*
