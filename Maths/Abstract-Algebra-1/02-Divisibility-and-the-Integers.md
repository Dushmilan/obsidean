# Divisibility & the Integers

The integers are our first algebraic structure and the source of every "worked example" in group theory later. Divisibility is the engine: it gives us the division algorithm, greatest common divisors, and — ultimately — unique prime factorization.

**The Intuition:** Think of divisibility as tiling: $d \mid n$ means you can tile a length-$n$ stick perfectly using length-$d$ sticks. The division algorithm says one partial tile may be left over, but the leftover is always *smaller than the tile itself* — that single fact powers everything, from the Euclidean algorithm to modular arithmetic. The GCD is the longest stick that tiles both of two given lengths; the Euclidean algorithm finds it by repeatedly shaving off the biggest whole pieces until nothing remains.

**The Math:**

- **Divisibility:** $d \mid n$ means $n = dq$ for some $q \in \mathbb{Z}$. Basic property: if $d \mid a$ and $d \mid b$ then $d \mid (ax + by)$ for any integers $x, y$.
- **Division Algorithm:** For $n \in \mathbb{Z}$ and $d > 0$, there exist **unique** $q, r \in \mathbb{Z}$ with
$$n = dq + r, \qquad 0 \le r < d.$$
- **GCD:** $g = \gcd(a,b)$ is the largest common divisor; equivalently the smallest positive value of $ax + by$. **Bézout's identity:** there exist $x, y \in \mathbb{Z}$ with $ax + by = g$. Moreover $a, b$ coprime ($\gcd = 1$) iff $ax + by = 1$ is solvable.
- **Euclidean Algorithm:** repeatedly replace $(a, b) \to (b,\ a \bmod b)$ until the remainder is $0$; the last nonzero remainder is $\gcd(a,b)$.
- **Euclid's Lemma:** if $p$ is prime and $p \mid ab$, then $p \mid a$ or $p \mid b$.
- **Fundamental Theorem of Arithmetic:** every integer $n > 1$ factors into primes uniquely up to order:
$$n = p_1^{e_1} p_2^{e_2} \cdots p_k^{e_k}.$$

From factorizations, $\gcd$ and $\mathrm{lcm}$ read off directly: minimum exponents give $\gcd$, maximum exponents give $\mathrm{lcm}$, and always
$$ab = \gcd(a,b)\cdot \mathrm{lcm}(a,b)$$
for positive $a, b$.

**Setup:** Compute $\gcd(252, 198)$ via the Euclidean algorithm and express it as $252x + 198y$.

**Solution:** Divide:
$252 = 1 \cdot 198 + 54$
$198 = 3 \cdot 54 + 36$
$54 = 1 \cdot 36 + 18$
$36 = 2 \cdot 18 + 0$
So $\gcd = 18$. Back-substitute: $18 = 54 - 36 = 54 - (198 - 3\cdot54) = 4\cdot54 - 198 = 4(252 - 198) - 198 = 4 \cdot 252 - 5 \cdot 198$. So $x = 4$, $y = -5$.

**Key insight:** Back-substitution is bookkeeping — keep each remainder expressed in terms of the original pair and the coefficients fall out automatically.

**Setup:** Prove that $\sqrt{2}$ is irrational.

**Solution:** Suppose $\sqrt{2} = a/b$ in lowest terms. Then $a^2 = 2b^2$, so $2 \mid a^2$, hence $2 \mid a$ (Euclid's Lemma). Write $a = 2c$: then $4c^2 = 2b^2 \Rightarrow b^2 = 2c^2$, so $2 \mid b$ too — contradicting lowest terms.

**Key insight:** Euclid's Lemma is what makes "the prime divides the square ⇒ divides the root" legal. Unique factorization is doing the real work here.

**Setup:** Find all integer solutions of $12x + 8y = 20$.

**Solution:** Divide through by $\gcd(12,8)=4$: $3x + 2y = 5$. One solution: $x=1, y=1$. General solution: $x = 1 + 2t$, $y = 1 - 3t$, $t \in \mathbb{Z}$.

**Key insight:** $ax + by = c$ is solvable iff $\gcd(a,b) \mid c$; once one solution is known, all others differ by multiples of $(b/g,\,-a/g)$.

---

### Additional Notes

Why does the Euclidean algorithm terminate? Each remainder satisfies $0 \le r_{i+1} < r_i$, so remainders form a strictly decreasing sequence of non-negative integers — it must hit $0$. This same "strictly decreasing non-negative integer" trick reappears in countless proofs, including showing that every subgroup of $\mathbb{Z}$ has the form $n\mathbb{Z}$ (05-Subgroups-and-Cyclic-Groups) and computing inverses in $\mathbb{Z}_n$ (03-Modular-Arithmetic).

---
