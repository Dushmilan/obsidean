
## Definition

Matrices themselves form **vector spaces** when the set is closed under addition and scalar multiplication. The space $M_{m\times n}$ of all $m\times n$ matrices has dimension $mn$.

| Space | Condition | Dimension |
|-------|-----------|-----------|
| All $m\times n$ matrices | — | $mn$ |
| Symmetric ($n\times n$) | $A = A^T$ | $\frac{n(n+1)}{2}$ |
| Upper triangular | $a_{ij} = 0$ for $i>j$ | $\frac{n(n+1)}{2}$ |
| Diagonal | $a_{ij} = 0$ for $i\neq j$ | $n$ |

## The Intuition

Any set where you can add and scalar-multiply without leaving is a vector space — matrices included. The dimension counts the free parameters: a symmetric matrix is determined by its diagonal and upper triangle.

## The Toolkit

| Space | Free parameters |
|-------|-----------------|
| $M_{m\times n}$ | $mn$ |
| Symmetric | $\frac{n(n+1)}{2}$ |
| Skew-symmetric | $\frac{n(n-1)}{2}$ |
| Upper triangular | $\frac{n(n+1)}{2}$ |

## Derivation

Symmetric: entries $(i,j)$ and $(j,i)$ are tied, so count the diagonal ($n$) plus the strictly upper triangle ($n(n-1)/2$). The intersection of two spaces (e.g., symmetric AND upper triangular = diagonal) still has dimension by the same counting. [Full derivations: Basis-and-Dimension]

## Method

1. Identify the constraints (symmetry, triangularity, trace-zero, etc.).
2. Count free parameters → dimension.
3. Verify closure (the set is a subspace of $M_{m\times n}$).

## Worked Examples

**Setup:** Dimension of the space of $3\times3$ symmetric matrices?

**Solution:** $\frac{3\cdot4}{2} = 6$ free parameters.

**Key insight:** Diagonal (3) + upper triangle (3) = 6.

---

**Setup:** Is the set of invertible matrices a vector space?

**Solution:** No — the zero matrix isn't invertible, and the sum of two invertibles can be singular. Not closed.

**Key insight:** Many "natural" matrix sets aren't subspaces — always check closure.

## Common Traps

- Invertible matrices aren't a subspace (no zero, no closure)
- Symmetric + triangular interplay: the intersection is diagonal ($n$ dim)
- A subspace must contain the zero matrix

## Connections

- Rank-One-Matrices · Basis-and-Dimension
- Four-Fundamental-Subspaces
