---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-4-cross-product]
parent: [[Vector Methods/12.4-Cross-Product.md]]
---

# Area of a Triangle in R³

The area of a triangle with two sides given by vectors $\mathbf{a}$ and $\mathbf{b}$ is:

$$A = \frac{1}{2}\|\mathbf{a}\times\mathbf{b}\|$$

This is half the area of the corresponding parallelogram.

## Example

Triangle with vertices $(0,0,0)$, $(1,1,0)$, $(0,1,1)$. Using $\mathbf{a}=\langle 1,1,0 \rangle$ and $\mathbf{b}=\langle 0,1,1 \rangle$:

$$\mathbf{a}\times\mathbf{b} = \langle 1,-1,1 \rangle$$

$$A = \frac{1}{2}\|\langle 1,-1,1 \rangle\| = \frac{\sqrt{3}}{2}$$

## Related

- [[Area-of-a-Parallelogram]]

*Part of [[Vector Methods/12.4-Cross-Product.md|12.4 Cross Product]]*
