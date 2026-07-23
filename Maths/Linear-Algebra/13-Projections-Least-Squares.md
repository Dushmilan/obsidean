---
date: 2026-07-21
type: linear-algebra-cluster
tags: [linear-algebra, strang]
lectures: [15]
prereq_clusters: ["08", "12"]
status: complete
source: manual
---

# 13 — Projections and Least Squares

## Concept Statement
When $A\mathbf{x} = \mathbf{b}$ has no exact solution (data fits don't), replace it with the *closest* projection. The result is the normal equations — the most important formula in data fitting.

## Lecture Sources
- Strang MIT 18.06, Lecture 15: *Projections onto Subspaces*

## Core Material

### The Setting
Bigger system than unknowns ($m \gg n$): more equations than unknowns. With noise, $\mathbf{b} \notin C(A)$. Exact solution impossible. Approximate one is forced.

### Project onto a 1D Line (Spanned by $\mathbf{a}$)

$\mathbf{p} = \hat{x}\mathbf{a}$ where $\mathbf{a} \perp (\mathbf{b} - \hat{x}\mathbf{a})$:
$$\mathbf{a}^T (\mathbf{b} - \hat{x}\mathbf{a}) = 0 \;\Longrightarrow\; \hat{x} = \frac{\mathbf{a}^T \mathbf{b}}{\mathbf{a}^T \mathbf{a}}$$

The 1D projection matrix:
$$P = \frac{\mathbf{a} \mathbf{a}^T}{\mathbf{a}^T \mathbf{a}}$$

### Project onto $C(A)$ (the General Case)

Let $\hat{\mathbf{x}}$ be the best coefficients. The error $\mathbf{e} = \mathbf{b} - A\hat{\mathbf{x}}$ must be orthogonal to $C(A)$:
$$A^T (\mathbf{b} - A\hat{\mathbf{x}}) = \mathbf{0}$$

### The Normal Equations
$$\boxed{A^T A \hat{\mathbf{x}} = A^T \mathbf{b}}$$
The crown jewel formula of least squares. Solvable iff $A^T A$ is invertible (which holds iff $A$ has full column rank).

### The General Projection Matrix $P$
$$\hat{\mathbf{x}} = (A^T A)^{-1} A^T \mathbf{b}, \quad \mathbf{p} = A\hat{\mathbf{x}} \;\Longrightarrow\; \boxed{P = A (A^T A)^{-1} A^T}$$

### Properties of $P$
- $P^T = P$ (symmetric)
- $P^2 = P$ (idempotent — projecting twice does nothing new)

## Cross-Cluster Links
- **Prereq**: [[08-Four-Fundamental-Subspaces]], [[12-Orthogonal-Vectors-Subspaces]]
- **Forward**: future lectures on Gram-Schmidt, QR, applications

## Thematic Summary
Projections show how linear algebra handles *imperfection*. When equations can't be solved exactly, find the projection — the closest vector in the column space to $\mathbf{b}$. The orthogonality criterion (error $\perp$ column space) yields the **normal equations**, the universal tool of data fitting. Every linear regression, system identification, and calibration problem in science reduces to this formula.

## Glossary

| Term | Definition |
|------|------------|
| **Projection ($\mathbf{p}$)** | Closest point in a subspace to a target vector; achieved by orthogonal drop. |
| **Error Vector ($\mathbf{e}$)** | $\mathbf{b} - \mathbf{p}$. Always orthogonal to the projection subspace. |
| **Normal Equations** | $A^T A \hat{\mathbf{x}} = A^T \mathbf{b}$. The least-squares master formula. |
| **Least Squares** | The fitting strategy of minimising $\Vert A\mathbf{x} - \mathbf{b} \Vert^2$. |
| **Idempotent** | $P^2 = P$. Projecting twice is the same as projecting once. |
| **Projection Matrix $P$** | $P = A(A^T A)^{-1}A^T$. Maps any $\mathbf{b}$ to its projection onto $C(A)$. |
