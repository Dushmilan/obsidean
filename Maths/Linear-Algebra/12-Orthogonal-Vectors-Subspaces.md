# Orthogonal Vectors and Subspaces

Part 2 of the Fundamental Theorem reveals the geometric punchline of [[08-Four-Fundamental-Subspaces|the four subspaces]]: they are not just dimension-complementary — they are perpendicular. The nullspace is locked at 90 degrees to the row space. Once orthogonality is established, every subsequent algorithm — Gram-Schmidt, least squares, QR — becomes geometric rather than algebraic. This is the engine that drives [[13-Projections-Least-Squares|projections]].

**The Intuition:** Orthogonal vectors are like coordinate axes — they point in completely independent directions. Knowing one tells you nothing about the other in that direction. In $\mathbb{R}^3$, the $x$-$y$ plane is perpendicular to the $z$-axis. Every vector in the plane is perpendicular to the $z$-axis. They are orthogonal complements. The Pythagorean theorem confirms perpendicularity: if $\mathbf{x} \perp \mathbf{y}$, then $\|\mathbf{x}\|^2 + \|\mathbf{y}\|^2 = \|\mathbf{x} + \mathbf{y}\|^2$.

**The Math:** Two vectors are orthogonal if $\mathbf{x}^T \mathbf{y} = 0$. Two subspaces $V$ and $W$ are orthogonal complements if every vector in $V$ is perpendicular to every vector in $W$. For a subspace $V$ of $\mathbb{R}^n$, $V^\perp$ has dimension $n - \dim(V)$, and together they span all of $\mathbb{R}^n$: every vector can be uniquely written as $\mathbf{v} + \mathbf{w}$ with $\mathbf{v} \in V$ and $\mathbf{w} \in V^\perp$. The Fundamental Theorem Part 2 states: $C(A^T) \perp N(A)$ in $\mathbb{R}^n$, and $C(A) \perp N(A^T)$ in $\mathbb{R}^m$.

Orthogonal is not the same as linearly independent — $(1,0)$ and $(1,1)$ are independent but not perpendicular. But perpendicular vectors are always independent. The orthogonal complement is unique: for a given $V$, there is exactly one $V^\perp$. And be aware: orthogonality depends on the inner product — a different inner product gives a different notion of perpendicularity.

**Setup:** $\mathbf{x} = (1, 2, 3)^T$, $\mathbf{y} = (4, -2, 0)^T$.

**Solution:** $\mathbf{x}^T \mathbf{y} = 1(4) + 2(-2) + 3(0) = 4 - 4 + 0 = 0$. They are orthogonal.

**Key insight:** For $V = \text{span}\{(1,1,1)^T\}$ in $\mathbb{R}^3$, $V^\perp = \{\mathbf{x} : x_1 + x_2 + x_3 = 0\}$, a plane through the origin. The orthogonal complement of a line is a plane; the orthogonal complement of a plane is a line. Verify with the Pythagorean theorem: $\mathbf{x} = (3,0)^T$, $\mathbf{y} = (0,4)^T$ give $\|\mathbf{x}\|^2 + \|\mathbf{y}\|^2 = 9 + 16 = 25 = \|\mathbf{x} + \mathbf{y}\|^2$. This confirms perpendicularity and is the foundation for projections — the error is always orthogonal to the column space.
