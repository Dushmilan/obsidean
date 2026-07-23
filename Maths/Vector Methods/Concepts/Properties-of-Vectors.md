---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-2-vectors, properties, laws]
parent: [[Vector Methods/12.2-Vectors.md]]
---

# Properties of Vectors

> **Stewart Calculus, Chapter 12, Section 12.2**

## Definition

The algebraic operations on vectors satisfy **8 fundamental laws**. For any vectors $\mathbf{a}, \mathbf{b}, \mathbf{c}$ and scalars $c, d$:

## Key Properties

1. $\mathbf{a} + \mathbf{b} = \mathbf{b} + \mathbf{a}$ — **Commutative law** for addition
2. $(\mathbf{a} + \mathbf{b}) + \mathbf{c} = \mathbf{a} + (\mathbf{b} + \mathbf{c})$ — **Associative law** for addition
3. $\mathbf{a} + \mathbf{0} = \mathbf{a}$ — **Additive identity** (zero vector)
4. $\mathbf{a} + (-\mathbf{a}) = \mathbf{0}$ — **Additive inverse**
5. $c(\mathbf{a} + \mathbf{b}) = c\mathbf{a} + c\mathbf{b}$ — **Distributive law** (scalar over vector sum)
6. $(c + d)\mathbf{a} = c\mathbf{a} + d\mathbf{a}$ — **Distributive law** (scalar sum over vector)
7. $c(d\mathbf{a}) = (cd)\mathbf{a}$ — **Associative law** for scalar multiplication
8. $1\mathbf{a} = \mathbf{a}$ — **Scalar identity**

## Worked Example

Verify law 5: $c(\mathbf{a} + \mathbf{b}) = c\mathbf{a} + c\mathbf{b}$ for $c = 2$, $\mathbf{a} = \langle 1, 3 \rangle$, $\mathbf{b} = \langle -2, 4 \rangle$.

**Solution:**

LHS: $\mathbf{a} + \mathbf{b} = \langle -1, 7 \rangle$, so $2\langle -1, 7 \rangle = \langle -2, 14 \rangle$.

RHS: $2\langle 1, 3 \rangle + 2\langle -2, 4 \rangle = \langle 2, 6 \rangle + \langle -4, 8 \rangle = \langle -2, 14 \rangle$.

LHS $=$ RHS. $\checkmark$

## Related Concepts

- [[Vector Methods/Concepts/Algebraic-Vector-Operations.md|Algebraic Vector Operations]]
- [[Vector Methods/Concepts/Vector-Components.md|Vector Components]]

---

*Part of [[Vector Methods/12.2-Vectors.md|12.2 Vectors]]*
