---
date: 2026-08-16
type: problem-pattern
tags: [maths, pure, patterns, algebra]
parent: [[01-Algebra_Index]]
---

# Mathematical Induction — Templates

Induction is a machine with four fixed parts: state the claim $P(n)$, verify the base case, assume $P(k)$, and show $P(k+1)$ by building on the assumption. The skill is knowing what algebraic move to make in the last step for each problem type.

## Template 1: Summation formulas

**When you see:** "show $\sum_{i=1}^{n} f(i) = g(n)$".

**The move:** Write $P(k+1) = P(k) + f(k+1)$, substitute the assumed formula for $P(k)$, then factor to reach $g(k+1)$.

**Example:** Show $\sum_{i=1}^n (2i-1) = n^2$.

**Setup:** Sum of the first $n$ odd numbers.

**Solution:** Base $n=1$: $1=1$ ✓. Assume $\sum_{i=1}^k (2i-1) = k^2$. Then $\sum_{i=1}^{k+1} (2i-1) = k^2 + 2(k+1)-1 = k^2 + 2k + 1 = (k+1)^2$. Done.

**Key insight:** The last step is pure algebra — expand $g(k) + f(k+1)$ and factor.

## Template 2: Divisibility

**When you see:** "show $P(n)$ is divisible by $m$".

**The move:** In the inductive step, write $P(k+1) - P(k)$ and show it's divisible by $m$; since $P(k)$ is, so is $P(k+1)$.

**Example:** Show $7^n - 1$ is divisible by 6.

**Setup:** Divisibility claim.

**Solution:** Base: $7^1-1=6$ ✓. Assume $7^k - 1 = 6t$. Then $7^{k+1} - 1 = 7(7^k) - 1 = 7(6t+1) - 1 = 42t + 6 = 6(7t+1)$.

**Key insight:** Express $P(k+1)$ in terms of $P(k)$ by peeling off one factor.

## Template 3: Inequalities

**The move:** After assuming $P(k)$, establish a simple extra bound to close the chain.

**Example:** Show $2^n \ge n+1$ for $n \ge 1$.

**Setup:** Inequality induction.

**Solution:** Base: $2\ge2$ ✓. Assume $2^k \ge k+1$. Then $2^{k+1} = 2\cdot 2^k \ge 2(k+1) = 2k+2 \ge k+2 = (k+1)+1$ since $k \ge 0$. Done.

**Key insight:** Doubling the left side makes it grow faster than the right — the trivial inequality $2k+2 \ge k+2$ is enough.

## Template 4: Recurrences / sequences

**The move:** Substitute the assumed closed form into the recurrence and simplify.

**Example:** $u_1 = 1$, $u_{n+1} = 3u_n + 2$. Show $u_n = 2\cdot 3^{n-1} - 1$.

**Setup:** Recurrence to closed form.

**Solution:** Base: $u_1 = 2\cdot 3^0 - 1 = 1$ ✓. Assume $u_k = 2\cdot3^{k-1}-1$. Then $u_{k+1} = 3(2\cdot3^{k-1}-1) + 2 = 2\cdot3^k - 3 + 2 = 2\cdot 3^k - 1$. Done.

**Key insight:** Rewrite $3\cdot3^{k-1}$ as $3^k$ — the closed form's shape is preserved by the recurrence.
