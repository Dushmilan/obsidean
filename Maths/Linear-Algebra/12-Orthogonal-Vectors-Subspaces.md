---
date: 2026-07-21
type: linear-algebra-cluster
tags: [linear-algebra, strang]
lectures: [14]
prereq_clusters: ["08"]
status: complete
source: manual
---

# 12 — Orthogonal Vectors and Subspaces

## Concept Statement
Install right-angle geometry into vector spaces via the dot product. Reveal the second half of the Fundamental Theorem: the four subspaces pair up into two **orthogonal complements**.

## Lecture Sources
- Strang MIT 18.06, Lecture 14: *Orthogonal Vectors and Subspaces*

## Core Material

### Orthogonality of Vectors
$\mathbf{x} \perp \mathbf{y}$ iff $\mathbf{x}^T \mathbf{y} = 0$.

**Pythagorean proof** — if they're perpendicular:
$$\Vert \mathbf{x} \Vert^2 + \Vert \mathbf{y} \Vert^2 = \Vert \mathbf{x} + \mathbf{y} \Vert^2$$
Expanding RHS: $\mathbf{x}^T\mathbf{x} + \mathbf{y}^T\mathbf{y} + \mathbf{x}^T\mathbf{y} + \mathbf{y}^T\mathbf{x}$. Equating forces $\mathbf{x}^T\mathbf{y} = 0$.

**Vector length**: $\Vert \mathbf{x} \Vert^2 = \mathbf{x}^T \mathbf{x}$.

### Orthogonal Subspaces
Subspace $V \perp W$ iff every $\mathbf{v} \in V$ is orthogonal to every $\mathbf{w} \in W$.

### The Fundamental Theorem (Part 2)

| Pair | Where | Dimensions |
|------|-------|------------|
| $C(A^T) \perp N(A)$ in $\mathbb{R}^n$ | $r + (n-r) = n$ | full orthogonal split |
| $C(A) \perp N(A^T)$ in $\mathbb{R}^m$ | $r + (m-r) = m$ | full orthogonal split |

If $\mathbf{x} \in N(A)$, it is perpendicular to **every row** of $A$. Hence $N(A)$ is the orthogonal complement of $C(A^T)$.

## Cross-Cluster Links
- **Prereq**: [[08-Four-Fundamental-Subspaces]]
- **Forward**: [[13-Projections-Least-Squares]] (orthogonality becomes the engine of approximation)
- **Visual**: [[10-Graphs-Networks-Incidence]] (now also physical: $\mathbf{e}$ in [[13-Projections-Least-Squares|Projections]] is perpendicular to $C(A)$)

## Thematic Summary
Part 2 of the Fundamental Theorem reveals the geometric punchline of Lecture 10: the four subspaces aren't just dimension-complementary — they're *perpendicular*. The nullspace is locked at $90°$ to the row space. Once orthogonality is established, every subsequent algorithm (Gram-Schmidt, least squares, QR) becomes geometric rather than algebraic.

## Glossary

| Term | Definition |
|------|------------|
| **Orthogonal** | Two vectors are perpendicular; their dot product is zero. |
| **Inner Product ($\mathbf{x}^T\mathbf{y}$)** | Sum of componentwise products. The angle measure for vectors. |
| **Orthogonal Complement** | For subspace $V$ in a larger space, all vectors $\perp$ to every vector in $V$. |
| **Fundamental Theorem (Part 2)** | $N(A) \perp C(A^T)$ in $\mathbb{R}^n$; $N(A^T) \perp C(A)$ in $\mathbb{R}^m$. |
