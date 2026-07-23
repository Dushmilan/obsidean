---
date: 2026-07-19
type: proof
tags: [maths, pure, proof, algebra, partial-fractions]
topic: [[Pure/01-Algebra/05-Partial-Fractions.md]]
---

# Partial Fractions — Full Derivations

## Fundamental Theorem

**Theorem:** If $P(x), Q(x)$ are polynomials with $\deg P < \deg Q$, and $Q(x)$ factors over $\mathbb{R}$ as:
$Q(x) = \prod_{i=1}^k (a_i x + b_i)^{n_i} \cdot \prod_{j=1}^m (c_j x^2 + d_j x + e_j)^{m_j}$
where $d_j^2 - 4c_j e_j < 0$ (irreducible quadratics), then
$\frac{P(x)}{Q(x)}$ can be uniquely written as:
$\sum_{i=1}^k \sum_{r=1}^{n_i} \frac{A_{i,r}}{(a_i x + b_i)^r} + \sum_{j=1}^m \sum_{s=1}^{m_j} \frac{B_{j,s}x + C_{j,s}}{(c_j x^2 + d_j x + e_j)^s}$

**Proof of Existence:**
Multiply both sides by $Q(x)$ to get polynomial identity.
LHS is $P(x)$. RHS is sum of polynomials.
Equate coefficients $\to$ linear system in unknowns $A, B, C$.
System is non-singular (can be shown by evaluating at roots of $Q$ and derivatives).
Unique solution exists. $\square$

**Proof of Uniqueness:**
If two decompositions equal, subtract them $\to$ zero polynomial.
Multiply by $Q(x)$: polynomial identity with zero coefficients.
All coefficients must be zero. $\square$

---

## Cover-Up Method (Heaviside)

For distinct linear factors: $\frac{P(x)}{(x-\alpha_1)\cdots(x-\alpha_n)} = \sum_{i=1}^n \frac{A_i}{x-\alpha_i}$

**Proof of $A_i = \frac{P(\alpha_i)}{\prod_{j \neq i} (\alpha_i - \alpha_j)}$:**

Multiply by $(x-\alpha_i)$:
$\frac{P(x)}{\prod_{j \neq i} (x-\alpha_j)} = A_i + (x-\alpha_i) \sum_{j \neq i} \frac{A_j}{x-\alpha_j}$

Take limit $x \to \alpha_i$: LHS $\to \frac{P(\alpha_i)}{\prod_{j \neq i} (\alpha_i - \alpha_j)}$, RHS $\to A_i$. $\square$

---

## Repeated Linear Factors

$\frac{P(x)}{(x-\alpha)^n} = \frac{A_1}{x-\alpha} + \frac{A_2}{(x-\alpha)^2} + \cdots + \frac{A_n}{(x-\alpha)^n}$

**Proof of $A_n = P(\alpha)$:**
Multiply by $(x-\alpha)^n$: $P(x) = A_1(x-\alpha)^{n-1} + \cdots + A_{n-1}(x-\alpha) + A_n$.
Set $x = \alpha$: $P(\alpha) = A_n$. $\square$

**Proof of $A_{n-1} = P'(\alpha)$:**
Differentiate $P(x) = A_1(x-\alpha)^{n-1} + \cdots + A_n$:
$P'(x) = (n-1)A_1(x-\alpha)^{n-2} + \cdots + A_{n-1}$
Set $x = \alpha$: $P'(\alpha) = A_{n-1}$. $\square$

**General:** $A_{n-k} = \frac{P^{(k)}(\alpha)}{k!}$ for $k = 0, 1, \ldots, n-1$.

---

## Irreducible Quadratic Factors

$\frac{P(x)}{(x^2+bx+c)^m} = \sum_{r=1}^m \frac{A_r x + B_r}{(x^2+bx+c)^r}$ (where $b^2-4c < 0$)

**Why $Ax+B$ numerator?**
The numerator must have degree $< 2$ (degree of denominator). Linear is max.

**Finding constants:** Multiply by $(x^2+bx+c)^m$, equate coefficients of $x^k$.
System is non-singular because $x^2+bx+c$ has no real roots $\Rightarrow$ no common factors with $P$.

---

## Integration Formulas from Partial Fractions

### $\int \frac{A}{ax+b} dx = \frac{A}{a} \ln|ax+b| + C$

**Proof:** Let $u = ax+b$, $du = a dx$. $\int \frac{A}{u} \frac{du}{a} = \frac{A}{a} \ln|u|$. $\square$

### $\int \frac{A}{(ax+b)^n} dx = \frac{A}{a(1-n)(ax+b)^{n-1}} + C$ ($n \neq 1$)

**Proof:** $u = ax+b$, $du = a dx$. $\int A u^{-n} \frac{du}{a} = \frac{A}{a} \frac{u^{1-n}}{1-n}$. $\square$

### $\int \frac{Ax+B}{ax^2+bx+c} dx$

Complete square: $ax^2+bx+c = a\left[(x+\frac{b}{2a})^2 + \frac{4ac-b^2}{4a^2}\right]$
Let $u = x + \frac{b}{2a}$, $k^2 = \frac{4ac-b^2}{4a^2} > 0$:
$\int \frac{A(u-\frac{b}{2a})+B}{a(u^2+k^2)} du = \frac{1}{a} \int \frac{(A-\frac{Ab}{2a}+B)}{u^2+k^2} du + \frac{1}{a} \int \frac{-\frac{Ab}{2a}}{u^2+k^2} du$
$= \frac{2Aa - Ab + 2Ba}{2a^2} \cdot \frac{1}{k} \arctan\frac{u}{k} + C$

---

## Telescoping Series

$\frac{1}{x(x+1)} = \frac{1}{x} - \frac{1}{x+1}$

**Sum:** $\sum_{k=1}^n \frac{1}{k(k+1)} = \sum_{k=1}^n \left(\frac{1}{k} - \frac{1}{k+1}\right) = 1 - \frac{1}{n+1} = \frac{n}{n+1}$

**General:** $\frac{1}{(x+a)(x+b)} = \frac{1}{b-a}\left(\frac{1}{x+a} - \frac{1}{x+b}\right)$ for $a \neq b$.

---

## Improper Fraction Division

If $\deg P \ge \deg Q$, divide: $P(x) = Q(x) S(x) + R(x)$, $\deg R < \deg Q$.
Then $\frac{P(x)}{Q(x)} = S(x) + \frac{R(x)}{Q(x)}$.

**Proof:** Division algorithm for polynomials. $S$ is quotient, $R$ remainder. $\square$

**Algorithm (synthetic division for $x-c$):**
```
c | a_n   a_{n-1}  ...  a_0
  |       c a_n  ...
  | a_n  a_{n-1}+c a_n ...
```
Quotient coefficients: $a_n, a_{n-1}+c a_n, \ldots$
Remainder: last value = $P(c)$ (Remainder Theorem).

---

## Residue Method (Complex Analysis Connection)

For $\frac{P(z)}{(z-\alpha)^n Q(z)}$ with $Q(\alpha) \neq 0$, the coefficient of $\frac{1}{(z-\alpha)^n}$ is $\frac{P(\alpha)}{Q(\alpha)}$.
More generally, coefficient of $\frac{1}{(z-\alpha)^k}$ is $\frac{1}{(n-k)!} \lim_{z \to \alpha} \frac{d^{n-k}}{dz^{n-k}} \left[(z-\alpha)^n \frac{P(z)}{Q(z)}\right]$.

**Proof:** This is the Laurent series expansion around $z=\alpha$. $\square$