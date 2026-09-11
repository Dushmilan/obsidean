
## Definition

$A\mathbf{x} = \mathbf{b}$ is **solvable iff $\mathbf{b}$ lies in the column space** $C(A)$ — the span of $A$'s columns:

$$\mathbf{b} \in C(A) \iff \exists \mathbf{x} \text{ with } A\mathbf{x} = \mathbf{b}$$

When $A$ is square, "$\mathbf{b}$ in $C(A)$" ⟺ "$\mathbf{b}$ in $\mathbb{R}^n$" ⟺ "$A$ invertible". When $m \neq n$, $C(A)$ is a proper subspace of $\mathbb{R}^m$ and most $\mathbf{b}$ are unreachable.

## The Intuition

A recipe question: can the available ingredients (columns) be combined to make dish $\mathbf{b}$? If the columns only span a line (all multiples of one another), you can only make dishes on that line — everything else is impossible, no matter how you mix.

## The Toolkit

| Condition | Meaning |
|-----------|---------|
| $\mathbf{b} \in C(A)$ | solvable |
| $\mathbf{b} \notin C(A)$ | inconsistent |
| $C(A) = \mathbb{R}^m$ | solvable for every $\mathbf{b}$ (full row rank) |
| $A$ square, invertible | unique solution for every $\mathbf{b}$ |
| Columns dependent | $C(A)$ is a proper subspace — most $\mathbf{b}$ fail |

## Derivation

$C(A)$ is by definition $\{A\mathbf{x} : \mathbf{x} \in \mathbb{R}^n\}$ — the set of reachable $\mathbf{b}$. Solvability is exactly membership. The row picture gives the same answer: inconsistent equations are contradictory hyperplanes, which happens when a row of elimination produces $0 = \text{nonzero}$. [Full derivations: Gaussian-Elimination]

## Method

1. Put the system in $A\mathbf{x} = \mathbf{b}$ form.
2. Check rank: the number of independent columns (pivots) tells you $\dim C(A)$.
3. If $\dim C(A) < m$, test whether $\mathbf{b}$ is in the span (e.g. via the consistency condition from elimination).

## Worked Examples

**Setup:** $x + 2y + 3z = 6$; $2x + 4y + 6z = 12$.

**Solution:** Columns $(1,2), (2,4), (3,6)$ are all multiples — they span a line. Both equations are consistent (the second is $2\times$ the first): infinitely many solutions along a line.

**Key insight:** The column picture reveals dependency instantly.

---

**Setup:** $\begin{bmatrix}1&2\\2&4\end{bmatrix}\mathbf{x} = \begin{bmatrix}3\\5\end{bmatrix}$.

**Solution:** Columns span the line through $(1,2)$. Is $(3,5)$ on it? No — inconsistent.

**Key insight:** One equation contradicts the other; the column space is one-dimensional.

## Common Traps

- Assuming square ⇒ always solvable (only true with independent columns)
- $\dim C(A) = \text{rank}$, not the number of rows or columns alone
- $C(A) \subset \mathbb{R}^m$ — count carefully which space it lives in

## Connections

- Row-and-Column-Pictures · Complete-Solution — full solvability story
- Linear-Independence — what "span" really needs
- Four-Fundamental-Subspaces
