# 1.8 Mathematical Induction

Mathematical induction proves a statement $P(n)$ is true for all natural numbers by verifying a base case and proving an inductive step. It is the domino principle formalised as a proof technique: if you knock over the first domino (base case) and ensure each domino knocks over the next (inductive step), then all dominoes fall. This simple idea is surprisingly powerful — it can prove summation formulas, divisibility results, inequalities, and properties of recursively defined sequences.

**The Intuition:** Imagine an infinite line of dominoes. If you knock over the first one, and each domino is close enough to knock over the next, then all dominoes fall. A chain of truth: $P(1)$ is true, $P(1) \Rightarrow P(2)$, $P(2) \Rightarrow P(3)$, and so on. By chaining these implications, $P(n)$ is true for every $n$.

**The Math:**

- **Standard induction:** Verify $P(1)$, then prove $P(k) \Rightarrow P(k+1)$ for all $k \ge 1$
- **Strong induction:** Verify $P(1)$, then prove $P(1) \land \cdots \land P(k) \Rightarrow P(k+1)$
- **Starting at $n_0$:** Verify $P(n_0)$, then prove $P(k) \Rightarrow P(k+1)$ for $k \ge n_0$
- **Two-step:** Verify $P(1), P(2)$, then prove $P(k) \land P(k+1) \Rightarrow P(k+2)$

The inductive hypothesis is an ASSUMPTION, not a proof. You assume $P(k)$ is true for some $k$, then use it to prove $P(k+1)$. You must reach EXACTLY $P(k+1)$ — if your final line is "close to" but not identical, the proof is incomplete.

**What does this mean for Pure Mathematics?** Induction is the only rigorous method for proving statements about all natural numbers, and it underpins much of number theory and discrete mathematics. Strong induction is useful when the truth of $P(k+1)$ depends on multiple previous cases, not just the immediate predecessor.

### Example 1: Prove $\sum_{r=1}^n r^2 = \frac{n(n+1)(2n+1)}{6}$

**Setup:** Summation formula.

**Solution:** Base ($n=1$): LHS = $1$, RHS = $\frac{1 \cdot 2 \cdot 3}{6} = 1$. Assume $P(k)$: $\sum_{r=1}^k r^2 = \frac{k(k+1)(2k+1)}{6}$. Prove $P(k+1)$: add $(k+1)^2$ to both sides and simplify:
$\frac{k(k+1)(2k+1)}{6} + (k+1)^2 = \frac{(k+1)[k(2k+1) + 6(k+1)]}{6} = \frac{(k+1)(k+2)(2k+3)}{6} = \frac{(k+1)((k+1)+1)(2(k+1)+1)}{6}$

**Key insight:** Add the $(k+1)$th term to both sides and simplify to the target form.

### Example 2: Prove $7^n - 2^n$ is divisible by 5

**Setup:** Divisibility statement.

**Solution:** Base ($n=1$): $7 - 2 = 5$. Assume $P(k)$: $7^k - 2^k = 5m$. Prove $P(k+1)$: $7^{k+1} - 2^{k+1} = 7 \cdot 7^k - 2 \cdot 2^k = 7(7^k - 2^k) + 7 \cdot 2^k - 2 \cdot 2^k = 7(5m) + 5 \cdot 2^k = 5(7m + 2^k)$.

**Key insight:** Express $a^{k+1} - b^{k+1}$ using $a^k - b^k$ to factor out the divisor.

### Example 3: Prove $2^n > n^2$ for $n \ge 5$

**Setup:** Inequality statement.

**Solution:** Base ($n=5$): $32 > 25$. Assume $P(k)$: $2^k > k^2$. Prove $P(k+1)$: $2^{k+1} = 2 \cdot 2^k > 2k^2$. Need $2k^2 \ge (k+1)^2 \iff (k-1)^2 \ge 2$, true for $k \ge 3$, hence for $k \ge 5$.

**Key insight:** For inequalities, you often need to prove an auxiliary inequality as part of the inductive step.

### Example 4: Prove $u_n = 3^n - 1$ where $u_1 = 2$, $u_{n+1} = 3u_n + 2$

**Setup:** Recurrence relation.

**Solution:** Base ($n=1$): $3^1 - 1 = 2 = u_1$. Assume $P(k)$: $u_k = 3^k - 1$. Prove $P(k+1)$: $u_{k+1} = 3u_k + 2 = 3(3^k - 1) + 2 = 3^{k+1} - 3 + 2 = 3^{k+1} - 1$.

**Key insight:** Substitute the inductive hypothesis directly into the recurrence relation.

---
