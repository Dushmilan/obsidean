---
date: 2026-07-21
type: linear-algebra-cluster
tags: [linear-algebra, strang]
lectures: [8]
prereq_clusters: ["02"]
status: complete
source: manual
---

# 06 — Complete Solutions and the Rank Cases

## Concept Statement
Solve $A\mathbf{x} = \mathbf{b}$ in full generality: a particular solution plus any nullspace vector. Categorise systems by rank to predict **how many** solutions exist.

## Lecture Sources
- Strang MIT 18.06, Lecture 8: *Solving $A\mathbf{x} = \mathbf{b}$ — Row Reduced Form $R$*

## Core Material

### Solubility
$A\mathbf{x} = \mathbf{b}$ has a solution iff $\mathbf{b} \in C(A)$. Eliminating on $[A \mid \mathbf{b}]$ confirms: zeros on the RHS wherever a row of $A$ becomes zero.

### Particular Solution $\mathbf{x}_p$
- Set **all** free variables to $0$ in $R\mathbf{x} = \mathbf{c}$.
- Read pivot variables directly off.
- $A\mathbf{x}_p = \mathbf{b}$ exactly.

### Complete Solution Structure
$$\mathbf{x}_\text{complete} = \mathbf{x}_p + c_1 \mathbf{x}_{s1} + c_2 \mathbf{x}_{s2} + \dots + c_{n-r} \mathbf{x}_{s(n-r)}$$
The general solution is a particular solution plus arbitrary linear combination of special solutions.

$\mathbf{x}_p$ **translates** the nullspace away from the origin — the complete-solution set is an **affine** line/plane/hyperplane through $\mathbf{x}_p$, parallel to $N(A)$.

### Four Rank Cases

For $m \times n$ matrix $A$ of rank $r$:

| Case | Shape | $r$ | RREF $= R$ | Solutions to $A\mathbf{x}=\mathbf{b}$ |
|------|-------|-----|------------|------|
| **Full rank** | $m = n$ | $r = m = n$ | $I$ | exactly 1, for every $\mathbf{b}$ |
| **Full column rank** | $m > n$ | $r = n$ | $\begin{bmatrix} I \\ 0 \end{bmatrix}$ | 0 or 1 |
| **Full row rank** | $m < n$ | $r = m$ | $\begin{bmatrix} I & F \end{bmatrix}$ | infinitely many, for every $\mathbf{b}$ |
| **Defective** | any | $r < m, r < n$ | $\begin{bmatrix} I & F \\ 0 & 0 \end{bmatrix}$ | 0 or infinitely many |

## Cross-Cluster Links
- **Prereq**: [[02-Elimination-and-RREF]] (RREF machinery)
- **Forward**: [[07-Independence-Basis-Dimension]] (formalises $r$, $n-r$)
- **Geometry**: [[08-Four-Fundamental-Subspaces]] ($C(A)$ dimension = $r$)

## Thematic Summary
Once elimination yields RREF, solutions reveal themselves as a translation of the nullspace. The four-rank-cases panel is the executive summary of "what does $A\mathbf{x} = \mathbf{b}$ look like?" — all four combinations of shape and solvability fit on one screen.

## Glossary

| Term | Definition |
|------|------------|
| **Particular Solution ($\mathbf{x}_p$)** | Any specific solution to $A\mathbf{x} = \mathbf{b}$; usually found by setting free vars to 0. |
| **Affine Set** | A translate of a subspace: $\mathbf{x}_p + N(A)$. Not itself a subspace (origin not in it unless $\mathbf{x}_p = 0$). |
| **Augmented Matrix $[A \mid \mathbf{b}]$** | $A$ with an extra column $\mathbf{b}$ glued on for simultaneous elimination. |
