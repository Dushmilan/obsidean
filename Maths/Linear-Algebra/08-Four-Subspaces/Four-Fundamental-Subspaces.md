
## Definition

Every $m\times n$ rank-$r$ matrix defines four subspaces:

| Space | Lives in | Dimension | Meaning |
|-------|----------|-----------|---------|
| $C(A)$ column space | $\mathbb{R}^m$ | $r$ | reachable outputs |
| $N(A)$ nullspace | $\mathbb{R}^n$ | $n-r$ | inputs crushed to zero |
| $C(A^T)$ row space | $\mathbb{R}^n$ | $r$ | what $A$ "sees" |
| $N(A^T)$ left nullspace | $\mathbb{R}^m$ | $m-r$ | observers seeing no effect |

**Orthogonality:** $C(A^T) \perp N(A)$ in $\mathbb{R}^n$; $C(A) \perp N(A^T)$ in $\mathbb{R}^m$.

## The Intuition

$A$ is a machine with an input room ($\mathbb{R}^n$) and output room ($\mathbb{R}^m$). Inputs split into "useful" (row space) and "wasted" (nullspace); outputs split into "reachable" (column space) and "unreachable" (left nullspace). The rank $r$ bridges both rooms: $r + (n-r) = n$, $r + (m-r) = m$.

## The Toolkit

| Space | Basis from | Dimension |
|-------|-----------|-----------|
| $C(A)$ | pivot columns of $A$ | $r$ |
| $C(A^T)$ | pivot rows of $R$ | $r$ |
| $N(A)$ | special solutions | $n-r$ |
| $N(A^T)$ | last $m-r$ rows of $E$ | $m-r$ |

## Derivation

Row rank = column rank follows from RREF having equal pivot rows and columns. Orthogonality: any $\mathbf{x} \in N(A)$ satisfies $A\mathbf{x} = \mathbf{0}$, so $\mathbf{x}$ is perpendicular to every row of $A$ — hence to $C(A^T)$. The left nullspace is the span of the "failure rows" produced by elimination ($0 = \text{nonzero}$). [Full derivations: Basis-and-Dimension]

## Method

1. Eliminate to $R$; identify pivot rows/columns.
2. Read bases: pivot columns of $A$; pivot rows of $R$; special solutions; leftover rows of $E$.
3. Verify dimensions: $r$, $r$, $n-r$, $m-r$ — sum checks each room.

## Worked Examples

**Setup:** $A = \begin{bmatrix}1&2\\3&6\\4&8\end{bmatrix}$. Describe the four subspaces.

**Solution:** Rank 1. $C(A)$: line through $(1,3,4)$ in $\mathbb{R}^3$. $N(A)$: line through $(-2,1)$ in $\mathbb{R}^2$. $C(A^T)$: line through $(1,2)$ in $\mathbb{R}^2$. $N(A^T)$: plane in $\mathbb{R}^3$ ⊥ $(1,3,4)$.

**Key insight:** $r=1$: $1+1=2$ in $\mathbb{R}^2$, $1+2=3$ in $\mathbb{R}^3$.

## Common Traps

- Which room each space lives in ($\mathbb{R}^m$ vs $\mathbb{R}^n$)
- Column-space basis from $R$ instead of $A$
- Forgetting $N(A^T)$ (the fourth, often missed space)
- Orthogonality pairs: row space ⟂ nullspace, column space ⟂ left nullspace

## Connections

- Orthogonal-Complements (cluster 12) · Basis-and-Dimension
- Incidence-Matrices — physical meaning of $N(A^T)$
- Least-Squares — column space is where $\hat{\mathbf{b}}$ lives
