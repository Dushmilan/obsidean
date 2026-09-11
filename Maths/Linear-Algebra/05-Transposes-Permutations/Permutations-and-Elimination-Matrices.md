
## Definition

- **Permutation matrix $P$:** a reordered identity. Applying $P$ swaps rows (or columns) of a matrix. There are $n!$ of them in $\mathbb{R}^{n\times n}$.
- **Elimination matrix $E_{ij}$:** identity plus one off-diagonal entry; multiplying by $E$ subtracts a multiple of one row from another.
- **Combined:** Gaussian elimination with row exchanges is $PA = LU$.

## The Intuition

$P$ is a "shuffle" operator; $E$ is a single "subtract" step. Every elimination algorithm is a sequence of $E$'s (and $P$'s when pivots fail) — matrix multiplication just applies them left-to-right.

## The Toolkit

| Object | Action | Inverse |
|--------|--------|---------|
| $E_{ij}$ | subtract $l$ × row $j$ from row $i$ | $E_{ij}^{-1}$ = same with $+l$ |
| $P$ | swap rows | $P^{-1} = P^T$ |
| $PA = LU$ | eliminate with swaps | $A = P^{-1}LU$ |

## Derivation

Each row operation is a left-multiplication by an elementary matrix; composing them gives $E\cdots E_2E_1 A = U$. Swapping rows is multiplying by $P$. The inverses are elementary too, which is why $L$ (the product of inverse-$E$'s) stays lower triangular. [Full derivations: Gaussian-Elimination]

## Method

1. Identify each elimination step as an $E$; record multipliers in $L$.
2. When a pivot is zero, insert a $P$; track $PA = LU$.
3. To reverse an operation, use the inverse ($E^{-1}$ or $P^T$).

## Worked Examples

**Setup:** Write "subtract 2× row 1 from row 2" as an elementary matrix.

**Solution:** $E_{21} = \begin{bmatrix}1&0&0\\-2&1&0\\0&0&1\end{bmatrix}$; its inverse $= \begin{bmatrix}1&0&0\\2&1&0\\0&0&1\end{bmatrix}$.

**Key insight:** $E$ records the *operation*; $E^{-1}$ undoes it.

---

**Setup:** Permutation swapping rows 1 and 2 of a $2\times2$ matrix.

**Solution:** $P = \begin{bmatrix}0&1\\1&0\end{bmatrix}$; $P^2 = I$ and $P^{-1} = P$.

**Key insight:** Swapping twice returns to the start — $P$ is its own inverse here.

## Common Traps

- Order of multiplication — $PE$ vs $EP$ do different things
- Forgetting $P$ in $PA = LU$ when pivots swap
- A permutation's inverse is its transpose, not itself (except for single swaps)

## Connections

- Transposes-and-Symmetric-Matrices · Gaussian-Elimination
- LU-Decomposition — $PA = LU$
