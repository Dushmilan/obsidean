
## Definition

Two vectors are **orthogonal** if $\mathbf{x}^T\mathbf{y} = 0$. Two subspaces $V, W$ are **orthogonal complements** if every vector of $V$ is ⊥ every vector of $W$ and they span all of the ambient space.

**Fundamental Theorem Part 2:** $C(A^T) \perp N(A)$ in $\mathbb{R}^n$ and $C(A) \perp N(A^T)$ in $\mathbb{R}^m$.

**Pythagoras:** $\mathbf{x} \perp \mathbf{y} \Rightarrow \|\mathbf{x}\|^2 + \|\mathbf{y}\|^2 = \|\mathbf{x}+\mathbf{y}\|^2$.

## The Intuition

Orthogonal vectors are coordinate axes — independent directions. In $\mathbb{R}^3$, the $xy$-plane is the orthogonal complement of the $z$-axis: every vector in the plane is ⊥ every vector on the axis.

## The Toolkit

| Fact | Statement |
|------|-----------|
| Orthogonality | $\mathbf{x}^T\mathbf{y} = 0$ |
| Complement | $\dim V^\perp = n - \dim V$ |
| Decomposition | every $\mathbf{x} = \mathbf{v} + \mathbf{w}$, $\mathbf{v}\in V$, $\mathbf{w}\in V^\perp$, unique |
| Row ⊥ null | $C(A^T) \perp N(A)$ |
| Column ⊥ left null | $C(A) \perp N(A^T)$ |

## Derivation

If $\mathbf{x} \in N(A)$ then $A\mathbf{x} = \mathbf{0}$, so $\mathbf{x}$ is orthogonal to every row of $A$ — hence to all of $C(A^T)$. The dimension counting gives the complement property: $\dim C(A^T) + \dim N(A) = r + (n-r) = n$. [Full derivations: Four-Fundamental-Subspaces]

## Method

1. Orthogonality test: dot product = 0.
2. Given a subspace, its complement: find vectors ⊥ to all of it.
3. Use the row/null orthogonal pairs to decompose $\mathbb{R}^n$.

## Worked Examples

**Setup:** $N(A)$ and $C(A^T)$ for $A = \begin{bmatrix}1&2\\2&4\end{bmatrix}$.

**Solution:** $N(A) = \text{span}(-2,1)$; $C(A^T) = \text{span}(1,2)$. Dot: $-2+2 = 0$ — orthogonal ✓.

**Key insight:** Orthogonal pairs split the room exactly.

## Common Traps

- Orthogonal ≠ independent — orthogonal vectors are independent, but not conversely
- A subspace and its complement *fill* the space (dimensions add to $n$)
- $\mathbf{x} \perp \mathbf{y}$ needs $\mathbf{x}^T\mathbf{y} = 0$, not just "visually perpendicular"

## Connections

- Orthogonal-Complements · Four-Fundamental-Subspaces
- Projections — the next geometric step
- [[12-Orthogonality/Orthogonal-Vectors-and-Subspaces]]
