---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-4-cross-product]
parent: [[Vector Methods/12.4-Cross-Product.md]]
---

# Cross Product via Symbolic Determinant

$$\mathbf{a}\times\mathbf{b} = \begin{vmatrix} \mathbf{i} & \mathbf{j} & \mathbf{k} \\ a_1 & a_2 & a_3 \\ b_1 & b_2 & b_3 \end{vmatrix}$$

Expanding along the first row:

$$= (a_2b_3 - a_3b_2)\mathbf{i} - (a_1b_3 - a_3b_1)\mathbf{j} + (a_1b_2 - a_2b_1)\mathbf{k}$$

## Example

$$\mathbf{i}\times\mathbf{j} = \begin{vmatrix} \mathbf{i} & \mathbf{j} & \mathbf{k} \\ 1 & 0 & 0 \\ 0 & 1 & 0 \end{vmatrix} = (0-0)\mathbf{i} - (0-0)\mathbf{j} + (1-0)\mathbf{k} = \mathbf{k}$$

## Related

- [[Cross-Product-Definition]]
- [[Determinants-of-Order-2-and-3]]

*Part of [[Vector Methods/12.4-Cross-Product.md|12.4 Cross Product]]*
