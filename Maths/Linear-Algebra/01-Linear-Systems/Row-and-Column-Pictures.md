
## Definition

The linear system $A\mathbf{x} = \mathbf{b}$ ($A$ is $m\times n$) has **two geometric pictures**:

- **Row picture:** each equation $a_{i1}x_1 + \cdots + a_{in}x_n = b_i$ is a hyperplane in $\mathbb{R}^n$; the solution is their intersection.
- **Column picture:** $\mathbf{b} = x_1\mathbf{a}_1 + x_2\mathbf{a}_2 + \cdots + x_n\mathbf{a}_n$ — find weights that build $\mathbf{b}$ from $A$'s columns.

## The Intuition

Columns are ingredients; $\mathbf{x}$ is the recipe; $\mathbf{b}$ is the dish. In 2D: two lines crossing (row picture) vs. two arrows scaling and adding to reach a target (column picture). Same answer, different geometry — and the column picture makes solvability visible.

## The Toolkit

| Picture | Question it answers | Use for |
|---------|--------------------|---------|
| Row | where do the hyperplanes intersect? | elimination algorithm |
| Column | is $\mathbf{b}$ reachable from the columns? | solvability analysis |
| Operator | what does $A$ map $\mathbf{x}$ to? | inverses, eigenvalues, transformations |

## Derivation

$A\mathbf{x} = \mathbf{b}$ expands column-wise: the matrix-vector product *is* the linear combination $\sum x_j\mathbf{a}_j$. Row-wise it's the set of dot-product equations. Both pictures are the same algebra in different coordinates. [Full derivations: Gaussian-Elimination]

## Method

1. **Solvability question** → use the column picture (fastest).
2. **Actually solving** → use the row picture with elimination.
3. **Square $A$:** solvability becomes "is $A$ invertible?"; **rectangular:** the column space is a proper subspace and most $\mathbf{b}$ are unreachable.

## Worked Examples

**Setup:** Solve $2x + y = 5$, $x + 3y = 7$.

**Solution:** Row picture: intersection of two lines. Column picture: $x(2,1) + y(1,3) = (5,7)$. Answer: $x = 1.6$, $y = 1.8$.

**Key insight:** Same answer, but the column picture shows the vectors being combined.

---

**Setup:** $x + y = 3$, $x + y = 5$.

**Solution:** Row: parallel lines, no meeting. Column: $(1,1)$ spans a line; $(3,5)$ is off it. No solution — $\mathbf{b} \notin C(A)$.

**Key insight:** The column picture immediately shows inconsistency.

## Common Traps

- Mixing up which picture answers which question
- Forgetting that in $\mathbb{R}^n$ the row picture uses hyperplanes ($n-1$ dimensional), not just lines
- The column picture needs $\mathbf{b}$ in $\mathbb{R}^m$ — count dimensions

## Connections

- Solvability-and-the-Column-Space · Gaussian-Elimination
- Four-Fundamental-Subspaces — where the column space lives
- Orthogonal-Vectors-and-Subspaces — row space ⊥ nullspace
