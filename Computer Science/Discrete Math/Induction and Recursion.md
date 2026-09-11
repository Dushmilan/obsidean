# Induction & Recursion

Mathematical induction proves statements about all natural numbers — or more generally, about any recursively defined structure. It's the mathematical twin of **recursion** in programming: the base case and the recursive step are the same idea in two languages.

**The Intuition:** Induction is like dominoes: if you (1) push the first domino, and (2) every domino knocks down the next one, then *all* dominoes fall. Step 1 is the **base case**; step 2 is the **inductive step**. You never check each domino — the general argument covers them all.

## The template

To prove $P(n)$ for all $n \ge n_0$:

```text
1. BASE CASE:   Show P(n₀) is true.                     (push domino 1)
2. INDUCTIVE STEP: Assume P(k) for some k ≥ n₀.          (the "domino" is falling)
   Show P(k+1) follows.
   The assumption P(k) is called the INDUCTIVE HYPOTHESIS.
3. Conclude: by the principle of induction, P(n) holds for all n ≥ n₀.
```

## Example — the classic sum

**Setup:** Prove $1 + 2 + \cdots + n = \frac{n(n+1)}{2}$ for all $n \ge 1$.

**Solution:**
```text
Base: n = 1 → LHS = 1, RHS = 1·2/2 = 1. ✓

Inductive step: assume 1 + ... + k = k(k+1)/2.   (IH)
For k+1:  1 + ... + k + (k+1)
        = k(k+1)/2 + (k+1)
        = (k+1)(k/2 + 1)
        = (k+1)(k+2)/2  ✓   — exactly the formula for n = k+1.
```
∎

**Key insight:** The inductive step is a *calculation*: use the assumption for k, then add one term and algebraically reshape into the formula for k+1.

## Induction on inequality

**Setup:** Prove $2^n \ge n^2$ for all $n \ge 4$.

**Solution:**
```text
Base: n = 4 → 2⁴ = 16 ≥ 16 ✓
Step: assume 2^k ≥ k² (k ≥ 4).
  2^(k+1) = 2·2^k ≥ 2k²            (by IH)
  Need: 2k² ≥ (k+1)² = k² + 2k + 1
  i.e., k² - 2k - 1 ≥ 0, true for k ≥ 4. ✓
```
∎

**Key insight:** Inequalities need an extra "bounding" step — you often must prove the IH gives you *more than enough* and then trim.

## Strong induction — the more powerful sibling

Sometimes $P(k+1)$ needs not just $P(k)$ but several earlier cases:
```text
Assume P(n₀), P(n₀+1), ..., P(k) all hold.   (strong IH)
Show P(k+1).
```

**Setup:** Every integer $n \ge 2$ is a product of primes.

**Solution:**
```text
Base: n = 2 is prime. ✓
Strong step: assume every integer 2..k is a product of primes.
For k+1: if prime, done. If composite, k+1 = a·b with 2 ≤ a,b ≤ k,
so by the strong IH both factor into primes — their product does too. ✓
```

**Key insight:** Composite factorization *needs* both factors, not just $k$ — strong induction supplies them. This is the Fundamental Theorem of Arithmetic.

## Structural induction — induction on structures, not numbers

Induction applies to any **recursively defined structure**: lists, trees, grammars. Prove the property for the base structures, then show it survives the recursive constructor.

**Setup:** Prove a binary tree with $n$ nodes has $n-1$ edges.

**Solution:**
```text
Base: one node (n=1) → 0 edges = n-1 ✓.
Step: a tree with root + left subtree (n_L nodes, n_L - 1 edges by IH)
+ right subtree (n_R nodes, n_R - 1 edges by IH).
Total nodes n = 1 + n_L + n_R. Edges = (n_L - 1) + (n_R - 1) + 2 = n - 1. ✓
```
∎

**Key insight:** The structural IH applies to the *subtrees* — smaller instances of the same structure. This is exactly how recursive functions on trees (height, traversal, sum) are proven correct.

## Loop invariants — induction on code

A **loop invariant** is a property true before each iteration. Prove it by induction on the iteration count — that's exactly what "the loop is correct" means.

**Setup:** Prove `sum` computes $\sum_{i=0}^{n-1} a[i]$ after this loop:
```c
sum = 0;
for (i = 0; i < n; i++) sum += a[i];
```

**Solution:** Invariant: *before iteration i, sum = a[0]+...+a[i-1].*
```text
Base: before i=0, sum = 0 = empty sum ✓.
Step: before i=k, sum = a[0]+...+a[k-1]; the iteration adds a[k] →
before i=k+1, sum = a[0]+...+a[k] ✓.
After i=n (loop exits), sum = a[0]+...+a[n-1]. ∎
```

**Key insight:** The invariant + "the loop ends" + "at the end the invariant gives the answer" = correctness proof. This is induction wearing a disguise — the single most useful proof technique for programmers.

---

**Setup:** Prove $1^2 + 2^2 + \cdots + n^2 = \frac{n(n+1)(2n+1)}{6}$.

**Solution:**
```text
Base: n=1 → 1 = 1·2·3/6 = 1 ✓.
Step: assume sum to k = k(k+1)(2k+1)/6.
  Sum to k+1 = k(k+1)(2k+1)/6 + (k+1)²
             = (k+1)[k(2k+1)/6 + (k+1)]
             = (k+1)[(2k² + k + 6k + 6)/6]
             = (k+1)(2k² + 7k + 6)/6
             = (k+1)(k+2)(2k+3)/6 ✓   (factored)
```

**Key insight:** The algebra "rhs(k) + (k+1)²" always factors back to the closed form at k+1. If it doesn't factor cleanly, your formula is probably wrong — a nice self-check.

---

**Setup:** Prove that any amount $\ge$ 4 cents can be made with 3¢ and 5¢ coins.

**Solution:**
```text
Base: 4 = 3+... no. 4 can't!? 4 = 5-1 no. Hmm: 4¢ is impossible with 3s and 5s.
Adjust: prove for n ≥ 8 instead:
Base: 8 = 3+5 ✓, 9 = 3+3+3 ✓, 10 = 5+5 ✓.
Step: assume all of 8..k possible. k+1 = (k-2) + 3; k-2 ≥ 8 → by IH possible, add a 3¢. ✓
```

**Key insight:** When the base case fails, *shift the claim*. The "add one coin to a smaller value" trick needs multiple base cases to start the chain — a classic strong-induction application.

---

## Practice (try before peeking)

1. Prove $3^n > n^3$ for $n \ge 4$.
2. Why can't ordinary induction prove "all heights ≤ k" for a claim needing k-1 and k-2?
3. A program loops `i` from 0 to n computing product. What's the invariant?

<details><summary>Answers</summary>

1. Base n=4: 81 > 64 ✓. Step: $3^{k+1} = 3·3^k > 3k^3$; need $3k^3 > (k+1)^3$, i.e. $k^3 > k^2 + k + ...$ — verify for k≥4 and you're done.
2. Because $P(k)$ alone doesn't reach back to $k-2$. Strong induction (assuming all earlier) is required.
3. *Before iteration i, product = a[0]·...·a[i-1]* — at the end it equals the full product.

</details>

---

**Common traps:**
- Skipping the base case — the whole chain falls
- Proving P(k+1) without *using* the inductive hypothesis — that's just a direct proof
- Induction on the wrong variable — the structure must shrink (subtrees, n-1, smaller integers)
- Assuming the claim in the step is "begging the question" — the IH is an *assumption* you're licensed to use, not a free conclusion
- Forgetting that strong induction still needs base cases

---
