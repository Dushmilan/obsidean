
## Definition

A **basis** of a subspace $V$ is a set of vectors that is **independent and spans** $V$. The **dimension** $\dim V$ is the number of vectors in any basis — invariant (every basis has the same count).

For $m\times n$ rank-$r$ matrix $A$:
- Pivot columns of $A$ (not of $R$!) form a basis of $C(A)$: $\dim = r$.
- Special solutions form a basis of $N(A)$: $\dim = n - r$.
- **Dimension theorem:** $r + (n-r) = n$ — nullspace + column space dimensions fill $\mathbb{R}^n$.

## The Intuition

A committee again: independence = no redundancy, spanning = covers every issue. A basis is the *minimum effective committee*. In $\mathbb{R}^3$: 3 independent vectors span all of it; 2 span a plane; 1 spans a line; 0 spans only the origin.

## The Toolkit

| Space | Basis | Dimension |
|-------|-------|-----------|
| $C(A)$ | pivot columns of $A$ | $r$ |
| $N(A)$ | special solutions | $n - r$ |
| $C(A^T)$ | pivot rows of $R$ | $r$ |
| $N(A^T)$ | from $E$ rows (elimination multipliers) | $m - r$ |

## Derivation

Pivot columns are independent (each has its own pivot) and span $C(A)$ (free columns are combinations). Every basis of $V$ has the same size — the exchange theorem (Steinitz) proves the count is invariant. [Full derivations: Four-Fundamental-Subspaces]

## Method

1. Find a basis: eliminate to RREF, take pivot columns **of the original $A$**.
2. Nullspace basis: one special solution per free variable.
3. Dimension: count basis vectors; verify against $r$ / $n-r$.

## Worked Examples

**Setup:** $A = \begin{bmatrix}1&2&2\\2&4&6\\3&6&8\end{bmatrix}$. Basis of $C(A)$ and $N(A)$?

**Solution:** Pivot columns 1, 3 of $A$: $\{(1,2,3),(2,6,8)\}$, $\dim = 2$. Free column 2: special solution $(-2,1,0)$ spans $N(A)$, $\dim = 1$.

**Key insight:** $2 + 1 = 3 = n$ — the dimension theorem in action.

## Common Traps

- Basing $C(A)$ on the pivot columns of **$R$**, not $A$ — the columns of $R$ are different vectors
- $\dim C(A) = r$ and $\dim N(A) = n - r$ — they live in different rooms
- "Basis" requires BOTH independence and spanning — one without the other isn't enough

## Connections

- Linear-Independence · Four-Fundamental-Subspaces
- Incidence-Matrices — dimensions of graph spaces
