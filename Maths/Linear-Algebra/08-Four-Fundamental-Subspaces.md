---
date: 2026-07-21
type: linear-algebra-cluster
tags: [linear-algebra, strang]
lectures: [10]
prereq_clusters: ["05", "06", "07"]
status: complete
source: manual
---

# 08 — The Four Fundamental Subspaces

## Concept Statement
State the master theorem: every $m \times n$ matrix $A$ of rank $r$ defines four subspaces, two in $\mathbb{R}^n$ and two in $\mathbb{R}^m$, with dimensions locked to $(r, n-r, r, m-r)$.

## Lecture Sources
- Strang MIT 18.06, Lecture 10: *The Four Fundamental Subspaces*

## Core Material

### The Four Spaces

| Subspace | Where it lives | Dimension | What it is |
|----------|----------------|-----------|------------|
| $C(A)$ | $\mathbb{R}^m$ | $r$ | span of columns of $A$ |
| $N(A)$ | $\mathbb{R}^n$ | $n - r$ | inputs mapped to $0$ |
| $C(A^T)$ (row space) | $\mathbb{R}^n$ | $r$ | span of rows of $A$ |
| $N(A^T)$ (left nullspace) | $\mathbb{R}^m$ | $m - r$ | $\mathbf{y}$ such that $\mathbf{y}^T A = \mathbf{0}^T$ |

### The Fundamental Theorem (Part 1)

$$\dim C(A) = r, \quad \dim N(A) = n - r, \quad \dim C(A^T) = r, \quad \dim N(A^T) = m - r$$

Within $\mathbb{R}^n$: $r + (n-r) = n$. Within $\mathbb{R}^m$: $r + (m-r) = m$. Always.

### The Symmetry Revelation

The number of independent rows of $A$ exactly equals the number of independent columns — both equal $r$. Row rank = column rank.

## Cross-Cluster Links
- **Prereq**: [[05-Transposes-Permutations-Spaces]], [[06-Complete-Solutions-and-Rank]], [[07-Independence-Basis-Dimension]]
- **Forward**: [[09-Matrix-Spaces-and-Rank1]] (subspaces of matrices), [[10-Graphs-Networks-Incidence]] (incidence matrix instantiates all four)
- **Geometry add-on**: [[12-Orthogonal-Vectors-Subspaces]] (Part 2 of the Fundamental Theorem)

## Thematic Summary
Lecture 10 is the architectural blueprint of the entire course. Every matrix decomposes its input space into the row space and nullspace, and its output space into the column space and left nullspace. Once this blueprint is internalised, every later result (orthogonal complements, projections, least squares) is a re-reading of this picture.

## Glossary

| Term | Definition |
|------|------------|
| **Fundamental Theorem (Part 1)** | The dimension count for the four subspaces. |
| **Row Space** | $C(A^T)$; span of rows of $A$. |
| **Left Nullspace** | $N(A^T)$; vectors $\mathbf{y}$ with $\mathbf{y}^T A = \mathbf{0}^T$. |
| **Rank** | $r = \dim C(A) = \dim C(A^T)$. The "size" of $A$'s action. |
