
## Definition

The **rank** $r$ of an $m\times n$ matrix $A$ = number of pivots = $\dim C(A) = \dim C(A^T)$. It completely classifies the solution structure of $A\mathbf{x} = \mathbf{b}$:

| Case | Condition | Solutions to $A\mathbf{x} = \mathbf{b}$ |
|------|-----------|----------------------------------------|
| Full rank (square) | $m = n = r$ | exactly 1 |
| Full column rank | $r = n \le m$ | 0 or 1 |
| Full row rank | $r = m \le n$ | ∞ (always consistent) |
| $r < \min(m, n)$ | deficient | 0 or ∞ |

## The Intuition

$r$ counts how many *independent* equations (rows) and unknowns (columns) actually matter. Extra rows that are combinations of others add no new constraints; extra columns that are combinations of others add free variables. Rank is the "effective size" of the matrix.

## The Toolkit

| Quantity | Value |
|----------|-------|
| $\dim C(A)$ | $r$ |
| $\dim N(A)$ | $n - r$ |
| $\dim C(A^T)$ | $r$ |
| $\dim N(A^T)$ | $m - r$ |
| Row rank = column rank | always |

## Derivation

RREF has $r$ pivot rows and $r$ pivot columns — so row rank and column rank are equal. The nullspace dimension $n-r$ counts the free columns. [Full derivations: Four-Fundamental-Subspaces]

## Method

1. Eliminate to RREF; count pivots → $r$.
2. Compare $r$ to $m$ and $n$ → pick the case.
3. Combine with consistency ($\mathbf{b} \in C(A)$) to get the exact count.

## Worked Examples

**Setup:** $A = \begin{bmatrix}1&2\\2&4\end{bmatrix}$. Classify.

**Solution:** Rank $r = 1$ (rows dependent). $m = n = 2 > r$: deficient → 0 or ∞ solutions.

**Key insight:** One independent equation, one free variable.

---

**Setup:** $A = \begin{bmatrix}1&0&2\\0&1&3\end{bmatrix}$. Classify.

**Solution:** $r = 2 = m < n = 3$: full row rank → always consistent, ∞ solutions (one free variable).

**Key insight:** Full row rank guarantees consistency for every $\mathbf{b}$.

## Common Traps

- Rank is the number of *pivots*, not nonzero rows in general form
- Row rank always equals column rank — the "surprising" theorem
- Full row rank ⇒ consistent but not unique; full column rank ⇒ unique but possibly inconsistent

## Connections

- Complete-Solution · Linear-Independence
- Four-Fundamental-Subspaces
