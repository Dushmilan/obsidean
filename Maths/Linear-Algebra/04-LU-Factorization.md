---
date: 2026-07-21
type: linear-algebra-cluster
tags: [linear-algebra, strang]
lectures: [4]
prereq_clusters: ["02"]
status: complete
source: manual
---

# 04 — $A = LU$ Factorization

## Concept Statement
Recognise that Gaussian elimination's record of action can be collapsed into **two triangular matrices** — a lower-triangular $L$ holding the multipliers and an upper-triangular $U$ holding the pivots.

## Lecture Sources
- Strang MIT 18.06, Lecture 4: *Factorization into $A = LU$*

## Core Material

### The Factorization (no row exchanges)
$$A = LU$$
- $L$ — lower triangular, **1s on the diagonal**, multipliers below diagonal in their exact $l_{ij}$ slots.
- $U$ — upper triangular, pivots on the diagonal.

### Why $L$ Beats the $E$-Product

Elimination matrices $E_{32}E_{31}E_{21}$ applied to $A$ all *interact* — multipliers re-mix when they meet. But their inverses $E_{21}^{-1}E_{31}^{-1}E_{32}^{-1}$ line up *cleanly*: each $l_{ij}$ lands precisely in position $(i,j)$ of $L$. Hence:
$$L = E^{-1} = E_{21}^{-1}E_{31}^{-1}E_{32}^{-1}$$

### When Row Exchanges Are Required
$$PA = LU$$
$P$ is the cumulative record of swaps required to keep pivots non-zero.

### Computational Cost
For an $n \times n$ matrix:
- $A \to U$ ~ $\tfrac{1}{3} n^3$ operations (elimination dominates)
- solve triangular systems ~ $n^2$
- elimination dominates everything else for large $n$

### Alternative: $A = LDU$
When pivots are factored into a separate diagonal matrix $D$, the resulting upper matrix has **1s on the diagonal**, mirroring $L$.
$$A = LDU, \quad D = \text{diag of pivots}$$

## Cross-Cluster Links
- **Prereq**: [[02-Elimination-and-RREF]] (elimination must come first)
- **Forward**: [[05-Transposes-Permutations-Spaces]] (permutation and transpose mechanics)
- **Algorithmic flavour**: [[08-Four-Fundamental-Subspaces]] (decomposing $A$ into pieces)

## Thematic Summary
$L$ and $U$ are the *algebraic shadow* of elimination. By packaging the algorithm into a product, you turn a 30-page derivation into a chain of matrix identities. $A = LU$ is the foundation of every modern numerical linear algebra package: in practice, sparse solvers store $L$ and $U$ to factor once and backsolve many times.

## Glossary

| Term | Definition |
|------|------------|
| **Lower Triangular Matrix ($L$)** | All entries above the diagonal are zero. |
| **Upper Triangular Matrix ($U$)** | All entries below the diagonal are zero. |
| **Permutation Matrix ($P$)** | Identity with rows reordered; $P^T = P^{-1}$. |
| **Diagonal Matrix ($D$)** | All off-diagonal entries are 0. Used in $A = LDU$ to extract pivots. |
