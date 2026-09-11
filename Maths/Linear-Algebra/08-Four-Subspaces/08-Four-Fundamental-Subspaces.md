# The Four Fundamental Subspaces

Every $m \times n$ matrix $A$ of rank $r$ defines four subspaces that together describe the complete structure of the linear system. Two live in $\mathbb{R}^n$ (the input room) and two in $\mathbb{R}^m$ (the output room). This is the architectural blueprint of the entire course — every later result on 12-Orthogonal-Vectors-Subspaces, 13-Projections-Least-Squares, and least squares is a re-reading of this picture. It builds directly on 05-Transposes-Permutations-Spaces and 07-Independence-Basis-Dimension.

**The Intuition:** Think of $A$ as a machine with an input room ($\mathbb{R}^n$) and an output room ($\mathbb{R}^m$). The input room splits into "useful inputs" (the row space $C(A^T)$) and "wasted inputs" (the nullspace $N(A)$). The output room splits into "reachable outputs" (the column space $C(A)$) and "unreachable outputs" (the left nullspace $N(A^T)$). The rank $r$ is the bridge connecting them.

**The Math:** $A$ is $m \times n$ with rank $r$. The four subspaces:

- $C(A)$ — column space in $\mathbb{R}^m$, dimension $r$. All possible outputs $A\mathbf{x}$.
- $N(A)$ — nullspace in $\mathbb{R}^n$, dimension $n - r$. All inputs crushed to zero.
- $C(A^T)$ — row space in $\mathbb{R}^n$, dimension $r$. What $A$ "sees."
- $N(A^T)$ — left nullspace in $\mathbb{R}^m$, dimension $m - r$. The "observers" that see no effect of $A$.

The dimension counts are locked: $r + (n - r) = n$ in $\mathbb{R}^n$ and $r + (m - r) = m$ in $\mathbb{R}^m$. The two subspaces in each room are complementary — they fill it completely. A key revelation: row rank equals column rank. This follows from RREF having the same number of pivot rows as pivot columns. The left nullspace $N(A^T)$ has a physical meaning in 10-Graphs-Networks-Incidence: it's the set of current distributions satisfying Kirchhoff's Current Law. Don't confuse $C(A)$ with $C(A^T)$ — they live in different spaces ($\mathbb{R}^m$ vs $\mathbb{R}^n$).

**Setup:** $A = \begin{bmatrix} 1 & 2 \\ 3 & 6 \\ 2 & 4 \end{bmatrix}$.

**Solution:** $r = 1$. $C(A) = \text{span}\{(1,3,2)^T\}$, a line in $\mathbb{R}^3$. $N(A) = \text{span}\{(-2,1)^T\}$, a line in $\mathbb{R}^2$. $C(A^T) = \text{span}\{(1,2)^T\}$, a line in $\mathbb{R}^2$. $N(A^T) = \text{span}\{(-3,1,0)^T, (-2,0,1)^T\}$, a plane in $\mathbb{R}^3$. Check: $r + (n - r) = 1 + 1 = 2 = n$ and $r + (m - r) = 1 + 2 = 3 = m$.

**Key insight:** For full-rank $2 \times 2$ matrices like $A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$, all four subspaces are trivial or full: $C(A) = \mathbb{R}^2$, $N(A) = \{0\}$, $C(A^T) = \mathbb{R}^2$, $N(A^T) = \{0\}$. In 10-Graphs-Networks-Incidence, a $3$-edge, $2$-node incidence matrix gives physical meaning to all four: $C(A^T)$ is potential differences along edges, $N(A)$ is equilibrium potentials, $C(A)$ is edge-state vectors, and $N(A^T)$ is current distributions satisfying KCL.
