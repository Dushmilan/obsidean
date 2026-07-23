---
date: 2026-07-21
type: linear-algebra-cluster
tags: [linear-algebra, strang]
lectures: [9]
prereq_clusters: ["05"]
status: complete
source: manual
---

# 07 — Independence, Basis, Dimension

## Concept Statement
Define what makes a set of vectors "minimal enough" to describe a space: linearly independent, spanning, hence a basis of definite cardinality — the dimension.

## Lecture Sources
- Strang MIT 18.06, Lecture 9: *Independence, Basis, and Dimension*

## Core Material

### Linear Independence
$\mathbf{x}_1, \dots, \mathbf{x}_k$ are **linearly independent** iff
$$c_1 \mathbf{x}_1 + \cdots + c_k \mathbf{x}_k = \mathbf{0} \implies c_1 = \cdots = c_k = 0$$

Equivalently: no vector is expressible as a linear combination of the others.

For $A$' columns to be linearly independent: $N(A) = \{\mathbf{0}\}$ and rank $r = k$.

### Spanning
$\mathbf{v}_1, \dots, \mathbf{v}_l$ **span** a space $V$ iff every vector in $V$ is a linear combination of those $\mathbf{v}_k$.

### Basis
A set that is **both** linearly independent **and** spanning.

- Provides the *minimum number of vectors* needed to describe the space.
- For an $m \times n$ matrix $A$ of rank $r$, the $r$ pivot columns of $A$ form a basis for $C(A)$.

### Dimension
The number of vectors in any basis. Invariant: *every* basis of a space has the same cardinality.

| Subspace | Dimension |
|----------|-----------|
| $C(A)$ | $r$ |
| $N(A)$ | $n - r$ |

## Cross-Cluster Links
- **Prereq**: [[05-Transposes-Permutations-Spaces]] (subspace machinery)
- **Forward**: [[08-Four-Fundamental-Subspaces]] (combines $C(A)$ and $N(A)$ dimension counts with their dual subspaces)
- **Applications**: [[10-Graphs-Networks-Incidence]] (independent loops)

## Thematic Summary
Independence, basis, and dimension are three faces of the same idea: **how much** of a subspace exists. Linear independence is the absence of redundancy; spanning is the property of completeness; basis is the union of both; dimension is the count. With these three definitions, every result about a matrix can be phrased as a count of independent components.

## Glossary

| Term | Definition |
|------|------------|
| **Linear Independence** | $c_1 \mathbf{v}_1 + \dots + c_k \mathbf{v}_k = \mathbf{0}$ forces all $c_i = 0$. |
| **Spanning Set** | A set $\mathbf{v}_1, \dots, \mathbf{v}_l$ such that every vector in the space is a linear combination of them. |
| **Basis** | A spanning, linearly independent set. The *minimum* description of a space. |
| **Dimension** | The cardinality of any basis of a space. Equivalent definitions (cardinality of any spanning set $=$ cardinality of any basis) hold. |
