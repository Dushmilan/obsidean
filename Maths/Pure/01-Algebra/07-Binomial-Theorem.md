---
date: 2026-07-19
type: concept
tags: [maths, pure, a-level, algebra, binomial-theorem]
parent: [[Pure/01-Algebra.md]]
proofs: [[Pure/Proofs/01-Algebra/07-Binomial-Theorem-Proofs.md]]
prerequisites: [[Pure/01-Algebra/06-Permutations-Combinations.md]]
---

# Binomial Theorem

## Positive Integer Index

For $n \in \mathbb{N}$:
$$(x+y)^n = \sum_{r=0}^n \binom{n}{r} x^{n-r} y^r = \binom{n}{0}x^n + \binom{n}{1}x^{n-1}y + \cdots + \binom{n}{n}y^n$$

**General term:** $T_{r+1} = \binom{n}{r} x^{n-r} y^r$, $r = 0, 1, \ldots, n$

**Properties:**
- Number of terms: $n+1$
- Powers of $x$ decrease, powers of $y$ increase
- Coefficients symmetric: $\binom{n}{r} = \binom{n}{n-r}$
- Sum of coefficients: $(1+1)^n = 2^n$
- Alternating sum: $(1-1)^n = 0$

## Pascal's Triangle
```
n=0:              1
n=1:            1   1
n=2:          1   2   1
n=3:        1   3   3   1
n=4:      1   4   6   4   1
n=5:    1   5  10  10   5   1
```
Each entry = sum of two above: $\binom{n}{r} = \binom{n-1}{r-1} + \binom{n-1}{r}$

## Rational Index (Generalised Binomial Theorem)

For $|x| < 1$ and $n \in \mathbb{Q}$:
$$(1+x)^n = 1 + nx + \frac{n(n-1)}{2!}x^2 + \frac{n(n-1)(n-2)}{3!}x^3 + \cdots = \sum_{r=0}^\infty \binom{n}{r} x^r$$

Where generalised binomial coefficient:
$$\binom{n}{r} = \frac{n(n-1)(n-2)\cdots(n-r+1)}{r!} \quad (r \ge 1), \quad \binom{n}{0} = 1$$

**Convergence:** $|x| < 1$ (radius of convergence = 1)

## Standard Expansions (Memorise)

| Expansion | Formula | Valid for |
|-----------|---------|-----------|
| $(1+x)^{-1}$ | $1 - x + x^2 - x^3 + \cdots$ | $|x| < 1$ |
| $(1-x)^{-1}$ | $1 + x + x^2 + x^3 + \cdots$ | $|x| < 1$ |
| $(1+x)^{-2}$ | $1 - 2x + 3x^2 - 4x^3 + \cdots$ | $|x| < 1$ |
| $(1-x)^{-2}$ | $1 + 2x + 3x^2 + 4x^3 + \cdots$ | $|x| < 1$ |
| $(1+x)^{-3}$ | $1 - 3x + 6x^2 - 10x^3 + \cdots$ | $|x| < 1$ |
| $(1+x)^{1/2}$ | $1 + \frac{1}{2}x - \frac{1}{8}x^2 + \frac{1}{16}x^3 - \cdots$ | $|x| < 1$ |
| $(1+x)^{-1/2}$ | $1 - \frac{1}{2}x + \frac{3}{8}x^2 - \frac{5}{16}x^3 + \cdots$ | $|x| < 1$ |

## General Term for Rational $n$
$T_{r+1} = \binom{n}{r} x^r = \frac{n(n-1)\cdots(n-r+1)}{r!} x^r$

**First few terms:**
- $T_1 = 1$
- $T_2 = nx$
- $T_3 = \frac{n(n-1)}{2}x^2$
- $T_4 = \frac{n(n-1)(n-2)}{6}x^3$

## Approximations

For small $x$, $(1+x)^n \approx 1 + nx$ (linear approximation)
Better: $(1+x)^n \approx 1 + nx + \frac{n(n-1)}{2}x^2$

**Example:** $\sqrt{1.02} = (1+0.02)^{1/2} \approx 1 + \frac{1}{2}(0.02) - \frac{1}{8}(0.02)^2 = 1.01 - 0.00005 = 1.00995$

## Applications

### 1. Finding Specific Coefficient
Expand $(2x - 3/x)^{10}$, find coefficient of $x^4$.
$T_{r+1} = \binom{10}{r} (2x)^{10-r} (-3/x)^r = \binom{10}{r} 2^{10-r} (-3)^r x^{10-2r}$
$10-2r = 4 \Rightarrow r = 3$
Coefficient = $\binom{10}{3} 2^7 (-3)^3 = 120 \cdot 128 \cdot (-27) = -414720$

### 2. Binomial Expansion with Partial Fractions
Expand $\frac{1}{(1+x)(1-x)^2}$ for small $x$.
Partial fractions: $\frac{A}{1+x} + \frac{B}{1-x} + \frac{C}{(1-x)^2}$
$A = 1/4, B = 1/4, C = 1/2$
$\frac{1}{4}(1-x+x^2-\cdots) + \frac{1}{4}(1+x+x^2+\cdots) + \frac{1}{2}(1+2x+3x^2+\cdots)$
$= 1 + x + \frac{3}{2}x^2 + \cdots$

### 3. Approximate Roots
$\sqrt[3]{1000.03} = \sqrt[3]{1000(1+0.00003)} = 10(1+0.00003)^{1/3} \approx 10(1 + \frac{0.00003}{3}) = 10.0001$

## Problem Patterns (A/L)

| Pattern | Approach |
|---------|----------|
| Expand $(a+bx)^n$ | Factor out $a^n$: $a^n(1+\frac{b}{a}x)^n$ |
| Find coefficient of $x^k$ | Match exponent: $n-r = k$ or use general term |
| Approximate value | Use $(1+x)^n \approx 1+nx$ for small $x$ |
| Sum of series | Recognise as binomial expansion at $x=1$ or $x=-1$ |
| Prove identity | Compare coefficients or use combinatorial argument |
| Validity range | $|x| < 1$ for rational $n$; all $x$ for integer $n$ |

## Worked Examples

### Example 1: Expand $(2-3x)^5$ up to $x^3$
$(2-3x)^5 = 2^5(1 - \frac{3}{2}x)^5 = 32[1 + 5(-\frac{3}{2}x) + 10(-\frac{3}{2}x)^2 + 10(-\frac{3}{2}x)^3 + \cdots]$
$= 32[1 - \frac{15}{2}x + \frac{90}{4}x^2 - \frac{270}{8}x^3 + \cdots]$
$= 32 - 240x + 720x^2 - 1080x^3 + \cdots$

### Example 2: $\frac{1+x}{(1-x)^3}$ up to $x^2$
$(1-x)^{-3} = 1 + 3x + 6x^2 + \cdots$
$(1+x)(1+3x+6x^2+\cdots) = 1 + 4x + 9x^2 + \cdots$

### Example 3: Find $x$ such that $(1+2x)^{1/2} = 1.02$ (approx)
$1 + \frac{1}{2}(2x) - \frac{1}{8}(2x)^2 + \cdots = 1.02$
$1 + x - \frac{1}{2}x^2 \approx 1.02$
$x - 0.5x^2 \approx 0.02$
$x \approx 0.02$ (first approximation)
Better: $x \approx 0.0202$

## Common Traps
- ❌ Forgetting validity condition $|x| < 1$ for rational $n$
- ❌ Using integer formula for rational index
- ❌ Wrong general term sign for $(a-bx)^n$
- ❌ Not factoring out constant before expansion
- ❌ Confusing $T_{r+1}$ index (starts at $r=0$)

## Cross-References
- [[Pure/01-Algebra/06-Permutations-Combinations.md]] — coefficients are $\binom{n}{r}$
- [[Pure/01-Algebra/08-Mathematical-Induction.md]] — prove binomial identities
- [[Pure/04-Calculus/07-Integration-Techniques.md]] — series integration
- [[Pure/08-Sequences-Series/06-Power-Series.md]] — binomial series as power series
- [[Physics/01-Measurement/04-Dimensional-Analysis.md]] — approximations

## Quick Reference
**Integer $n$:** $(x+y)^n = \sum_{r=0}^n \binom{n}{r} x^{n-r} y^r$, all $x,y$
**Rational $n$:** $(1+x)^n = \sum_{r=0}^\infty \binom{n}{r} x^r$, $|x| < 1$
**General term:** $T_{r+1} = \binom{n}{r} x^{n-r} y^r$ (integer) or $\binom{n}{r} x^r$ (rational)
**$\binom{n}{r}$ (rational):** $\frac{n(n-1)\cdots(n-r+1)}{r!}$
**Validity:** Integer $n$ — all real $x,y$; Rational $n$ — $|x| < 1$