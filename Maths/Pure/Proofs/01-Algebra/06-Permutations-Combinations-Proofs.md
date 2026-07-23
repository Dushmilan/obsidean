---
date: 2026-07-19
type: proof
tags: [maths, pure, proof, algebra, permutations, combinations]
topic: [[Pure/01-Algebra/06-Permutations-Combinations.md]]
---

# Permutations & Combinations — Full Derivations

## Fundamental Principle of Counting

**Rule:** If event $A$ has $m$ outcomes and event $B$ has $n$ outcomes:
- $A$ **and** $B$ (both occur): $m \times n$ outcomes
- $A$ **or** $B$ (mutually exclusive): $m + n$ outcomes

**Proof:** For each of $m$ outcomes of $A$, there are $n$ outcomes of $B$. Total pairs: $m \times n$. $\square$

---

## Permutations

### $^nP_r = \frac{n!}{(n-r)!}$

**Proof:**
Choose first element: $n$ ways
Choose second: $n-1$ ways
...
Choose $r$-th: $n-r+1$ ways
Total: $n(n-1)\cdots(n-r+1) = \frac{n!}{(n-r)!}$. $\square$

### Circular Permutations: $(n-1)!$

**Proof:**
Fix one object as reference point (breaks rotation symmetry).
Arrange remaining $n-1$ in line: $(n-1)!$ ways. $\square$

### Circular with Reflection: $\frac{(n-1)!}{2}$ (for $n \ge 3$)

**Proof:**
Each arrangement counted twice (clockwise/anticlockwise equivalent).
Divide by 2. For $n=1,2$: $\frac{(n-1)!}{2}$ not integer; actual is 1. $\square$

### With Identical Objects: $\frac{n!}{n_1! n_2! \cdots n_k!}$

**Proof:**
If all $n$ objects distinct: $n!$ arrangements.
$n_1$ identical objects can be permuted in $n_1!$ ways without changing arrangement.
Similarly for other types. Total overcount: $\prod n_i!$. Divide. $\square$

---

## Combinations

### $\binom{n}{r} = \frac{n!}{r!(n-r)!}$

**Proof:**
$^nP_r = \binom{n}{r} \times r!$ (choose $r$ then order them)
$\binom{n}{r} = \frac{^nP_r}{r!} = \frac{n!}{(n-r)! r!}$. $\square$

### Symmetry: $\binom{n}{r} = \binom{n}{n-r}$

**Proof:**
$\binom{n}{r} = \frac{n!}{r!(n-r)!} = \frac{n!}{(n-r)! r!} = \binom{n}{n-r}$.
Combinatorial: Choosing $r$ to include = choosing $n-r$ to exclude. $\square$

### Pascal's Identity: $\binom{n}{r} + \binom{n}{r-1} = \binom{n+1}{r}$

**Proof (Algebraic):**
$\frac{n!}{r!(n-r)!} + \frac{n!}{(r-1)!(n-r+1)!} = n! \left[ \frac{n-r+1 + r}{r!(n-r+1)!} \right] = \frac{(n+1)!}{r!(n-r+1)!} = \binom{n+1}{r}$. $\square$

**Proof (Combinatorial):** Choose $r$ from $n+1$ items. Fix one item. Either it's chosen (choose $r-1$ from remaining $n$) or not (choose $r$ from remaining $n$). $\square$

---

## Sum of Binomial Coefficients

### $\sum_{r=0}^n \binom{n}{r} = 2^n$

**Proof:** $(1+1)^n = \sum \binom{n}{r} 1^r = \sum \binom{n}{r}$. $\square$

### Alternating Sum: $\sum_{r=0}^n (-1)^r \binom{n}{r} = 0$ ($n \ge 1$)

**Proof:** $(1-1)^n = 0$. $\square$

### Weighted Sum: $\sum_{r=0}^n r \binom{n}{r} = n 2^{n-1}$

**Proof:** Differentiate $(1+x)^n = \sum \binom{n}{r} x^r$:
$n(1+x)^{n-1} = \sum r \binom{n}{r} x^{r-1}$. Set $x=1$. $\square$

### Hockey-Stick: $\sum_{k=r}^n \binom{k}{r} = \binom{n+1}{r+1}$

**Proof (Combinatorial):** Choose $r+1$ from $n+1$. Let largest chosen be $k+1$. Choose $r$ from $\{1,\ldots,k\}$. $\square$

---

## Combinations with Repetitions

### $\binom{n+r-1}{r}$ = number of ways to choose $r$ from $n$ types (unlimited supply)

**Proof (Stars and Bars):**
Represent choices as $r$ stars $\star$ and $n-1$ bars $\mid$ separating types.
Total objects: $r + n - 1$. Choose $r$ positions for stars: $\binom{n+r-1}{r}$.
Equivalently, choose $n-1$ positions for bars: $\binom{n+r-1}{n-1}$. $\square$

---

## Inclusion-Exclusion Principle

### For two sets: $|A \cup B| = |A| + |B| - |A \cap B|$

### For three sets: $|A \cup B \cup C| = |A|+|B|+|C| - |A\cap B|-|A\cap C|-|B\cap C| + |A\cap B\cap C|$

### General:
$|\bigcup_{i=1}^n A_i| = \sum |A_i| - \sum |A_i \cap A_j| + \sum |A_i \cap A_j \cap A_k| - \cdots + (-1)^{n-1} |\bigcap A_i|$

**Proof:** An element in exactly $k$ sets is counted:
$\binom{k}{1} - \binom{k}{2} + \binom{k}{3} - \cdots + (-1)^{k-1}\binom{k}{k} = 1 - (1-1)^k = 1$ times. $\square$

---

## Derangements

### Number of permutations of $n$ with no fixed points: $!n = n! \sum_{k=0}^n \frac{(-1)^k}{k!}$

**Proof by Inclusion-Exclusion:**
Let $A_i$ = permutations fixing element $i$.
$|A_i| = (n-1)!$, $|A_i \cap A_j| = (n-2)!$, etc.
$!n = n! - \binom{n}{1}(n-1)! + \binom{n}{2}(n-2)! - \cdots = n! \sum_{k=0}^n \frac{(-1)^k}{k!}$. $\square$

**Limit:** $\lim_{n\to\infty} \frac{!n}{n!} = \frac{1}{e}$. $\square$

---

## Catalan Numbers

$C_n = \frac{1}{n+1} \binom{2n}{n}$

**Interpretations:**
- Valid parentheses sequences of length $2n$
- Paths from $(0,0)$ to $(n,n)$ not crossing diagonal
- Binary trees with $n+1$ leaves

**Recurrence:** $C_0 = 1$, $C_{n+1} = \sum_{k=0}^n C_k C_{n-k}$

**Proof of Formula (Reflection Principle):**
Total paths from $(0,0)$ to $(n,n)$: $\binom{2n}{n}$.
Bad paths crossing diagonal: reflect after first touch $\to$ paths to $(n-1, n+1)$: $\binom{2n}{n-1}$.
$C_n = \binom{2n}{n} - \binom{2n}{n-1} = \frac{1}{n+1}\binom{2n}{n}$. $\square$

---

## Multinomial Coefficient

$\frac{n!}{n_1! n_2! \cdots n_k!}$ where $\sum n_i = n$

**Proof:** Arrange $n$ items with $n_i$ of type $i$.
Total distinct arrangements: $n! / \prod n_i!$. $\square$

---

## Stirling Numbers

### First Kind $s(n,k)$: Permutations of $n$ with $k$ cycles
### Second Kind $S(n,k)$: Partitions of $n$ into $k$ non-empty subsets

**Recurrences:**
$s(n,k) = s(n-1,k-1) - (n-1)s(n-1,k)$
$S(n,k) = S(n-1,k-1) + k S(n-1,k)$

**Connection to powers:**
$x^n = \sum_{k=0}^n S(n,k) x(x-1)\cdots(x-k+1)$
$x(x-1)\cdots(x-n+1) = \sum_{k=0}^n s(n,k) x^k$

**Proof:** By induction using recurrences. $\square$