# Transposes, Permutations, and Vector Spaces

We can solve systems and factor matrices — but what are we actually solving *in*? This lecture introduces the two most important subspaces of any matrix: the column space $C(A)$ (what can $A$ reach?) and the nullspace $N(A)$ (what gets crushed to zero?). Every solvability question reduces to "is $\mathbf{b}$ in $C(A)$?" and every multiplicity-of-solutions question reduces to "what is $N(A)$?" The transpose and permutation matrices are the component-level tools that connect rows to columns, and they set up the [[08-Four-Fundamental-Subspaces|four fundamental subspaces]].

**The Intuition:** Think of $A$ as a machine that takes $n$-dimensional inputs and produces $m$-dimensional outputs. $C(A)$ is all possible outputs — the "output range." $N(A)$ is all inputs that produce zero — the "input waste." If you think of $A$ as a transformation, the column space tells you what it can do, and the nullspace tells you what it cannot distinguish. The transpose is not just a notation trick — it's the bridge to the row space and left nullspace. Permutation matrices are the simplest orthogonal matrices, and they're the building blocks of all row-exchange algorithms. Every subspace must pass through the origin, be closed under addition, and be closed under scalar multiplication. In $\mathbb{R}^3$, the only subspaces are the origin, lines through the origin, planes through the origin, and all of $\mathbb{R}^3$.

**The Math:** $A$ is $m \times n$. The transpose $A^T$ swaps rows and columns, and reverses order in products: $(AB)^T = B^T A^T$. For any matrix $R$, the product $R^T R$ is always symmetric — this is the foundation of [[13-Projections-Least-Squares|least squares]]. The column space $C(A)$ is the span of $A$'s columns, a subspace of $\mathbb{R}^m$. The nullspace $N(A)$ is all $\mathbf{x}$ with $A\mathbf{x} = \mathbf{0}$, a subspace of $\mathbb{R}^n$. They live in different spaces and cannot be compared directly. Permutation matrices $P$ satisfy $P^T = P^{-1}$ and record row swaps. To check if $\mathbf{b}$ is in $C(A)$, augment $[A \mid \mathbf{b}]$ and row-reduce — if you get a zero row with a non-zero on the right, $\mathbf{b}$ is not in $C(A)$. To find $N(A)$, row-reduce $A$ to RREF and read off the special solutions.

**Worked Examples:**

**Example 1: Column space of a 2x3 matrix.** $A = \begin{bmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \end{bmatrix}$. $C(A)$ is the span of columns $(1,4)^T$, $(2,5)^T$, $(3,6)^T$ in $\mathbb{R}^2$. The first two are independent, so $C(A) = \mathbb{R}^2$ — the entire plane.

**Key insight:** When $m < n$, $C(A)$ can be all of $\mathbb{R}^m$ if the rank is $m$.

**Example 2: Nullspace via RREF.** $A = \begin{bmatrix} 1 & 2 & 2 \\ 2 & 4 & 6 \end{bmatrix}$. RREF gives $R = \begin{bmatrix} 1 & 2 & 0 \\ 0 & 0 & 1 \end{bmatrix}$. One free variable (column 2). Special solution: $\mathbf{x}_s = (-2, 1, 0)^T$. So $N(A) = \text{span}\{(-2, 1, 0)^T\}$, a line in $\mathbb{R}^3$.

**Key insight:** The nullspace is the set of all combinations of special solutions — one per free variable.

**Example 3: Transpose and symmetry.** $A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$. $A^T = \begin{bmatrix} 1 & 3 \\ 2 & 4 \end{bmatrix}$. $A^T A = \begin{bmatrix} 10 & 14 \\ 14 & 20 \end{bmatrix}$, which is symmetric. $R^T R$ is always symmetric, regardless of $R$.

**Key insight:** $C(A)$ and $N(A)$ live in different spaces ($\mathbb{R}^m$ vs $\mathbb{R}^n$) — don't confuse $C(A)$ with $C(A^T)$.
