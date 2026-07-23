---
date: 2026-07-19
type: concept
tags: [maths, pure, a-level, algebra, partial-fractions]
parent: [[Pure/01-Algebra.md]]
proofs: [[Pure/Proofs/01-Algebra/05-Partial-Fractions-Proofs.md]]
prerequisites: [[Pure/01-Algebra/04-Polynomials.md]]
---

# Partial Fractions

## Purpose
Decompose a rational function $\frac{P(x)}{Q(x)}$ into simpler fractions for integration, binomial expansion, or solving DEs.

**Requirement:** $\deg(P) < \deg(Q)$ (proper fraction). If improper, do polynomial long division first.

## Decomposition Rules

| Factor in $Q(x)$ | Partial Fraction Form |
|------------------|----------------------|
| Linear $(ax+b)$ | $\frac{A}{ax+b}$ |
| Repeated linear $(ax+b)^n$ | $\frac{A_1}{ax+b} + \frac{A_2}{(ax+b)^2} + \cdots + \frac{A_n}{(ax+b)^n}$ |
| Irreducible quadratic $(ax^2+bx+c)$ | $\frac{Ax+B}{ax^2+bx+c}$ |
| Repeated irreducible quadratic $(ax^2+bx+c)^n$ | $\frac{A_1x+B_1}{ax^2+bx+c} + \frac{A_2x+B_2}{(ax^2+bx+c)^2} + \cdots$ |

**Irreducible quadratic:** $b^2 - 4ac < 0$

## Methods to Find Constants

### 1. Substitution (Cover-up) Method
For distinct linear factors:
- $\frac{P(x)}{(x-a)(x-b)\cdots} = \frac{A}{x-a} + \frac{B}{x-b} + \cdots$
- $A = \frac{P(a)}{(a-b)\cdots}$ (cover up $x-a$ and substitute $x=a$)

### 2. Equating Coefficients
Multiply both sides by denominator, expand, equate coefficients of $x^k$.

### 3. Substitution of Convenient Values
Pick $x$ values that simplify equation (e.g., $x=0, 1, -1$).

### 4. Limit Method (for repeated factors)
For $\frac{P(x)}{(x-a)^n} = \frac{A_1}{x-a} + \frac{A_2}{(x-a)^2} + \cdots$:
- Multiply by $(x-a)^n$: $P(x) = A_1(x-a)^{n-1} + A_2(x-a)^{n-2} + \cdots + A_n$
- $A_n = P(a)$
- $A_{n-1} = P'(a)$, etc. (derivatives)

## Step-by-Step Procedure

1. **Check degree:** If $\deg(P) \ge \deg(Q)$, divide: $\frac{P}{Q} = S(x) + \frac{R(x)}{Q(x)}$
2. **Factorise $Q(x)$ completely** over $\mathbb{R}$
3. **Write form** according to factor types
4. **Find constants** using any method above
5. **Verify** by combining or substituting a value

## Worked Examples

### Example 1: $\frac{5x+1}{(x-1)(x+2)}$
Form: $\frac{A}{x-1} + \frac{B}{x+2}$
Cover-up:
$A = \frac{5(1)+1}{1+2} = 2$
$B = \frac{5(-2)+1}{-2-1} = \frac{-9}{-3} = 3$
Result: $\frac{2}{x-1} + \frac{3}{x+2}$

### Example 2: $\frac{3x^2+5x+2}{(x+1)^2(x-2)}$
Form: $\frac{A}{x+1} + \frac{B}{(x+1)^2} + \frac{C}{x-2}$
Multiply: $3x^2+5x+2 = A(x+1)(x-2) + B(x-2) + C(x+1)^2$
- $x=-1$: $3-5+2 = B(-3) \Rightarrow 0 = -3B \Rightarrow B=0$
- $x=2$: $12+10+2 = C(9) \Rightarrow 24 = 9C \Rightarrow C = 8/3$
- $x=0$: $2 = A(1)(-2) + B(-2) + C(1) = -2A + 8/3 \Rightarrow 2A = 8/3 - 6/3 = 2/3 \Rightarrow A = 1/3$
Result: $\frac{1/3}{x+1} + \frac{0}{(x+1)^2} + \frac{8/3}{x-2} = \frac{1}{3(x+1)} + \frac{8}{3(x-2)}$

### Example 3: $\frac{x^2+1}{(x^2+1)(x-1)}$ (improper? No, degree 2 < 3)
Form: $\frac{Ax+B}{x^2+1} + \frac{C}{x-1}$
$x^2+1 = (Ax+B)(x-1) + C(x^2+1)$
$x=1$: $2 = C(2) \Rightarrow C=1$
Coefficients of $x^2$: $1 = A + C \Rightarrow A = 0$
Constant: $1 = -B + C \Rightarrow B = 0$
Result: $\frac{1}{x-1}$

### Example 4: $\frac{2x^3+3x^2+4x+5}{(x^2+1)^2}$ (repeated irreducible quadratic)
Form: $\frac{Ax+B}{x^2+1} + \frac{Cx+D}{(x^2+1)^2}$
$2x^3+3x^2+4x+5 = (Ax+B)(x^2+1) + Cx + D$
$= Ax^3 + Bx^2 + (A+C)x + (B+D)$
Compare:
$x^3$: $2 = A$
$x^2$: $3 = B$
$x^1$: $4 = A+C = 2+C \Rightarrow C=2$
$const$: $5 = B+D = 3+D \Rightarrow D=2$
Result: $\frac{2x+3}{x^2+1} + \frac{2x+2}{(x^2+1)^2}$

## Applications

| Application | Use |
|-------------|-----|
| **Integration** | $\int \frac{P(x)}{Q(x)} dx$ term by term |
| **Binomial expansion** | Expand $\frac{1}{(1+ax)^n}$ etc. |
| **Inverse Laplace** | Standard forms |
| **Differential equations** | Separation of variables |

## Problem Patterns (A/L)

| Pattern | Key Steps |
|---------|-----------|
| Distinct linear factors | Cover-up method fastest |
| Repeated linear | Find highest power first ($x=-a$), then equate coefficients |
| Irreducible quadratic | $Ax+B$ numerator, equate coefficients |
| Improper fraction | Long division first |
| Find constants $A,B,C$ | Set up system, solve |

## Common Traps
- ❌ Not checking proper fraction first
- ❌ Missing factor in denominator (not fully factorised)
- ❌ Wrong numerator form for quadratics (must be $Ax+B$, not $A$)
- ❌ Sign errors in cover-up ($x-a$ vs $x+a$)
- ❌ Forgetting to multiply through by denominator before equating

## Cross-References
- [[Pure/01-Algebra/04-Polynomials.md]] — factorising denominator
- [[Pure/04-Calculus/07-Integration-Techniques.md]] — integration by partial fractions
- [[Pure/01-Algebra/07-Binomial-Theorem.md]] — expanding each term
- [[Pure/04-Calculus/10-Differential-Equations.md]] — separation of variables

## Quick Reference
**Proper fraction:** $\deg(P) < \deg(Q)$
**Factor types:**
- $(ax+b)$ $\to$ $\frac{A}{ax+b}$
- $(ax+b)^n$ $\to$ $\frac{A_1}{ax+b} + \frac{A_2}{(ax+b)^2} + \cdots + \frac{A_n}{(ax+b)^n}$
- $ax^2+bx+c$ (irreducible) $\to$ $\frac{Ax+B}{ax^2+bx+c}$
- $(ax^2+bx+c)^n$ $\to$ $\frac{A_1x+B_1}{ax^2+bx+c} + \cdots + \frac{A_nx+B_n}{(ax^2+bx+c)^n}$

**Cover-up:** For $\frac{A}{x-a}$, $A = \frac{P(a)}{Q'(a)}$ where $Q(x) = (x-a)R(x)$