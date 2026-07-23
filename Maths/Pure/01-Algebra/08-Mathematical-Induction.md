---
date: 2026-07-19
type: concept
tags: [maths, pure, a-level, algebra, induction]
parent: [[Pure/01-Algebra.md]]
proofs: [[Pure/Proofs/01-Algebra/08-Mathematical-Induction-Proofs.md]]
prerequisites: []
---

# Mathematical Induction

## Principle of Mathematical Induction (PMI)

To prove statement $P(n)$ true for all $n \in \mathbb{N}$ (or $n \ge n_0$):

1. **Base Case:** Prove $P(1)$ (or $P(n_0)$) is true.
2. **Inductive Step:** Assume $P(k)$ true for some $k \ge 1$ (inductive hypothesis). Prove $P(k+1)$ is true using this assumption.
3. **Conclusion:** $P(n)$ true for all $n \in \mathbb{N}$ (or $n \ge n_0$).

**Why it works:** Domino effect — base case knocks over $P(1)$, inductive step ensures each knocks the next.

## Variants

| Type | Base | Inductive Step |
|------|------|----------------|
| **Standard** | $P(1)$ | $P(k) \Rightarrow P(k+1)$ |
| **Strong** | $P(1)$ | $P(1) \land \cdots \land P(k) \Rightarrow P(k+1)$ |
| **Starting at $n_0$** | $P(n_0)$ | $P(k) \Rightarrow P(k+1)$ for $k \ge n_0$ |
| **Two-step** | $P(1), P(2)$ | $P(k) \land P(k+1) \Rightarrow P(k+2)$ |

## Common Proof Templates

### 1. Summation Formulas
$\sum_{r=1}^n f(r) = F(n)$
**Inductive step:** $LHS_{k+1} = LHS_k + f(k+1) = F(k) + f(k+1) \to$ simplify to $F(k+1)$

### 2. Divisibility
Prove $a_n$ divisible by $m$.
**Inductive step:** $a_{k+1} = a_k \cdot \text{something} + \text{multiple of } m$

### 3. Inequalities
Prove $f(n) \ge g(n)$ or similar.
**Inductive step:** $f(k+1) = f(k) + \text{term} \ge g(k) + \text{term} \to$ show $\ge g(k+1)$

### 4. Recurrence Relations
Given $u_{n+1} = f(u_n)$, prove formula for $u_n$.
**Inductive step:** Substitute formula for $u_k$ into recurrence.

### 5. Matrix Powers
Prove $A^n = \text{formula}$.
**Inductive step:** $A^{k+1} = A^k \cdot A$

## Step-by-Step Structure

```
**Proof by Induction**

**Statement:** Let P(n) be "..." for n ∈ ℕ.

**Base Case (n = 1):**
LHS = ...
RHS = ...
LHS = RHS, so P(1) is true.

**Inductive Step:**
Assume P(k) is true for some k ∈ ℕ.
That is, [state P(k) explicitly].

We need to prove P(k+1): [state P(k+1) explicitly].

[Manipulation using P(k) to reach P(k+1)]

Therefore, P(k) ⇒ P(k+1).

**Conclusion:**
By the Principle of Mathematical Induction, P(n) is true for all n ∈ ℕ.
```

## Worked Examples

### Example 1: Summation — $\sum_{r=1}^n r^2 = \frac{n(n+1)(2n+1)}{6}$
**Base ($n=1$):** LHS = $1^2 = 1$, RHS = $\frac{1\cdot2\cdot3}{6} = 1$ ✓

**Assume $P(k)$:** $\sum_{r=1}^k r^2 = \frac{k(k+1)(2k+1)}{6}$

**Prove $P(k+1)$:**
$\sum_{r=1}^{k+1} r^2 = \frac{k(k+1)(2k+1)}{6} + (k+1)^2$
$= \frac{(k+1)[k(2k+1) + 6(k+1)]}{6}$
$= \frac{(k+1)(2k^2+7k+6)}{6}$
$= \frac{(k+1)(k+2)(2k+3)}{6}$
$= \frac{(k+1)((k+1)+1)(2(k+1)+1)}{6}$ ✓

### Example 2: Divisibility — $7^n - 2^n$ divisible by 5
**Base ($n=1$):** $7-2=5$ ✓

**Assume $P(k)$:** $7^k - 2^k = 5m$

**Prove $P(k+1)$:**
$7^{k+1} - 2^{k+1} = 7\cdot7^k - 2\cdot2^k$
$= 7(7^k - 2^k) + 7\cdot2^k - 2\cdot2^k$
$= 7(5m) + 5\cdot2^k = 5(7m + 2^k)$ ✓

### Example 3: Inequality — $2^n > n^2$ for $n \ge 5$
**Base ($n=5$):** $32 > 25$ ✓

**Assume $P(k)$:** $2^k > k^2$, $k \ge 5$

**Prove $P(k+1)$:**
$2^{k+1} = 2\cdot2^k > 2k^2$ (by hypothesis)
Need $2k^2 \ge (k+1)^2 = k^2 + 2k + 1$
$\iff k^2 - 2k - 1 \ge 0 \iff (k-1)^2 \ge 2$
True for $k \ge 3$, hence for $k \ge 5$ ✓

### Example 4: Recurrence — $u_1=2$, $u_{n+1}=3u_n+2$. Prove $u_n = 3^n - 1$.
**Base ($n=1$):** $3^1-1=2$ ✓

**Assume $P(k)$:** $u_k = 3^k - 1$

**Prove $P(k+1)$:**
$u_{k+1} = 3u_k + 2 = 3(3^k - 1) + 2 = 3^{k+1} - 3 + 2 = 3^{k+1} - 1$ ✓

### Example 5: Trigonometric — $\cos\theta \cdot \cos2\theta \cdots \cos2^{n-1}\theta = \frac{\sin2^n\theta}{2^n\sin\theta}$
**Base ($n=1$):** LHS = $\cos\theta$, RHS = $\frac{\sin2\theta}{2\sin\theta} = \frac{2\sin\theta\cos\theta}{2\sin\theta} = \cos\theta$ ✓

**Assume $P(k)$:** product up to $\cos2^{k-1}\theta = \frac{\sin2^k\theta}{2^k\sin\theta}$

**Prove $P(k+1)$:**
Multiply both sides by $\cos2^k\theta$:
LHS = product up to $\cos2^k\theta$
RHS = $\frac{\sin2^k\theta}{2^k\sin\theta}\cos2^k\theta = \frac{2\sin2^k\theta\cos2^k\theta}{2^{k+1}\sin\theta} = \frac{\sin2^{k+1}\theta}{2^{k+1}\sin\theta}$ ✓

## Problem Patterns (A/L)

| Pattern | Base | Inductive Strategy |
|---------|------|-------------------|
| $\sum f(r) = F(n)$ | $n=1$ | Add $f(k+1)$ to both sides, simplify |
| $a^n \pm b^n$ divisible | $n=1$ | Express $a^{k+1} \pm b^{k+1}$ using $a^k \pm b^k$ |
| $f(n) > g(n)$ | Find smallest $n_0$ | Use $f(k+1) = f(k) \times \text{something}$ |
| Recurrence $u_{n+1} = f(u_n)$ | $n=1$ | Substitute formula into recurrence |
| Matrix $A^n$ | $n=1$ | $A^{k+1} = A^k \cdot A$ |
| Trig product | $n=1$ | Multiply by next factor, use double-angle |

## Common Traps
- ❌ Not stating $P(n)$ clearly at start
- ❌ Forgetting base case (or checking wrong base)
- ❌ Inductive hypothesis: assuming what you need to prove
- ❌ Using $P(k+1)$ to prove $P(k)$ (backwards)
- ❌ Not simplifying to exactly the target form
- ❌ Divisibility: not showing the extra term is multiple of divisor
- ❌ Inequalities: not checking base case for the actual starting $n$
- ❌ "Assume true for all $k$" — should be "for some $k$"

## Cross-References
- [[Pure/01-Algebra/06-Permutations-Combinations.md]] — prove $\binom{n}{r}$ identities
- [[Pure/01-Algebra/07-Binomial-Theorem.md]] — prove $(1+x)^n$ expansion
- [[Pure/08-Sequences-Series/01-Arithmetic-Progression.md]] — sum formulas
- [[Pure/08-Sequences-Series/02-Geometric-Progression.md]] — sum formulas
- [[Pure/04-Calculus/11-Applications-DE.md]] — induction in recurrence solutions

## Quick Reference
**PMI Structure:**
1. Let $P(n)$ be "..." for $n \in \mathbb{N}$.
2. Base: $P(1)$ true because ...
3. Assume $P(k)$ true for some $k \in \mathbb{N}$.
4. Prove $P(k+1)$ using $P(k)$.
5. Conclude: $P(n)$ true $\forall n \in \mathbb{N}$.

**Strong Induction:** Assume $P(1), \ldots, P(k)$ all true, prove $P(k+1)$.
**Starting at $n_0$:** Base $P(n_0)$, then $P(k) \Rightarrow P(k+1)$ for $k \ge n_0$.