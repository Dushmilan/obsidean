---
date: 2026-07-21
type: reference
tags: [linear-algebra, strang, formula-sheet]
---

# Formula Sheet — Linear Algebra (L1–L15)

## Notation

| Symbol | Meaning |
|--------|---------|
| $A$ | $m \times n$ matrix |
| $\mathbf{x}$, $\mathbf{b}$, $\mathbf{y}$ | column vectors |
| $A^T$ | transpose |
| $A^{-1}$ | inverse |
| $r$ | rank of $A$ |
| $N(A)$ | nullspace of $A$ |
| $C(A)$ | column space of $A$ |
| $P$ | projection matrix |

## Fundamental Identities

| Identity |
|----------|
| $A\mathbf{x} = \mathbf{b}$ |
| $A = LU$ |
| $A = LDU$ |
| $PA = LU$ |
| $A^T A$ symmetric (always) |
| $P^T P = I \Rightarrow P^T = P^{-1}$ |
| $(AB)^{-1} = B^{-1} A^{-1}$ |
| $(AB)^T = B^T A^T$ |

## Four Subspaces

| Subspace | $\mathbb{R}^?$ | Dim |
|----------|-----|-----|
| $C(A)$ | $\mathbb{R}^m$ | $r$ |
| $N(A)$ | $\mathbb{R}^n$ | $n-r$ |
| $C(A^T)$ | $\mathbb{R}^n$ | $r$ |
| $N(A^T)$ | $\mathbb{R}^m$ | $m-r$ |

$N(A) \perp C(A^T)$ in $\mathbb{R}^n$. $N(A^T) \perp C(A)$ in $\mathbb{R}^m$.

## Rank Cases

| Case | $r$ | Solutions |
|------|-----|-----------|
| $m = n$, $r = m = n$ | full | unique |
| $r = n < m$ | full column | 0 or 1 |
| $r = m < n$ | full row | $\infty$ |
| $r < m$, $r < n$ | defect | 0 or $\infty$ |

## Solution Structure

For non-defective $A\mathbf{x} = \mathbf{b}$:
$$\mathbf{x} = \mathbf{x}_p + c_1\mathbf{x}_{s1} + \dots + c_{n-r}\mathbf{x}_{s(n-r)}$$

## Projection

| Formula |
|---------|
| $\hat{x} = \dfrac{\mathbf{a}^T\mathbf{b}}{\mathbf{a}^T\mathbf{a}}$ |
| $P_\text{line} = \dfrac{\mathbf{a}\mathbf{a}^T}{\mathbf{a}^T\mathbf{a}}$ |
| $A^T A \hat{\mathbf{x}} = A^T\mathbf{b}$ (Normal Eqns) |
| $P_{C(A)} = A(A^T A)^{-1} A^T$ |

## Properties of $P$

- $P^T = P$ (symmetric)
- $P^2 = P$ (idempotent)

## Orthogonality

- $\mathbf{x} \perp \mathbf{y} \iff \mathbf{x}^T\mathbf{y} = 0$
- $\Vert\mathbf{x}\Vert^2 = \mathbf{x}^T\mathbf{x}$

## Rank-1 Factorisation

$$A \text{ rank 1} \iff A = \mathbf{u}\mathbf{v}^T$$

---

*Source: L1–L15 Strang MIT 18.06*
