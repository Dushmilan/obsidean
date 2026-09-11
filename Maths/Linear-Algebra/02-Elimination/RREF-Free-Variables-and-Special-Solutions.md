
## Definition

Continue elimination to the **reduced row echelon form** $R = \text{rref}(A)$: pivots = 1, zeros above and below. RREF is **unique** for any matrix (though different elimination sequences give different $U$'s).

- **Pivot columns** ↔ pivot variables.
- **Free columns** ↔ free variables, one per column without a pivot.
- Each free variable generates a **special solution** to $A\mathbf{x} = \mathbf{0}$.

## The Intuition

The staircase ends with steps only in some columns. Columns without steps hold the free variables — you're free to choose them, and everything else is determined. The **nullspace matrix** $N$ has the special solutions as columns: set one free variable to 1, the rest to 0, solve upward.

## The Toolkit

| Quantity | Formula |
|----------|---------|
| Rank | $r$ = number of pivots |
| Free variables | $n - r$ |
| Special solutions | $n - r$ vectors in the nullspace |
| Nullspace matrix | $N$ = $[\, -\text{combination of } R \mid I \,]$-style columns |
| $\dim N(A)$ | $n - r$ |

## Derivation

From $R$, the equation $R\mathbf{x} = \mathbf{0}$ lets pivot variables be expressed in terms of free variables. Setting free variable $i$ to 1 (rest 0) gives one independent solution; there are $n-r$ of them, and they span the nullspace. [Full derivations: Four-Fundamental-Subspaces]

## Method

1. Compute $R = \text{rref}(A)$; read off pivot and free columns.
2. One special solution per free variable: set it to 1, others 0.
3. Back-substitute in $R\mathbf{x} = \mathbf{0}$ to fill the pivot entries.

## Worked Examples

**Setup:** $A = \begin{bmatrix}1&2&2&2\\2&4&6&8\\3&6&8&10\end{bmatrix}$. Find the special solutions.

**Solution:** rref gives $R = \begin{bmatrix}1&2&0&-2\\0&0&1&2\\0&0&0&0\end{bmatrix}$: pivot columns 1,3; free columns 2,4. Special solutions: $x_2$ free → $(-2,1,0,0)$; $x_4$ free → $(2,0,-2,1)$.

**Key insight:** $n - r = 2$ free variables → 2 special solutions spanning $N(A)$.

## Common Traps

- RREF is unique, $U$ is not — use $R$ for reading the nullspace
- A row of zeros ⇒ free variable, not an error
- Special solutions solve $A\mathbf{x} = \mathbf{0}$ (homogeneous), not the full system

## Connections

- Gaussian-Elimination · Complete-Solution — adding the particular solution
- Linear-Independence — special solutions are independent
- Four-Fundamental-Subspaces
