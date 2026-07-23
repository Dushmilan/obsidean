---
date: 2026-07-21
type: linear-algebra-cluster
tags: [linear-algebra, strang]
lectures: [11]
prereq_clusters: ["08"]
status: complete
source: manual
---

# 09 — Matrix Spaces and Rank-1 Matrices

## Concept Statement
Lift the vector-space framework one level up: matrices can themselves be vectors. The most elementary matrix is one of rank 1 — a column times a row.

## Lecture Sources
- Strang MIT 18.06, Lecture 11: *Matrix Spaces and Rank 1 Matrices*

## Core Material

### Matrix Spaces are Vector Spaces
The set $M = \mathbb{R}^{3 \times 3}$ of all $3 \times 3$ matrices is a vector space:
- Closed under addition.
- Closed under scalar multiplication.
- Has dimension 9. Standard basis: nine matrices, each with a single $1$.

### Subspaces of $M$

| Subspace | Definition | Dimension |
|----------|------------|-----------|
| Symmetric $3\times3$ | $S = \{A : A = A^T\}$ | 6 |
| Upper triangular $3\times3$ | $U = \{A : a_{ij} = 0 \text{ for } i > j\}$ | 6 |
| $S \cap U$ | diagonal $3\times3$ | 3 |

### Rank-1 Matrices
A matrix of rank $r = 1$ has **every column a multiple of one column** and **every row a multiple of one row**.

**Factorisation:**
$$A = \mathbf{u} \mathbf{v}^T$$
where $\mathbf{u}$ is a single column vector and $\mathbf{v}^T$ is a single row vector.

Rank-1 matrices are the **atoms** of matrix algebra. A rank-$r$ matrix decomposes as the sum of $r$ rank-1 matrices — central to Singular Value Decomposition later.

## Cross-Cluster Links
- **Prereq**: [[08-Four-Fundamental-Subspaces]]
- **Forward**: [[11-Quiz-1-Synthesis]] (synthesis across L1–L12), [[10-Graphs-Networks-Incidence]] (further instantiation)
- **Future**: rank-1 decomposition recurs in eigen-decomposition and SVD

## Thematic Summary
Strang stretches the definition of "vector" to include matrices themselves: any time you can add and scalar-multiply without leaving the set, you have a vector space. Rank-1 matrices reveal the deeper truth: every matrix is a *sum of simple pieces*. This decomposition will resurface in singular value decomposition (SVD) — the most important algorithm in applied linear algebra.

## Glossary

| Term | Definition |
|------|------------|
| **Matrix Space ($M_{m\times n}$)** | The vector space of all $m \times n$ matrices; dimension $mn$. |
| **Symmetric Subspace $S$** | $S = \{A : A = A^T\}$; a subspace of $M$. |
| **Upper Triangular Subspace $U$** | $U = \{A : a_{ij} = 0, i > j\}$; a subspace of $M$. |
| **Rank-1 Matrix** | A matrix that equals $\mathbf{u}\mathbf{v}^T$ for some column $\mathbf{u}$ and row $\mathbf{v}^T$. |
