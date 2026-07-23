---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-4-cross-product]
parent: [[Vector Methods/12.4-Cross-Product.md]]
---

# Determinants of Order 2 and 3

## 2×2 Determinant

$$\begin{vmatrix} a & b \\ c & d \end{vmatrix} = ad - bc$$

## 3×3 Determinant

Computed via cofactor expansion along any row or column. Along the first row:

$$\begin{vmatrix} a_1 & a_2 & a_3 \\ b_1 & b_2 & b_3 \\ c_1 & c_2 & c_3 \end{vmatrix} = a_1\begin{vmatrix} b_2 & b_3 \\ c_2 & c_3 \end{vmatrix} - a_2\begin{vmatrix} b_1 & b_3 \\ c_1 & c_3 \end{vmatrix} + a_3\begin{vmatrix} b_1 & b_2 \\ c_1 & c_2 \end{vmatrix}$$

## Example

$$\begin{vmatrix} 1 & 2 \\ 3 & 4 \end{vmatrix} = (1)(4) - (2)(3) = 4 - 6 = -2$$

## Related

- [[Cross-Product-via-Symbolic-Determinant]]

*Part of [[Vector Methods/12.4-Cross-Product.md|12.4 Cross Product]]*
