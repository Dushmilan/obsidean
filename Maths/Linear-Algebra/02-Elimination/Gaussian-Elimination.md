
## Definition

The fundamental algorithm: reduce $A$ to **upper triangular** $U$ by forward elimination.

$$E\cdots E_2E_1 A = U$$

**Elementary row operations:** swap rows (permutation $P$), multiply a row by a scalar, and subtract a multiple of one row from another — none change the solution set. The multipliers $l_{ij}$ used to eliminate entry $(i,j)$ become the entries of $L$ in $A = LU$.

## The Intuition

Sorting the equations into a staircase: each pivot steps down and to the right. Once triangular, back-substitution solves it trivially. If a zero appears in a pivot position and no row exchange fixes it, the matrix is **singular** — columns dependent. The failure is a diagnosis, not a bug.

## The Toolkit

| Quantity | Meaning |
|----------|---------|
| Pivot | the nonzero diagonal entry of $U$ at each step |
| Rank $r$ | number of pivots |
| Free variables | $n - r$ |
| Multiplier $l_{ij}$ | factor subtracted from row $i$ |
| Singular | zero pivot unavoidable (dependent columns) |

## Derivation

Row operations correspond to multiplying by elementary matrices $E_{ij}$ (identity plus one off-diagonal entry). Permutations are the matrices $P$. The product of all the $E$'s inverse is $L$ — because inverses of elementary matrices are elementary, the multipliers line up cleanly. [Full derivations: LU-Decomposition]

## Method

1. Forward elimination column by column: make pivots nonzero, eliminate below.
2. Record multipliers; swap rows when a pivot is zero.
3. Stop at triangular $U$; count pivots → rank.

## Worked Examples

**Setup:** $A = \begin{bmatrix}1&2&1\\2&6&1\\1&2&3\end{bmatrix}$. Eliminate.

**Solution:** Row 2 − 2·Row 1: $l_{21} = 2$. Row 3 − 1·Row 1: $l_{31} = 1$. Row 3 − 0·Row 2: $l_{32} = 0$. Result $U = \begin{bmatrix}1&2&1\\0&2&-1\\0&0&2\end{bmatrix}$, rank 3.

**Key insight:** Each elimination step records a multiplier — that's your $L$.

## Common Traps

- Zero pivot with an available row exchange — always try to swap first
- Changing the right-hand side mid-elimination (apply operations to $\mathbf{b}$ too)
- Counting rank wrong when a zero row appears

## Connections

- RREF-Free-Variables-and-Special-Solutions — the next step
- Row-and-Column-Pictures · LU-Decomposition
- Complete-Solution
