
## Definition

The transpose $A^T$ swaps rows and columns of $A$ ($m\times n$ → $n\times m$).

- **Reverses products:** $(AB)^T = B^TA^T$.
- **Symmetric:** $A^T = A$ — always true of $R^TR$ for any $R$.
- **Skew-symmetric:** $A^T = -A$.

**Permutation matrices** $P$ are the simplest orthogonal matrices ($P^T = P^{-1}$) — every row swap is one, and they're the building blocks of elimination with exchanges.

## The Intuition

$R^TR$ is symmetric because it "multiplies a pattern against itself" — the $(i,j)$ and $(j,i)$ entries compute the same dot product. Symmetric matrices are everywhere in least squares and physics (stiffness, covariance, adjacency).

## The Toolkit

| Property | Rule |
|----------|------|
| Transpose of product | $(AB)^T = B^TA^T$ |
| Transpose of inverse | $(A^{-1})^T = (A^T)^{-1}$ |
| $R^TR$ | always symmetric (and positive semidefinite) |
| Permutation | $P^T = P^{-1}$, $\det P = \pm1$ |
| Symmetric diagonalisation | $A = Q\Lambda Q^T$ (later) |

## Derivation

$(AB)^T_{ij} = (AB)_{ji} = \sum_k A_{jk}B_{ki} = \sum_k (B^T)_{ik}(A^T)_{kj} = (B^TA^T)_{ij}$. The symmetry of $R^TR$ follows from $(R^TR)^T = R^T(R^T)^T = R^TR$. [Full derivations: Four-Ways-to-Multiply]

## Method

1. To transpose a product: reverse the order — "socks then shoes, undo shoes then socks".
2. Symmetric test: compare $a_{ij}$ with $a_{ji}$.
3. $R^TR$ appears in least squares — expect symmetry.

## Worked Examples

**Setup:** $A = \begin{bmatrix}1&3\\2&4\\5&6\end{bmatrix}$. Show $A^TA$ is symmetric.

**Solution:** $A^TA = \begin{bmatrix}1&2&5\\3&4&6\end{bmatrix}\begin{bmatrix}1&3\\2&4\\5&6\end{bmatrix} = \begin{bmatrix}30&41\\41&61\end{bmatrix}$ — symmetric.

**Key insight:** $A^TA$ is always square and symmetric — the engine of least squares.

## Common Traps

- $(AB)^T = B^TA^T$, not $A^TB^T$
- $A^T \neq A$ unless $A$ is symmetric
- A permutation matrix squared can be $I$ (swapping twice)

## Connections

- Permutations-and-Elimination-Matrices · Inverses-and-Gauss-Jordan
- Least-Squares — where $A^TA$ rules
- Four-Fundamental-Subspaces — row space = $C(A^T)$
