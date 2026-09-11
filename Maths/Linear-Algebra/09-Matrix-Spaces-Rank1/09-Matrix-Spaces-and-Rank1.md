# Matrix Spaces and Rank-1 Matrices

Matrices themselves can be vectors in a vector space — any time you can add and scalar-multiply without leaving the set, you have a vector space. The most elementary non-zero matrix is one of rank 1: an outer product $A = \mathbf{u}\mathbf{v}^T$, where every entry is a product of one entry from $\mathbf{u}$ and one from $\mathbf{v}$. Rank-1 matrices are the atoms of matrix algebra — every rank-$r$ matrix decomposes as a sum of $r$ rank-1 matrices, which is the foundation of SVD. This builds on 08-Four-Fundamental-Subspaces.

**The Intuition:** A rank-1 matrix is like a single brushstroke — it captures one direction of variation. A rank-$r$ matrix is like $r$ brushstrokes layered on top of each other. The outer product $\mathbf{u}\mathbf{v}^T$ is an $m \times n$ matrix where every column is a multiple of $\mathbf{u}$ and every row is a multiple of $\mathbf{v}^T$. If you have only one independent column and one independent row, every entry is determined by their product — there is no other freedom.

**The Math:** $\mathbf{u}$ is $m \times 1$, $\mathbf{v}$ is $n \times 1$, then $\mathbf{u}\mathbf{v}^T$ is $m \times n$ with rank at most 1. If $\mathbf{u} = \mathbf{0}$ or $\mathbf{v} = \mathbf{0}$, the rank is 0; otherwise it's exactly 1. The space of all $m \times n$ matrices $M_{m \times n}$ has dimension $mn$ (each entry is a free parameter). Subspaces like symmetric matrices ($A = A^T$) have dimension $\frac{n(n+1)}{2}$, and upper triangular matrices ($a_{ij} = 0$ for $i > j$) also have dimension $\frac{n(n+1)}{2}$. Their intersection — diagonal matrices — has dimension $n$.

Be careful: $\mathbf{u}\mathbf{v}^T$ (outer product, a matrix) is completely different from $\mathbf{u}^T\mathbf{v}$ (inner product, a scalar). The set of all $m \times n$ matrices satisfies all vector space axioms: closure under addition, closure under scalar multiplication, existence of zero matrix. Symmetry constraints like $A = A^T$ are linear, so the symmetric matrices form a subspace.

**Setup:** $\mathbf{u} = (1, 2, 3)^T$, $\mathbf{v} = (4, 5)^T$.

**Solution:** $\mathbf{u}\mathbf{v}^T = \begin{bmatrix} 4 & 5 \\ 8 & 10 \\ 12 & 15 \end{bmatrix}$. Every column is a multiple of $(1, 2, 3)^T$, and every row is a multiple of $(4, 5)$. This is rank 1.

**Key insight:** For $A = \begin{bmatrix} 1 & 2 \\ 3 & 6 \end{bmatrix}$, column 2 is $2 \times$ column 1, so $A = (1, 3)^T(1, 2)$ — rank 1. The space of $3 \times 3$ symmetric matrices has dimension 6: 3 diagonal entries + 3 above-diagonal entries determine the whole matrix, since $a_{ij} = a_{ji}$. This is the first step toward SVD, where any rank-$r$ matrix decomposes into $r$ rank-1 pieces.
