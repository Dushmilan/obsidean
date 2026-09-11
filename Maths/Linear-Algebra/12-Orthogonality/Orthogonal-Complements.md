
## Definition

For a subspace $V$ of $\mathbb{R}^n$, the **orthogonal complement**

$$V^\perp = \{\mathbf{x} : \mathbf{x}^T\mathbf{v} = 0 \text{ for all } \mathbf{v} \in V\}$$

has dimension $n - \dim V$. Every vector splits uniquely as $\mathbf{x} = \mathbf{v} + \mathbf{w}$ with $\mathbf{v} \in V$, $\mathbf{w} \in V^\perp$.

**The four fundamental subspaces form two complementary pairs:**
- $C(A^T)^\perp = N(A)$ and $N(A)^\perp = C(A^T)$
- $C(A)^\perp = N(A^T)$ and $N(A^T)^\perp = C(A)$

## The Intuition

Every room $\mathbb{R}^n$ is the sum of two perpendicular rooms: the row space (useful inputs) and the nullspace (crushed inputs). Anything you can't reach through the useful directions is exactly "perpendicular" to them — no overlap, no gap.

## The Toolkit

| Pair | $\mathbb{R}^n$ | $\mathbb{R}^m$ |
|------|----------------|----------------|
| Complement pair 1 | $C(A^T)$ ↔ $N(A)$ | — |
| Complement pair 2 | — | $C(A)$ ↔ $N(A^T)$ |
| Uniqueness | $\mathbf{x} = \mathbf{v}+\mathbf{w}$, $\mathbf{v}\in V$, $\mathbf{w}\in V^\perp$ | same |

## Derivation

$N(A)$ is the set of vectors orthogonal to every row of $A$; the rows span $C(A^T)$, so $N(A) = C(A^T)^\perp$. The dimension theorem then fixes the complementary dimensions. Uniqueness follows because the two spaces intersect only at $\mathbf{0}$. [Full derivations: Four-Fundamental-Subspaces]

## Method

1. Identify which subspace $\mathbf{b}$ or $\mathbf{x}$ lives in.
2. To find $V^\perp$: solve the equations forcing dot products to zero.
3. Decompose any vector into its $V$ and $V^\perp$ parts (projection).

## Worked Examples

**Setup:** In $\mathbb{R}^3$, $V$ = the plane through the origin with normal $\mathbf{n} = (1,1,1)$. Find $V^\perp$.

**Solution:** $V^\perp$ = line spanned by $\mathbf{n}$: $\{(t,t,t)\}$. Every plane vector is ⊥ $\mathbf{n}$.

**Key insight:** $\dim = 3 - 2 = 1$ — the normal line.

## Common Traps

- $V \cap V^\perp = \{\mathbf{0}\}$ only — they never share nonzero vectors
- "Complement" ≠ "orthogonal complement" — complements partition; orthogonal complements partition *perpendicularly*
- The two pairs live in different rooms ($\mathbb{R}^n$ vs $\mathbb{R}^m$)

## Connections

- Orthogonal-Vectors-and-Subspaces · Four-Fundamental-Subspaces
- Projections — decomposition machinery
