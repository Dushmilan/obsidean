# Matrix Multiplication and Inverses

How do you combine two matrices into one? Matrix multiplication can be computed four equivalent ways — dot-product, column, row, and block — and each view reveals different structure. The inverse $A^{-1}$ is the matrix that undoes $A$: a record of 02-Elimination-and-RREF run in reverse. If $A^{-1}$ exists, then $A\mathbf{x} = \mathbf{b}$ has a unique solution $\mathbf{x} = A^{-1}\mathbf{b}$. This is the language of 05-Transposes-Permutations-Spaces.

**The Intuition:** Multiplication is composition of transformations. If $A$ rotates and $B$ scales, then $AB$ scales first, then rotates — read right to left. Think of $A$ as a machine and $B$ as its input: the product $AB$ is the output of running $B$ through $A$, column by column. The entry $(i,j)$ of $AB$ is the dot product of row $i$ of $A$ and column $j$ of $B$, which is the only way to combine entries while respecting linear structure.

**The Math:** $A$ is $m \times n$, $B$ is $n \times p$, $C = AB$ is $m \times p$. Inner dimensions must match. The four views: dot-product way gives entry $(i,j)$ as row $i$ of $A$ dot column $j$ of $B$. Column way: column $j$ of $C$ is a combination of columns of $A$ using column $j$ of $B$. Row way: row $i$ of $C$ is a combination of rows of $B$ using row $i$ of $A$. Block way: partition into conformable sub-blocks and multiply as if blocks were scalars. Multiplication is not commutative: $AB \neq BA$ in general. The inverse satisfies $A^{-1}A = AA^{-1} = I$, and only exists for square, full-rank matrices — equivalent conditions include: columns are linearly independent, determinant is non-zero, rank is $n$, and nullspace is trivial. For inverses, order reverses: $(AB)^{-1} = B^{-1}A^{-1}$. Gauss-Jordan computes $A^{-1}$ by forming $[A \mid I]$ and row-reducing until the left side becomes $I$; the right side becomes $A^{-1}$.

**What does this mean for matrix algebra?** Always check $(AB)^{-1} = B^{-1}A^{-1}$, not $A^{-1}B^{-1}$. Order reverses for both inverses and 05-Transposes-Permutations-Spaces.

**Setup:** $A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$, $B = \begin{bmatrix} 5 & 6 \\ 7 & 8 \end{bmatrix}$. Compute $AB$ the column way.

**Solution:** Column 1 of $AB$ = $5 \times (\text{col 1 of } A) + 7 \times (\text{col 2 of } A) = 5(1,3)^T + 7(2,4)^T = (19, 43)^T$.

**Key insight:** The column view makes it clear — each column of the product is a combination of $A$'s columns.

**Setup:** $A = \begin{bmatrix} 1 & 2 \\ 3 & 7 \end{bmatrix}$. Find $A^{-1}$ via Gauss-Jordan.

**Solution:** Form $[A \mid I] = \begin{bmatrix} 1 & 2 & 1 & 0 \\ 3 & 7 & 0 & 1 \end{bmatrix}$. Row-reduce: $\to \begin{bmatrix} 1 & 2 & 1 & 0 \\ 0 & 1 & -3 & 1 \end{bmatrix} \to \begin{bmatrix} 1 & 0 & 7 & -2 \\ 0 & 1 & -3 & 1 \end{bmatrix}$. So $A^{-1} = \begin{bmatrix} 7 & -2 \\ -3 & 1 \end{bmatrix}$.

**Key insight:** The operations that reduce $A$ to $I$ also transform $I$ into $A^{-1}$.

**Setup:** $A = \begin{bmatrix} 0 & 1 \\ 0 & 0 \end{bmatrix}$, $B = \begin{bmatrix} 0 & 0 \\ 1 & 0 \end{bmatrix}$. Compare $AB$ and $BA$.

**Solution:** $AB = \begin{bmatrix} 1 & 0 \\ 0 & 0 \end{bmatrix}$ but $BA = \begin{bmatrix} 0 & 0 \\ 0 & 1 \end{bmatrix}$. Even for $2 \times 2$ matrices, multiplication is almost never commutative.

---
