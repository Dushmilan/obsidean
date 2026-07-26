---
date: 2026-07-19
type: proof
tags: [maths, pure, proof, algebra, induction]
parent: [[Pure/01-Algebra/08-Mathematical-Induction]]
---

# Mathematical Induction — Full Derivations & Advanced Forms

## Principle of Mathematical Induction (PMI)

**Axiom (Peano):** Let $P(n)$ be a statement for each $n \in \mathbb{N}$. If:
1. $P(1)$ is true (base case)
2. $P(k) \Rightarrow P(k+1)$ for all $k \in \mathbb{N}$ (inductive step)

Then $P(n)$ is true for all $n \in \mathbb{N}$.

**Proof of PMI from Well-Ordering Principle:**
Assume PMI is false. Let $S = \{n \in \mathbb{N} : P(n) \text{ is false}\}$. $S \neq \emptyset$ (since PMI false means not all true).
By Well-Ordering, $S$ has a least element $m$. $m \neq 1$ because $P(1)$ true.
Then $m-1 \in \mathbb{N}$ and $m-1 \notin S$ (by minimality), so $P(m-1)$ is true.
But $P(m-1) \Rightarrow P(m)$ by inductive step, so $P(m)$ true $\Rightarrow m \notin S$. Contradiction. $\square$

---

## Strong Induction (Complete Induction)

**Principle:** If $P(1)$ true and $[P(1) \land P(2) \land \cdots \land P(k)] \Rightarrow P(k+1)$ for all $k \ge 1$, then $P(n)$ true $\forall n$.

**Proof:** Define $Q(n) = P(1) \land P(2) \land \cdots \land P(n)$.
$Q(1) = P(1)$ true.
Assume $Q(k)$ true. Then $P(1),\ldots,P(k)$ true. By hypothesis, $P(k+1)$ true.
So $Q(k+1) = Q(k) \land P(k+1)$ true.
By standard PMI, $Q(n)$ true $\forall n$. Thus $P(n)$ true $\forall n$. $\square$

---

## Induction Starting at $n_0$

**Principle:** If $P(n_0)$ true and $P(k) \Rightarrow P(k+1)$ for $k \ge n_0$, then $P(n)$ true $\forall n \ge n_0$.

**Proof:** Define $Q(n) = P(n+n_0-1)$. Then $Q(1) = P(n_0)$ true.
$Q(k) \Rightarrow P(k+n_0-1) \Rightarrow P(k+n_0) = Q(k+1)$.
By PMI, $Q(n)$ true $\forall n \Rightarrow P(n)$ true $\forall n \ge n_0$. $\square$

---

## Two-Step Induction

For recurrences like $u_{n+2} = f(u_{n+1}, u_n)$:
Prove $P(1)$ and $P(2)$, then $[P(k) \land P(k+1)] \Rightarrow P(k+2)$.

**Proof:** Define $Q(n) = P(2n-1) \land P(2n)$. Show $Q(1)$ true and $Q(k) \Rightarrow Q(k+1)$. $\square$

---

## Cauchy Induction (Forward-Backward)

**Steps:**
1. Base: $P(1)$ true
2. Forward: $P(k) \Rightarrow P(2k)$ for all $k$
3. Backward: $P(k) \Rightarrow P(k-1)$ for all $k \ge 2$

**Proof:** By forward, $P(2^m)$ true for all $m$.
For any $n$, choose $m$ such that $2^m > n$. By backward, $P(2^m) \Rightarrow P(2^m-1) \Rightarrow \cdots \Rightarrow P(n)$. $\square$

**Use case:** AM-GM inequality: $\frac{x_1+\cdots+x_n}{n} \ge \sqrt[n]{x_1\cdots x_n}$

---

## Structural Induction

For recursively defined structures (trees, formulas, etc.):
1. Base cases: Prove for atomic/primitive elements.
2. Inductive step: If true for substructures, prove for compound structure.

**Example:** All well-formed formulas (wffs) have equal number of '(' and ')'.
Base: Atomic formulas $p, q, \ldots$ have 0 '(' and 0 ')'.
Inductive: If $\varphi$ has $m$ '(' and $m$ ')', and $\psi$ has $n$ '(' and $n$ ')',
then $(\varphi \land \psi)$ has $m+n+1$ '(' and $m+n+1$ ')'. $\square$

---

## Common Induction Proof Templates

### Template 1: Summation Identity
$\sum_{r=1}^n f(r) = F(n)$

**Base:** $\sum_{r=1}^1 f(r) = f(1) = F(1)$.
**Assume:** $\sum_{r=1}^k f(r) = F(k)$.
**Step:** $\sum_{r=1}^{k+1} f(r) = F(k) + f(k+1)$. Simplify to $F(k+1)$.

---

### Template 2: Divisibility
$a_n$ divisible by $m$

**Base:** $a_1 = m \cdot \text{integer}$.
**Assume:** $a_k = m \cdot t$.
**Step:** $a_{k+1} = a_k \cdot A + m \cdot B$ or express in terms of $a_k$ plus multiple of $m$.

---

### Template 3: Inequality
$f(n) \le g(n)$ for $n \ge n_0$

**Base:** $f(n_0) \le g(n_0)$.
**Assume:** $f(k) \le g(k)$.
**Step:** $f(k+1) = f(k) + \Delta f \le g(k) + \Delta f$. Show $\Delta f \le g(k+1) - g(k)$.

---

### Template 4: Recurrence Relation
Given $u_{n+1} = f(u_n)$, prove $u_n = F(n)$.

**Base:** $u_1 = F(1)$.
**Assume:** $u_k = F(k)$.
**Step:** $u_{k+1} = f(u_k) = f(F(k))$. Show $= F(k+1)$.

---

### Template 5: Matrix Powers
$A^n = M_n$

**Base:** $A^1 = M_1$.
**Assume:** $A^k = M_k$.
**Step:** $A^{k+1} = A^k \cdot A = M_k \cdot A$. Compute to get $M_{k+1}$.

---

### Template 6: Trigonometric Identities
$\prod_{r=1}^n \cos(2^{r-1}\theta) = \frac{\sin 2^n \theta}{2^n \sin \theta}$

**Base ($n=1$):** $\cos\theta = \frac{\sin 2\theta}{2\sin\theta}$. True by double-angle.
**Assume:** True for $k$.
**Step:** Multiply both sides by $\cos 2^k \theta$:
LHS: product up to $2^k \theta$.
RHS: $\frac{\sin 2^k \theta}{2^k \sin \theta} \cos 2^k \theta = \frac{2\sin 2^k \theta \cos 2^k \theta}{2^{k+1} \sin \theta} = \frac{\sin 2^{k+1} \theta}{2^{k+1} \sin \theta}$.

---

## Proof of Induction Validity (Meta)

### From Axiom of Infinity + Separation (ZFC)
$\mathbb{N}$ is the smallest inductive set (contains 0, closed under successor).
Inductive step ensures $P$ is closed under successor.
Base ensures $0 \in P$.
By minimality of $\mathbb{N}$, $\mathbb{N} \subseteq P$. $\square$

### From Well-Ordering Principle (Equivalent)
Every non-empty subset of $\mathbb{N}$ has a least element.
Assume $P$ not true for all $n$. Let $S = \{n : \neg P(n)\} \neq \emptyset$.
Let $m = \min S$. $m \neq 1$ (base case). So $m-1 \in \mathbb{N}$.
$m-1 \notin S \Rightarrow P(m-1)$ true. But $P(m-1) \Rightarrow P(m)$, contradiction. $\square$

---

## Common Mistakes in Induction Proofs

1. **Assuming $P(k+1)$ to prove $P(k)$** (backwards)
2. **Not verifying base case** (or wrong base case)
3. **Using "true for all $k$" instead of "true for some $k$"**
4. **Inductive step fails for small $k$** (e.g., $P(1) \not\Rightarrow P(2)$)
5. **Algebraic errors** in simplifying to target form
5. **Circular reasoning** — using $P(k+1)$ in proof of $P(k+1)$

---

## Example: Why "Assume for all $k$" is Wrong

**False proof that all horses are same color:**
$P(n)$: In any set of $n$ horses, all same color.
$P(1)$: True.
Assume $P(k)$ true for all $k \le n$.
Consider $n+1$ horses. Remove 1: $n$ horses all same color by $P(n)$.
Remove different 1: $n$ horses all same color.
Therefore all $n+1$ same color. ✓

**Flaw:** "Assume for all $k$" is stronger than needed, but the real flaw is $P(1) \not\Rightarrow P(2)$: two horses, no overlap between the two $n=1$ sets. The inductive step fails for $k=1 \to 2$.

**Correct inductive hypothesis:** "Assume $P(k)$ is true for **some** $k \in \mathbb{N}$" (or "for an arbitrary but fixed $k$").