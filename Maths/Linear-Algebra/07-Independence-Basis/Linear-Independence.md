
## Definition

Vectors $\{\mathbf{v}_1, \dots, \mathbf{v}_k\}$ are **linearly independent** if

$$c_1\mathbf{v}_1 + \cdots + c_k\mathbf{v}_k = \mathbf{0} \Rightarrow c_1 = \cdots = c_k = 0$$

Equivalently: no vector is a combination of the others, and the matrix with these columns has a trivial nullspace. Dependent ⇔ some vector is redundant.

## The Intuition

A committee: independence means every member brings a unique perspective — nobody is redundant. One vector is independent iff nonzero. Two are dependent iff parallel. In $\mathbb{R}^3$ you can have at most 3 independent vectors.

## The Toolkit

| Test | Condition |
|------|-----------|
| Definition | combination = 0 forces all weights 0 |
| Matrix test | columns of $A$ independent ⇔ $N(A) = \{\mathbf{0}\}$ |
| Square test | $\det A \neq 0$ |
| Count | more than $n$ vectors in $\mathbb{R}^n$ ⇒ dependent |
| Pivots | independent ⇔ pivot in every column |

## Derivation

Set up $A\mathbf{c} = \mathbf{0}$ and solve. If the only solution is $\mathbf{c} = \mathbf{0}$, the columns are independent — that's the definition rewritten. Redundancy: if $\mathbf{v}_j = \sum_{i\neq j} c_i\mathbf{v}_i$, then moving terms gives a nontrivial zero combination. [Full derivations: RREF-Free-Variables-and-Special-Solutions]

## Method

1. Put the vectors as columns of $A$.
2. Compute rank: independent ⇔ $r = k$ (all columns pivot).
3. Equivalently for square $A$: $\det A \neq 0$.

## Worked Examples

**Setup:** Are $(1,2)$, $(2,4)$ independent?

**Solution:** $A = \begin{bmatrix}1&2\\2&4\end{bmatrix}$, rank 1 < 2 → dependent (second is $2\times$ first).

**Key insight:** Dependent ⇒ one is a multiple/combination of others.

---

**Setup:** Are $(1,0,0),(0,1,0),(1,1,1)$ independent?

**Solution:** Matrix $\begin{bmatrix}1&0&1\\0&1&1\\0&0&1\end{bmatrix}$ has 3 pivots — independent.

**Key insight:** Three independent vectors in $\mathbb{R}^3$ span everything (a basis).

## Common Traps

- Independent doesn't mean orthogonal — just not redundant
- More vectors than dimension ⇒ automatically dependent
- The zero vector makes any set dependent
- Checking independence of *rows* vs *columns* — both have the same rank

## Connections

- Basis-and-Dimension · Rank-and-Free-Variables
- Inverses-and-Gauss-Jordan — invertibility test
