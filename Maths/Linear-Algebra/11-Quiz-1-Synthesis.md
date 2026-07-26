# Quiz 1 Synthesis

The first twelve lectures collapse into a single equivalence: for a square matrix, "invertible" = "full rank" = "trivial nullspace" = "independent columns" = "columns span $\mathbb{R}^n$." All five are the same fact wearing different hats. This builds on [[08-Four-Fundamental-Subspaces|the four subspaces]], [[09-Matrix-Spaces-and-Rank1|matrix spaces]], and [[10-Graphs-Networks-Incidence|networks]], and sets up [[12-Orthogonal-Vectors-Subspaces|orthogonality]] and [[13-Projections-Least-Squares|least squares]].

**The Intuition:** The equivalence table is a Rosetta Stone: one fact written in five different languages. If you know one (e.g., "the nullspace is trivial"), you automatically know all the others. Five doors all leading to the same room. The rank determines everything — if you know $r$ and $n$, you know the nullspace dimension, the column space dimension, whether solutions exist, and whether they're unique.

**The Math:** For an $n \times n$ matrix $A$, these are equivalent: (1) $A$ is invertible, (2) $r = n$ (full rank), (3) $N(A) = \{0\}$ (trivial nullspace), (4) columns of $A$ are linearly independent, (5) columns of $A$ span $\mathbb{R}^n$. The dimension formula $r + (n - r) = n$ always holds. For rectangular $A$, the equivalences split: full column rank ($r = n$) gives independence, invertibility of $A^T A$, and at most one solution. Full row rank ($r = m$) gives spanning and solutions for every $\mathbf{b}$. The key fact: $A^T A$ is invertible if and only if $A$ has full column rank — this is the bridge to least squares. The strategy for any problem: eliminate to $R$, count pivots, read off rank and nullspace dimension, check solvability.

**Setup:** $A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$.

**Solution:** Eliminate: 2 pivots. $r = 2 = n$. Nullspace $= \{0\}$. Columns independent. $A^T A = \begin{bmatrix} 10 & 14 \\ 14 & 20 \end{bmatrix}$, $\det = 4 \neq 0$, invertible. All five equivalences hold.

**Key insight:** One fact ($r = n$) implies all the others. For $A = \begin{bmatrix} 1 & 2 \\ 2 & 4 \end{bmatrix}$, we get $r = 1 < n = 2$: the nullspace has dimension 1, columns are dependent, $A^T A$ is singular — all five "failure" equivalences hold simultaneously. For the rectangular $A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \\ 5 & 6 \end{bmatrix}$, $r = 2 = n$ gives full column rank equivalences (independent columns, $A^T A$ invertible), but $r = 2 < m = 3$ means $C(A) \neq \mathbb{R}^3$ — full row rank fails. Always check whether you're in the square, full-column, or full-row case before applying equivalences.
