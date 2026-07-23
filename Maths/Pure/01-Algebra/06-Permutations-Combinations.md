---
date: 2026-07-19
type: concept
tags: [maths, pure, a-level, algebra, permutations, combinations]
parent: [[Pure/01-Algebra.md]]
proofs: [[Pure/Proofs/01-Algebra/06-Permutations-Combinations-Proofs.md]]
prerequisites: []
---

# Permutations & Combinations

## Fundamental Principle of Counting
If event $A$ can occur in $m$ ways and event $B$ in $n$ ways:
- **AND** (both occur): $m \times n$ ways
- **OR** (either occurs): $m + n$ ways (if mutually exclusive)

## Factorial Notation
$n! = n \times (n-1) \times \cdots \times 2 \times 1$
$0! = 1$
$(n+1)! = (n+1) \times n!$

## Permutations ($^nP_r$)
**Ordered arrangements** of $r$ objects from $n$ distinct objects.

$$^nP_r = \frac{n!}{(n-r)!} = n(n-1)\cdots(n-r+1)$$

| Situation | Formula |
|-----------|---------|
| All $n$ objects | $^nP_n = n!$ |
| $r$ objects from $n$ | $^nP_r = \frac{n!}{(n-r)!}$ |
| With repetitions allowed | $n^r$ |

### Special Cases
- **Circular permutations:** $(n-1)!$ (fix one position as reference)
- **Circular with reflections identical:** $\frac{(n-1)!}{2}$ (necklaces, bracelets)
- **Permutations with identical objects:** $\frac{n!}{n_1! n_2! \cdots n_k!}$

## Combinations ($^nC_r$ or $\binom{n}{r}$)
**Selections** where order doesn't matter.

$$^nC_r = \binom{n}{r} = \frac{n!}{r!(n-r)!} = \frac{^nP_r}{r!}$$

### Properties
- Symmetry: $\binom{n}{r} = \binom{n}{n-r}$
- Pascal's identity: $\binom{n}{r} + \binom{n}{r-1} = \binom{n+1}{r}$
- Sum: $\sum_{r=0}^n \binom{n}{r} = 2^n$
- Alternating sum: $\sum_{r=0}^n (-1)^r \binom{n}{r} = 0$

## Key Differences

| | Permutation | Combination |
|---|-------------|-------------|
| **Order** | Matters | Doesn't matter |
| **Formula** | $^nP_r$ | $^nC_r$ |
| **Relation** | $^nP_r = ^nC_r \times r!$ | $^nC_r = ^nP_r / r!$ |
| **Keywords** | Arrange, order, sequence, rank | Select, choose, committee, group |

## Problem Types

### 1. Arrangements with Restrictions
- **Together:** Treat as single unit $\to$ arrange units $\times$ arrange within
- **Not together:** Total - together
- **Alternating:** Arrange one type first, place others in gaps

### 2. Selections with Restrictions
- **At least one:** Total - none
- **Exactly $r$:** Select $r$ from type A, rest from type B
- **Inclusion/Exclusion:** Use Venn diagram logic

### 3. Circular Arrangements
- **$n$ distinct:** $(n-1)!$
- **If reflections identical:** $\frac{(n-1)!}{2}$

### 4. Distribution Problems
- **Distinct objects to distinct boxes:** $n^r$ (repetition allowed)
- **Identical objects to distinct boxes (stars & bars):** $\binom{n+r-1}{r-1}$

## Worked Examples

### Example 1: Arrange "MATHEMATICS" with vowels together
Letters: M(2), A(2), T(2), H(1), E(1), I(1), C(1), S(1)
Vowels: A, A, E, I (4 letters, A repeated)
Treat vowels as 1 block: block + 7 consonants = 8 items
Arrangements: $\frac{8!}{2!2!} \times \frac{4!}{2!} = \frac{40320}{4} \times 12 = 120960$

### Example 2: Committee of 5 from 7 men, 6 women with at least 3 women
$\binom{6}{3}\binom{7}{2} + \binom{6}{4}\binom{7}{1} + \binom{6}{5}\binom{7}{0} = 20\cdot21 + 15\cdot7 + 6\cdot1 = 420 + 105 + 6 = 531$

### Example 3: Number of diagonals in $n$-gon
Select 2 vertices: $\binom{n}{2}$
Subtract $n$ sides: $\binom{n}{2} - n = \frac{n(n-3)}{2}$

### Example 4: Paths on grid from $(0,0)$ to $(m,n)$ (only right/up)
$m$ rights, $n$ ups: $\binom{m+n}{m}$ or $\binom{m+n}{n}$

## Problem Patterns (A/L)

| Pattern | Keywords | Approach |
|---------|----------|----------|
| Arrange with some together | "together", "adjacent" | Group as unit |
| Arrange with some apart | "separated", "not together" | Total - together |
| Circular | "round table", "necklace" | $(n-1)!$ or $(n-1)!/2$ |
| Select committee | "choose", "select", "team" | Combinations |
| Distribution | "distribute", "allocate" | Stars & bars or $n^r$ |
| Paths | "grid", "routes" | $\binom{m+n}{m}$ |

## Common Traps
- ❌ Confusing $^nP_r$ and $^nC_r$ (order matters?)
- ❌ Forgetting $0! = 1$
- ❌ Overcounting identical objects
- ❌ Circular: forgetting to divide by $n$ for rotation, or by 2 for reflection
- ❌ "At least one" = total - none (not sum of individual)
- ❌ Stars & bars: identical objects, distinct boxes

## Cross-References
- [[Pure/01-Algebra/07-Binomial-Theorem.md]] — coefficients are $\binom{n}{r}$
- [[Pure/01-Algebra/08-Mathematical-Induction.md]] — prove combinatorial identities
- [[Pure/08-Sequences-Series/03-Summation-Notation.md]] — sums of binomial coefficients
- [[Physics/06-Modern-Physics/05-Semiconductors.md]] — Fermi-Dirac statistics (combinations)

## Quick Reference
**Permutation:** $^nP_r = \frac{n!}{(n-r)!}$
**Combination:** $^nC_r = \frac{n!}{r!(n-r)!}$
**Relation:** $^nP_r = ^nC_r \cdot r!$
**Circular (distinct):** $(n-1)!$
**Circular (necklace):** $\frac{(n-1)!}{2}$
**With repetitions:** $n^r$
**Stars & bars (identical to distinct):** $\binom{n+r-1}{r-1}$
**Multinomial:** $\frac{n!}{n_1! n_2! \cdots n_k!}$