# 1.6 Permutations & Combinations

Permutations count ordered arrangements; combinations count unordered selections. The distinction is simple but critical: permutations care about order, combinations do not. This single difference changes which formula you use and which answer you get. The Fundamental Principle of Counting underpins everything: AND means multiply, OR means add (for mutually exclusive events). Every combinatorial problem reduces to breaking it into AND/OR components.

**The Intuition:** Permutations are like assigning seats in a row — who sits where matters. Combinations are like forming a committee — who is in it matters, not where they sit. $^nP_r = \frac{n!}{(n-r)!}$ counts ways to fill $r$ slots from $n$ options where each slot reduces the pool by one. $^nC_r = \frac{^nP_r}{r!}$ divides out the overcounting from different orderings of the same group.

**The Math:**

- **Factorial:** $n! = n \times (n-1) \times \cdots \times 1$, $0! = 1$
- **Permutation:** $^nP_r = \frac{n!}{(n-r)!} = n(n-1)\cdots(n-r+1)$
- **Combination:** $^nC_r = \binom{n}{r} = \frac{n!}{r!(n-r)!} = \frac{^nP_r}{r!}$
- **Circular permutations:** $(n-1)!$; with reflections identical: $\frac{(n-1)!}{2}$
- **Identical objects:** $\frac{n!}{n_1! n_2! \cdots n_k!}$
- **Stars and bars:** $\binom{n+r-1}{r-1}$ distributions of $n$ identical objects into $r$ bins
- **Grid paths** from $(0,0)$ to $(m,n)$: $\binom{m+n}{m}$

**What does this mean for Pure Mathematics?** Combinatorics is the foundation of probability, statistics, and discrete mathematics. Getting the count wrong means getting the probability wrong. The "at least one" trick (total minus none) and "together" trick (treat group as unit) are the most common problem variants.

### Example 1: Arrange "MATHEMATICS" with vowels together

**Setup:** 11 letters with repeats: M(2), A(2), T(2), H(1), E(1), I(1), C(1), S(1).

**Solution:** Vowels: A, A, E, I. Treat vowels as 1 block: block + 7 consonants = 8 items. Arrange 8 items: $\frac{8!}{2!2!}$. Arrange vowels within block: $\frac{4!}{2!}$. Total: $\frac{8!}{2!2!} \times \frac{4!}{2!} = 120{,}960$.

**Key insight:** "Together" problems: treat the group as a single unit, then arrange within the group.

### Example 2: Committee of 5 from 7 men, 6 women with at least 3 women

**Setup:** Selection with a minimum constraint.

**Solution:** Cases: exactly 3, 4, or 5 women. $\binom{6}{3}\binom{7}{2} + \binom{6}{4}\binom{7}{1} + \binom{6}{5}\binom{7}{0} = 420 + 105 + 6 = 531$.

**Key insight:** "At least" problems: list all valid cases and sum them.

### Example 3: Paths on grid from $(0,0)$ to $(m,n)$

**Setup:** A grid where you can only move right or up.

**Solution:** Need $m$ rights and $n$ ups: total $m+n$ moves. Choose which $m$ are rights: $\binom{m+n}{m}$.

**Key insight:** Grid path problems reduce to choosing positions for one type of move.

---
