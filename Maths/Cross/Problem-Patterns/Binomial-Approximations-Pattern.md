---
date: 2026-08-16
type: problem-pattern
tags: [maths, pure, patterns, algebra]
parent: [[01-Algebra_Index]]
---

# Binomial Expansions & Approximations — Problem Patterns

The binomial theorem does two jobs: expand $(a+b)^n$ exactly when $n$ is a positive integer, and produce infinite series that approximate $(1+x)^\alpha$ when $\alpha$ is rational.

## Pattern 1: Integer powers

$(a+b)^n = \sum_{k=0}^{n} \binom{n}{k} a^{n-k} b^k$.

**Example:** Find the coefficient of $x^5$ in $(2 - 3x)^8$.

**Setup:** A specific term in an integer-power expansion.

**Solution:** General term: $\binom{8}{k} 2^{8-k}(-3x)^k$. Set $k=5$: $\binom{8}{5} 2^3 (-3)^5 = 56 \cdot 8 \cdot (-243) = -108864$.

**Key insight:** $\binom{n}{k}$ counts the ways; the minus sign from $(-3)^k$ is the easiest thing to drop — track it carefully.

## Pattern 2: Fractional / negative powers (validity!)

$(1+x)^\alpha = 1 + \alpha x + \frac{\alpha(\alpha-1)}{2!}x^2 + \cdots$, valid for $|x|<1$ when $\alpha$ is not a non-negative integer.

**Example:** Expand $(1-2x)^{-1/2}$ up to $x^3$.

**Setup:** Negative fractional exponent.

**Solution:** With $\alpha = -\tfrac12$ and $u = -2x$:
$1 + (-\tfrac12)(-2x) + \frac{(-\tfrac12)(-\tfrac32)}{2}(-2x)^2 + \frac{(-\tfrac12)(-\tfrac32)(-\tfrac52)}{6}(-2x)^3$
$= 1 + x + \tfrac32 x^2 + \tfrac52 x^3$. Valid for $|-2x|<1$, i.e. $|x|<\tfrac12$.

**Key insight:** The validity condition is inherited from the base $(1+u)^\alpha$ with $|u|<1$ — plug in whatever you substituted for $u$.

## Pattern 3: Approximation

**Example:** Approximate $\sqrt{0.98}$ to 3 decimal places.

**Setup:** Convert to $1 \pm$ (small).

**Solution:** $\sqrt{0.98} = \sqrt{1-0.02} = (1-0.02)^{1/2} \approx 1 - 0.01 - \frac{0.0004}{8} = 1 - 0.01 - 0.00005 = 0.98995 \approx 0.990$.

**Key insight:** Always rewrite the number as $1 + (\text{small})$ first — the series converges fast only then.

## Pattern 4: Comparing coefficients

**Example:** If $(1+ax)^{-1} = 1 - 2x + bx^2 + \cdots$, find $a$ and $b$.

**Setup:** Coefficient matching.

**Solution:** $(1+ax)^{-1} = 1 - ax + a^2x^2 - \cdots$. Match: $-a = -2 \Rightarrow a=2$, and $b = a^2 = 4$.

**Key insight:** Match term-by-term; each coefficient gives one equation.
