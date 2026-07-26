# Independence, Basis, Dimension

We've learned to solve systems and factor matrices — but how do we measure the size of a subspace? Three concepts answer this: linear independence (no redundancy), spanning (complete coverage), and basis (both at once). The dimension is the count of vectors in any basis, and it turns out this count is invariant — every basis of the same subspace has the same number of elements. For a matrix $A$ of rank $r$, $\dim(C(A)) = r$ and $\dim(N(A)) = n - r$, and these two numbers capture the entire structure of the linear system. This vocabulary underpins everything from [[08-Four-Fundamental-Subspaces|the four fundamental subspaces]] to [[10-Graphs-Networks-Incidence|network analysis]].

**The Intuition:** Think of a committee. Independence means every member brings a unique perspective — no one is redundant. Spanning means you have enough members to cover every issue. Basis is the minimum effective committee. In $\mathbb{R}^3$, three independent vectors span all of $\mathbb{R}^3$. Two span a plane. One spans a line. Zero vectors span only the origin.

**The Math:** A set of vectors $\{v_1, \ldots, v_k\}$ is **linearly independent** if $c_1 v_1 + \cdots + c_k v_k = \mathbf{0}$ forces all $c_i = 0$. Equivalently, no vector is a combination of the others, and the matrix with these vectors as columns has a trivial nullspace. A set **spans** $V$ if every vector in $V$ is a linear combination of them. A **basis** is both independent and spanning. The **dimension** is the number of vectors in any basis — a theorem guarantees this is the same for every basis.

For an $m \times n$ matrix $A$ of rank $r$: the pivot columns of $A$ (not of $R$) form a basis for $C(A)$ with $\dim(C(A)) = r$. The special solutions from RREF form a basis for $N(A)$ with $\dim(N(A)) = n - r$. The dimension theorem gives $r + (n - r) = n$. Don't confuse independence with perpendicularity — in $\mathbb{R}^3$, two vectors can be independent without being perpendicular. And the zero vector is never part of a linearly independent set.

**What does this mean for linear algebra?** These three concepts answer "how much of a subspace exists?" — and every result about a matrix can be phrased as a count of independent components. The rank is the most important number associated with a matrix: it tells you the dimension of the column space, the dimension of the nullspace, and the number of solutions to $A\mathbf{x} = \mathbf{b}$. In data science, the rank of a data matrix reveals the effective dimensionality of the dataset.

**Setup:** $v_1 = (1, 2, 3)^T$, $v_2 = (4, 5, 6)^T$, $v_3 = (7, 8, 9)^T$. Are these independent?

**Solution:** Form the matrix $[v_1 \ v_2 \ v_3]$ and eliminate. $R = \begin{bmatrix} 1 & 4 & 7 \\ 0 & -3 & -6 \\ 0 & 0 & 0 \end{bmatrix}$. Only 2 pivots.

**Key insight:** The third column is a combination of the first two — the set is dependent. To find a basis for $C(A)$ when $A = \begin{bmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \end{bmatrix}$, RREF gives two pivots in columns 1 and 2, so the pivot columns of $A$ — $(1, 4)^T$ and $(2, 5)^T$ — form a basis with $\dim(C(A)) = 2$. For the space of $3 \times 3$ symmetric matrices, count the free parameters: 3 diagonal + 3 above diagonal = 6, so the dimension is 6. Symmetry forces $a_{ij} = a_{ji}$, cutting the 9 parameters roughly in half.
