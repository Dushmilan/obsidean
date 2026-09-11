# Quiz & Synthesis

The first half of this course collapses into one storyline: the division algorithm powers the Euclidean algorithm, which powers Bézout, which powers inverses in $\mathbb{Z}_n$, which makes $\mathbb{Z}_n$ the model finite group — and subgroups of cyclic groups turn out to be countable by divisors alone. Every topic is load-bearing for the next. This synthesizes 01-Sets-Relations-Functions through 05-Subgroups-and-Cyclic-Groups.

**The Intuition:** Think of the course as a chain of dominoes. Sets and functions teach you how to *compare* structures; divisibility teaches you how $\mathbb{Z}$ *measures itself*; modular arithmetic builds a *new* structure from that measurement; the group axioms extract the *skeleton* common to all such structures; and cyclic groups show the skeleton's simplest body. If one domino wobbles (usually Bézout), everything after it falls.

**The Math:** The dependency map to keep in your head:

$$
\text{division alg.} \;\Rightarrow\; \gcd \;\Rightarrow\; \text{Bézout } ax+by=g \;\Rightarrow\; a^{-1} \text{ in } \mathbb{Z}_n \iff \gcd(a,n)=1
$$

$$
\langle a \rangle = \{e, a, \dots, a^{n-1}\}, \qquad |[k]|_{\mathbb{Z}_n} = \frac{n}{\gcd(k,n)}, \qquad H \le \mathbb{Z} \iff H = n\mathbb{Z}
$$

Test yourself below before reading the solutions.

**Setup (Q1 — foundations):** On $\mathbb{Z}$ define $a \sim b$ iff $3 \mid a - b$. List the equivalence classes and explain why $[7] = [100]$.

**Solution:** Classes are $[0], [1], [2]$: remainders mod 3. Since $100 - 7 = 93 = 31 \cdot 3$, we have $3 \mid 93$, so $100 \sim 7$ and both live in class $[1]$.

**Key insight:** "$n \mid$ difference" is membership in the same remainder bin — equivalence classes are exactly congruence classes.

**Setup (Q2 — divisibility):** Show: if $d \mid a$ and $a \ne 0$, then $d \le |a|$.

**Solution:** $a = dq$ with $q \in \mathbb{Z}$ nonzero. Then $|d| = |a|/|q| \le |a|$ since $|q| \ge 1$.

**Key insight:** Divisors are bounded by their multiples — the fact that makes "find all divisors" a finite search.

**Setup (Q3 — Bézout):** Find integers $x, y$ with $51x + 21y = 3$.

**Solution:** $51 = 2 \cdot 21 + 9$; $21 = 2 \cdot 9 + 3$; $9 = 3 \cdot 3 + 0$. So $\gcd = 3$. Back-substitute: $3 = 21 - 2 \cdot 9 = 21 - 2(51 - 2 \cdot 21) = 5 \cdot 21 - 2 \cdot 51$. So $x = -2$, $y = 5$.

**Key insight:** The last nonzero remainder is the gcd; back-substitution hands you the coefficients.

**Setup (Q4 — modular arithmetic):** Solve $4x \equiv 6 \pmod {10}$.

**Solution:** $d = \gcd(4,10) = 2$ and $2 \mid 6$, so there are $d = 2$ solutions. Divide through by 2: $2x \equiv 3 \pmod 5$. Inverse of $2$ mod 5 is $3$ ($2 \cdot 3 = 6 \equiv 1$). So $x \equiv 9 \equiv 4 \pmod 5$, i.e. $x \equiv 4$ or $x \equiv 9 \pmod{10}$.

**Key insight:** Non-coprime coefficients don't kill solvability — they multiply the solution count by $\gcd(a,n)$.

**Setup (Q5 — groups):** Let $G$ be a group with $ab = e$. Prove $ba = e$ without assuming $G$ is abelian.

**Solution:** From $ab = e$, we get $b = eb = (a^{-1}a)b = a^{-1}(ab) = a^{-1}e = a^{-1}$. Hence $ba = a^{-1}a = e$. ✓

**Key insight:** In a group, a one-sided inverse is automatically two-sided — associativity plus identity does all the work.

**Setup (Q6 — cyclics):** In $\mathbb{Z}_{15}$, compute $|[10]|$, list $\langle[10]\rangle$, and decide whether $[10]$ generates the whole group.

**Solution:** $|[10]| = 15/\gcd(10,15) = 15/5 = 3$. $\langle[10]\rangle = \{0, 10, 5\}$. Since order $3 < 15$, $[10]$ is not a generator; generators require coprimality: $[1], [2], [4], [7], [8], [11], [13], [14]$.

**Key insight:** One formula, three answers: $n/\gcd(k,n)$ gives order, then powers give the subgroup, then comparison with $n$ settles generator status.

---

### Additional Notes

Self-check rubric: can you (1) verify an equivalence relation in three lines, (2) run the Euclidean algorithm forwards *and* backwards, (3) invert any unit in $\mathbb{Z}_n$, (4) prove uniqueness facts by double-computation, and (5) enumerate subgroups of $\mathbb{Z}_n$ from divisors? If yes, this course is yours. The natural sequel — cosets, Lagrange's theorem, homomorphisms — reuses every single tool above at larger scale.

---
