
## Definition

Mathematical induction proves a statement $P(n)$ is true for **all natural numbers** by:

1. **Base case** — verify $P(n_0)$ is true (usually $n_0 = 1$ or $0$).
2. **Inductive step** — prove $P(k) \Rightarrow P(k+1)$ for all $k \ge n_0$.

Then, by the chain $P(n_0) \Rightarrow P(n_0+1) \Rightarrow \cdots$, the statement holds for every $n \ge n_0$.

**Variants:**
- **Strong induction:** assume $P(1) \land P(2) \land \cdots \land P(k)$, prove $P(k+1)$ — used when $P(k+1)$ depends on multiple earlier cases.
- **Two-step induction:** verify $P(1), P(2)$; prove $P(k) \land P(k+1) \Rightarrow P(k+2)$.

## The Intuition

An infinite line of dominoes. If you knock over the first one, and each domino is close enough to knock over the next, then *all* dominoes fall. Induction formalizes "knock over first, ensure each knocks the next" into a proof technique. The inductive hypothesis is an **assumption** used to build the next truth — never a proof by itself.

## The Toolkit

| Step | Requirement |
|------|-------------|
| Base case | $P(n_0)$ true for the starting value |
| Inductive hypothesis | assume $P(k)$ true (for some $k \ge n_0$) |
| Inductive step | derive $P(k+1)$ **exactly** from $P(k)$ |
| Conclusion | "by induction, $P(n)$ for all $n \ge n_0$" |
| Strong variant | hypothesis $= P(1) \land \cdots \land P(k)$ |
| Two-step variant | hypothesis $= P(k) \land P(k+1)$, prove $P(k+2)$ |

## Derivation

The validity rests on the Well-Ordering Principle: if some $n$ makes $P$ false, the set of counterexamples has a least element $m$. Since $P(n_0)$ is true, $m > n_0$, so $P(m-1)$ is true; but the inductive step says $P(m-1) \Rightarrow P(m)$, a contradiction. [Full derivations: 08-Mathematical-Induction-Proofs]


**Prove $\sum_{r=1}^n f(r) = F(n)$:**
1. Base: check $n = n_0$.
2. Assume $P(k)$.
3. Add the $(k+1)$-th term to both sides: $F(k) + f(k+1)$.
4. Algebraically simplify to $F(k+1)$ **in exactly the target form**.

**Prove divisibility ($d \mid a^n - b^n$):**
1. Base case.
2. Express $a^{k+1} - b^{k+1}$ using $a^k - b^k$ to factor out $d$:
   $$a^{k+1} - b^{k+1} = a(a^k - b^k) + (a - b)b^k$$

**Prove inequalities:** sometimes need an auxiliary inequality (e.g. $2k^2 \ge (k+1)^2$) inside the inductive step.

**Recurrences:** substitute the hypothesis directly into the recurrence definition.

## Worked Examples

**Setup:** Prove $\sum_{r=1}^n r^2 = \frac{n(n+1)(2n+1)}{6}$.

**Solution:** Base ($n=1$): LHS $= 1$, RHS $= \frac{1\cdot2\cdot3}{6} = 1$. Assume $P(k)$. Add $(k+1)^2$:
$$\frac{k(k+1)(2k+1)}{6} + (k+1)^2 = \frac{(k+1)[k(2k+1) + 6(k+1)]}{6} = \frac{(k+1)(k+2)(2k+3)}{6}$$
which is exactly $P(k+1)$.

**Key insight:** Add the $(k+1)$-th term to both sides and simplify to the target form.

---

**Setup:** Prove $7^n - 2^n$ is divisible by 5.

**Solution:** Base ($n=1$): $7-2=5$. Assume $7^k - 2^k = 5m$. Then
$$7^{k+1} - 2^{k+1} = 7\cdot7^k - 2\cdot2^k = 7(7^k - 2^k) + 5\cdot2^k = 7(5m) + 5\cdot2^k = 5(7m + 2^k)$$

**Key insight:** Express $a^{k+1} - b^{k+1}$ using $a^k - b^k$ to factor out the divisor.

---

**Setup:** Prove $2^n > n^2$ for $n \ge 5$.

**Solution:** Base ($n=5$): $32 > 25$. Assume $2^k > k^2$. Then $2^{k+1} = 2\cdot2^k > 2k^2$. Need $2k^2 \ge (k+1)^2 \iff (k-1)^2 \ge 2$, true for $k \ge 3$.

**Key insight:** For inequalities you often prove an auxiliary inequality inside the inductive step.

---

**Setup:** Prove $u_n = 3^n - 1$ for the recurrence $u_1 = 2$, $u_{n+1} = 3u_n + 2$.

**Solution:** Base: $3^1 - 1 = 2 = u_1$. Assume $u_k = 3^k - 1$. Then $u_{k+1} = 3u_k + 2 = 3(3^k - 1) + 2 = 3^{k+1} - 1$.

**Key insight:** Substitute the inductive hypothesis directly into the recurrence.

## Common Traps

- Skipping the base case — the chain needs its first link
- Inductive hypothesis assumed, not proven — it's a tool, not the conclusion
- Finishing "close to" $P(k+1)$ instead of exactly — the proof is incomplete
- Wrong starting value — the base must match the claim's domain ($n \ge 5$, etc.)
- Using ordinary induction where strong induction is required (multiple prior cases)

## Connections

- 07-Binomial-Theorem — proving coefficient identities
- 06-Permutations-Combinations — combinatorial proofs
- 04.7-Series-Expansions — series formulas
- 03.7-Trigonometric-Series — trig series identities


## Cross-Track Connections

*Reconstructed 2026-08-24 after the registry-loss incident — see [[Maths-MOC]].*

- [[Induction and Recursion]] — the CS-side twin of induction proofs
