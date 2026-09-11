
## Definition

A rank-1 matrix is an **outer product**:

$$A = \mathbf{u}\mathbf{v}^T \quad (m\times n), \qquad a_{ij} = u_i v_j$$

Every column is a multiple of $\mathbf{u}$; every row is a multiple of $\mathbf{v}^T$. Rank is 1 (unless $\mathbf{u}$ or $\mathbf{v}$ is zero).

**Every rank-$r$ matrix is a sum of $r$ rank-1 matrices** — the foundation of the SVD.

## The Intuition

A rank-1 matrix is a single brushstroke — one direction of variation. A rank-$r$ matrix is $r$ brushstrokes layered. If you know one column and one row, the whole matrix is determined: every entry is their product, with no extra freedom.

## The Toolkit

| Fact | Statement |
|------|-----------|
| Outer product | $\mathbf{u}\mathbf{v}^T$ has rank ≤ 1 |
| Decomposition | $A = \sum_{i=1}^r \mathbf{u}_i\mathbf{v}_i^T$ |
| Entry formula | $a_{ij} = u_iv_j$ |
| Column/row structure | all columns ∥ $\mathbf{u}$, all rows ∥ $\mathbf{v}^T$ |
| SVD connection | $A = U\Sigma V^T = \sum \sigma_i \mathbf{u}_i\mathbf{v}_i^T$ |

## Derivation

$\text{rank}(\mathbf{u}\mathbf{v}^T) = 1$: the column space is spanned by $\mathbf{u}$. Any rank-$r$ matrix has $r$ independent columns; writing each column's contribution from the row space decomposition gives the rank-1 sum. [Full derivations: Four-Fundamental-Subspaces]

## Method

1. Rank-1 test: is every column a multiple of one vector?
2. To express as $\mathbf{u}\mathbf{v}^T$: pick $\mathbf{u}$ = first column, solve for $\mathbf{v}$.
3. For a general matrix, peel off rank-1 layers (SVD/later lectures).

## Worked Examples

**Setup:** Write $\begin{bmatrix}1&2&3\\2&4&6\end{bmatrix}$ as $\mathbf{u}\mathbf{v}^T$.

**Solution:** $\mathbf{u} = (1,2)^T$, $\mathbf{v} = (1,2,3)^T$ — check: $u_iv_j$ gives the entries.

**Key insight:** Rows are multiples of $(1,2,3)$; columns are multiples of $(1,2)$.

---

**Setup:** Why does $\begin{bmatrix}1&0\\0&1\end{bmatrix}$ need two rank-1 pieces?

**Solution:** Rank 2: $I = \begin{bmatrix}1\\0\end{bmatrix}(1,0) + \begin{bmatrix}0\\1\end{bmatrix}(0,1)$.

**Key insight:** Rank counts the minimum number of brushstrokes.

## Common Traps

- Outer product order: $\mathbf{u}\mathbf{v}^T$ is $m\times n$; $\mathbf{v}\mathbf{u}^T$ is $n\times m$
- Rank 0 if either vector is zero
- A rank-1 matrix has *both* row and column spaces 1-dimensional

## Connections

- Matrix-Spaces · Least-Squares
- Four-Fundamental-Subspaces — the row/column structure
- Future: SVD ($\Sigma$ = rank-1 weights)
