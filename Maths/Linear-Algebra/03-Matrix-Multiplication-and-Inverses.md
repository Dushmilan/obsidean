---
date: 2026-07-21
type: linear-algebra-cluster
tags: [linear-algebra, strang]
lectures: [3]
prereq_clusters: ["01"]
status: complete
source: manual
---

# 03 — Matrix Multiplication and Inverses

## Concept Statement
Multiplying matrices under four equivalent mental models (dot, column, row, block) and recognising that an inverse $A^{-1}$ is a *record of elimination run in reverse*.

## Lecture Sources
- Strang MIT 18.06, Lecture 3: *Multiplication and Inverse Matrices*

## Core Material

### Four Ways to Compute $AB = C$

Let $A$ be $m \times n$ and $B$ be $n \times p$. Result $C$ is $m \times p$.

**1. Dot-product way** — entry $(i,j)$ of $C$ = (row $i$ of $A$)·(column $j$ of $B$).

**2. Column way** — column $j$ of $C$ = linear combination of columns of $A$, using entries of column $j$ of $B$ as weights.

**3. Row way** — row $i$ of $C$ = linear combination of rows of $B$, using entries of row $i$ of $A$ as weights.

**4. Block way** — partition $A$ and $B$ into conformable sub-blocks and apply the usual rules as if the blocks were scalars.

### Inverses

A *square* matrix $A$ is **invertible** iff there exists $A^{-1}$ with
$$A^{-1}A = I, \qquad AA^{-1} = I.$$

**Singular ↔ columns dependent** — $A\mathbf{x} = \mathbf{0}$ has a non-trivial $\mathbf{x}$ iff $A$ is singular. Equivalently: $A$'s columns lie in a degenerate subspace.

**Inverse of a product reverses order:**
$$(AB)^{-1} = B^{-1}A^{-1}$$

### Gauss-Jordan — Inversion by Elimination

Form $[A \mid I]$. Run elimination until $A$ becomes $I$. The right side has become $A^{-1}$:
$$[A \mid I] \;\longrightarrow\; [I \mid A^{-1}]$$

### The Identity Matrix $I$

$I$ plays the role of the number 1: $AI = A$, $IA = A$, $A^{-1}A = I$. Diagonal entries are 1; off-diagonal are 0.

## Cross-Cluster Links
- **Prereq**: [[01-Linear-Systems-and-Axb]]
- **Companion**: [[02-Elimination-and-RREF]] (Gauss-Jordan uses RREF machinery)
- **Forward**: [[04-LU-Factorization]] (elimination matrices beg a cleaner product form)

## Thematic Summary
Strang gives matrix multiplication the same "three views" treatment he applies to $A\mathbf{x}=\mathbf{b}$. The product $AB$ can be computed row-wise, column-wise, or with blocks — the answer is the same. Inverses enter as the existence question for the equation $A\mathbf{x} = \mathbf{b}$: when $\mathbf{b}$ is free to roam (any $\mathbf{b}$ admit a solution) and $A$ is square, $A^{-1}$ exists and the linear system becomes unique multiplication.

## Glossary

| Term | Definition |
|------|------------|
| **Identity Matrix ($I$)** | Square matrix with 1s on the main diagonal and 0s elsewhere. Neutral element of multiplication. |
| **Inverse ($A^{-1}$)** | Square matrix satisfying $A^{-1}A = AA^{-1} = I$. Undoer of $A$. |
| **Singular Matrix** | Square $A$ with no $A^{-1}$. Determined by det$(A) = 0$ or some $A\mathbf{x} = \mathbf{0}$, $\mathbf{x} \neq \mathbf{0}$. |
| **Gauss-Jordan Elimination** | Elimination continued past RREF, paired with a simultaneous homogenising reduction on $[A \mid I]$. |
