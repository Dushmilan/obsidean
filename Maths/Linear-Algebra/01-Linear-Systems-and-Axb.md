---
date: 2026-07-21
type: linear-algebra-cluster
tags: [linear-algebra, strang]
lectures: [1]
prereq_clusters: []
status: complete
source: manual
---

# 01 — Linear Systems and the Meaning of $A\mathbf{x} = \mathbf{b}$

## Concept Statement
Master the three lenses through which a single matrix equation is viewed: row-by-row, column-by-column, and as a box multiplying a vector. Stratify which lens reveals which fact.

## Lecture Sources
- Strang MIT 18.06, Lecture 1: *The Geometry of Linear Equations*

## Core Material

### The Equation Itself
$$A\mathbf{x} = \mathbf{b}$$
- $A$ — coefficient matrix, dimensions $m \times n$
- $\mathbf{x} \in \mathbb{R}^n$ — column vector of unknowns
- $\mathbf{b} \in \mathbb{R}^m$ — column vector of constants

### Three Lenses

**Row Picture** — solve equation-by-equation. Each row of $A$ defines a hyperplane in $\mathbb{R}^n$. The solution lies at the simultaneous intersection of all hyperplanes.
- 2D: two **lines** intersect at a point.
- 3D: three **planes** intersect at a point.

**Column Picture** (Strang's preferred) — view $A\mathbf{x}$ as a *combination* of $A$'s columns:
$$x_1 \mathbf{a}_1 + x_2 \mathbf{a}_2 + \cdots + x_n \mathbf{a}_n = \mathbf{b}$$
The question becomes: *which weights $x_1, \dots, x_n$ assemble the columns into $\mathbf{b}$?*

**Matrix Picture** — $A$ is a single operator acting on $\mathbf{x}$. The same equation, abstracted from row/column detail.

### Why the Column Picture Wins
The column picture exposes that $\mathbf{b}$ must lie in the *span of the columns of $A$*. The row picture hides this insight until equations are solved.

## Cross-Cluster Links
- **Next**: [[02-Elimination-and-RREF]] (solving by row operations)
- **Forward**: [[05-Transposes-Permutations-Spaces]] (column space $C(A)$ formalised)
- **Operator lens**: [[03-Matrix-Multiplication-and-Inverses]] (multiplication as an operation)

## Thematic Summary
Strang's first move is philosophical: refuse the row picture's century-old dominance and reframe every equation as a *combination problem*. This reframing is the bedrock of the course — once $A\mathbf{x} = \mathbf{b}$ is recognised as "find weights that build $\mathbf{b}$ from columns," every later concept (column space, rank, basis, projection) acquires geometric meaning.

## Glossary

| Term | Definition |
|------|------------|
| **Linear Combination** | A sum $x_1 \mathbf{v}_1 + \cdots + x_k \mathbf{v}_k$ of vectors weighted by scalars. The column picture lives here. |
| **Coefficient Matrix ($A$)** | The $m \times n$ grid of numerical weights that links unknowns $\mathbf{x}$ to constants $\mathbf{b}$. |
| **Singular Matrix** | A square $A$ with linearly dependent columns; no $A^{-1}$ exists; column picture collapses onto a lower-dim set. |
