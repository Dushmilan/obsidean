---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-4-cross-product]
parent: [[Vector Methods/12.4-Cross-Product.md]]
---

# Scalar Triple Product

The scalar triple product of three vectors is:

$$\mathbf{a}\cdot(\mathbf{b}\times\mathbf{c})$$

It equals the determinant:

$$\begin{vmatrix} a_1 & a_2 & a_3 \\ b_1 & b_2 & b_3 \\ c_1 & c_2 & c_3 \end{vmatrix}$$

## Geometric Meaning

The absolute value gives the **signed volume of the parallelepiped** formed by $\mathbf{a}$, $\mathbf{b}$, and $\mathbf{c}$.

## Example

$$\langle 1,0,0 \rangle \cdot (\langle 0,1,0 \rangle \times \langle 0,0,1 \rangle) = \langle 1,0,0 \rangle \cdot \langle 0,0,1 \rangle = 1$$

Wait — let's recompute: $\langle 0,1,0 \rangle \times \langle 0,0,1 \rangle = \langle 1,0,0 \rangle$, so $\langle 1,0,0 \rangle \cdot \langle 1,0,0 \rangle = 1$.

## Related

- [[Volume-of-a-Parallelepiped]]
- [[Coplanar-Vectors-Test]]

*Part of [[Vector Methods/12.4-Cross-Product.md|12.4 Cross Product]]*
