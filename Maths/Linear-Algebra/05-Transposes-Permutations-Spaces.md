---
date: 2026-07-21
type: linear-algebra-cluster
tags: [linear-algebra, strang]
lectures: [5, 6]
prereq_clusters: ["02", "03"]
status: complete
source: manual
---

# 05 — Transposes, Permutations, and Vector Spaces $\mathbb{R}^n$

## Concept Statement
Bundle the matrix-component manipulations (transposes, permutations, symmetry) with the first formal definition of *vector space* and the two most important subspaces of any matrix: $C(A)$ and $N(A)$.

## Lecture Sources
- Strang MIT 18.06, Lecture 5: *Transposes, Permutations, Spaces $\mathbb{R}^n$*
- Strang MIT 18.06, Lecture 6: *Column Space and Nullspace*

## Core Material

### Permutation Matrices (recap)
- $P$ is the identity matrix with rows reordered.
- $P^T = P^{-1}$ — orthogonal by construction.
- $n!$ permutations of size $n$.

### Transpose $A^T$
- $A_{ij}^T = A_{ji}$.
- **Product rule**: $(AB)^T = B^T A^T$ (order reverses, as with inverses).

### Symmetric Matrices
- $A = A^T$.
- **Striking fact**: For *any* matrix $R$ (square or not), $R^T R$ is symmetric.
- $\mathbb{R}^n$ sub-tells: the row space $C(A^T)$ is a subspace of $\mathbb{R}^n$.

### Vector Spaces and Subspaces
- $\mathbb{R}^n$ — the space of all $n$-component column vectors with real entries.
- A **subspace** must:
  1. Contain $\mathbf{0}$.
  2. Be closed under addition.
  3. Be closed under scalar multiplication.

**Crucial**: every subspace passes through the origin. A line/plane that doesn't pass through origin is *not* a subspace.

### Subspaces in $\mathbb{R}^3$
Only four possibilities: the origin, a line through origin, a plane through origin, all of $\mathbb{R}^3$.

### Column Space $C(A)$
- All linear combinations of $A$'s columns.
- Lives in $\mathbb{R}^m$ (when $A$ is $m \times n$).
- **Theorem**: $A\mathbf{x} = \mathbf{b}$ has a solution iff $\mathbf{b} \in C(A)$.

### Nullspace $N(A)$
- All $\mathbf{x}$ such that $A\mathbf{x} = \mathbf{0}$.
- Lives in $\mathbb{R}^n$.
- It's a subspace: if $A\mathbf{x} = \mathbf{0}$ and $A\mathbf{y} = \mathbf{0}$, then $A(\mathbf{x}+\mathbf{y}) = \mathbf{0}$. Closed.

## Cross-Cluster Links
- **Prereq**: [[02-Elimination-and-RREF]], [[03-Matrix-Multiplication-and-Inverses]]
- **Next**: [[07-Independence-Basis-Dimension]] (formalises size of $C(A)$, $N(A)$)
- **Forward**: [[08-Four-Fundamental-Subspaces]] (the master theorem)
- **Conceptual**: [[10-Graphs-Networks-Incidence]] (C(A) and N(A) get physical)

## Thematic Summary
Lecture 5 cleans up loose ends (transposes, permutations, symmetry) and introduces vector space, the most abstract object of the course. Lecture 6 hangs two subspaces on every matrix — the *column space* (output range) and the *nullspace* (inputs crushed to zero). These two spaces will dominate the rest of the course: every system solvability question reduces to "is $\mathbf{b}$ in $C(A)$?", every multiplicity-of-solutions question reduces to "what is $N(A)$?".

## Glossary

| Term | Definition |
|------|------------|
| **Transpose ($A^T$)** | Swap rows and columns; $(AB)^T = B^T A^T$. |
| **Symmetric Matrix** | $A = A^T$. Note $R^T R$ is always symmetric. |
| **Vector Space** | A collection closed under addition and scalar multiplication. |
| **Subspace** | A vector space contained within a larger vector space. Must pass through origin. |
| **Column Space $C(A)$** | Linear span of $A$'s columns. Subspace of $\mathbb{R}^m$. |
| **Nullspace $N(A)$** | All $\mathbf{x}$ with $A\mathbf{x} = \mathbf{0}$. Subspace of $\mathbb{R}^n$. |
