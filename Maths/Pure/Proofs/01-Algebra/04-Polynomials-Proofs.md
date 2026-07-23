---
date: 2026-07-19
type: proof
tags: [maths, pure, proof, algebra, polynomials]
topic: [[Pure/01-Algebra/04-Polynomials.md]]
---

# Polynomials — Full Derivations

## Remainder Theorem

**Statement:** When $P(x)$ is divided by $(x-a)$, remainder $R = P(a)$.

**Proof:**
By polynomial division, $P(x) = (x-a)Q(x) + R$ where $R$ is constant (degree less than divisor degree 1).
Substitute $x = a$: $P(a) = (a-a)Q(a) + R = R$. $\square$

---

## Factor Theorem

**Statement:** $(x-a)$ is a factor of $P(x)$ $\iff$ $P(a) = 0$.

**Proof:**
($\Rightarrow$) If $(x-a) \mid P(x)$, then $P(x) = (x-a)Q(x)$. Then $P(a) = 0 \cdot Q(a) = 0$.
($\Leftarrow$) If $P(a) = 0$, by Remainder Theorem, remainder on division by $(x-a)$ is $P(a) = 0$. So division is exact $\Rightarrow (x-a)$ is a factor. $\square$

---

## Vieta's Formulas (Roots & Coefficients)

**Quadratic:** $ax^2 + bx + c = 0$ with roots $\alpha, \beta$

**Proof:**
$ax^2 + bx + c = a(x-\alpha)(x-\beta) = a(x^2 - (\alpha+\beta)x + \alpha\beta)$
$= ax^2 - a(\alpha+\beta)x + a\alpha\beta$
Equate coefficients:
$-a(\alpha+\beta) = b \Rightarrow \alpha+\beta = -\frac{b}{a}$
$a\alpha\beta = c \Rightarrow \alpha\beta = \frac{c}{a}$. $\square$

**Cubic:** $ax^3 + bx^2 + cx + d = 0$ with roots $\alpha, \beta, \gamma$

**Proof:**
$a(x-\alpha)(x-\beta)(x-\gamma) = a[x^3 - (\sum\alpha)x^2 + (\sum\alpha\beta)x - \alpha\beta\gamma]$
$= ax^3 - a(\sum\alpha)x^2 + a(\sum\alpha\beta)x - a\alpha\beta\gamma$
Equate:
$-a(\sum\alpha) = b \Rightarrow \sum\alpha = -\frac{b}{a}$
$a(\sum\alpha\beta) = c \Rightarrow \sum\alpha\beta = \frac{c}{a}$
$-a\alpha\beta\gamma = d \Rightarrow \alpha\beta\gamma = -\frac{d}{a}$. $\square$

---

## General Vieta (Degree $n$)

$P(x) = a_n x^n + a_{n-1} x^{n-1} + \cdots + a_0 = a_n \prod_{i=1}^n (x - \alpha_i)$

**Elementary symmetric polynomials:**
$e_1 = \sum \alpha_i$, $e_2 = \sum_{i<j} \alpha_i \alpha_j$, ..., $e_n = \prod \alpha_i$

**Result:**
$e_k = (-1)^k \frac{a_{n-k}}{a_n}$ for $k = 1, 2, \ldots, n$

**Proof by induction on $n$** using expansion of $\prod (x - \alpha_i)$. $\square$

---

## Newton's Identities (Power Sums)

Let $S_k = \sum_{i=1}^n \alpha_i^k$.

Then for $k \le n$:
$S_k + a_{n-1}S_{k-1} + a_{n-2}S_{k-2} + \cdots + a_{n-k+1}S_1 + k a_{n-k} = 0$

**Proof:**
Each $\alpha_i$ satisfies $a_n \alpha_i^n + a_{n-1} \alpha_i^{n-1} + \cdots + a_0 = 0$.
Multiply by $\alpha_i^{k-n}$ and sum over $i$. $\square$

---

## Symmetric Polynomial Expressions

### $\alpha^2 + \beta^2 + \gamma^2$
$= (\sum \alpha)^2 - 2\sum\alpha\beta$

**Proof:**
$(\alpha+\beta+\gamma)^2 = \alpha^2+\beta^2+\gamma^2 + 2(\alpha\beta+\beta\gamma+\gamma\alpha)$.
Rearrange. $\square$

### $\alpha^3 + \beta^3 + \gamma^3$
$= (\sum\alpha)^3 - 3\sum\alpha\sum\alpha\beta + 3\alpha\beta\gamma$

**Proof:**
$(\alpha+\beta+\gamma)^3 = \alpha^3+\beta^3+\gamma^3 + 3\sum_{\text{sym}}\alpha^2\beta + 6\alpha\beta\gamma$
Also $(\alpha+\beta+\gamma)(\alpha\beta+\beta\gamma+\gamma\alpha) = \sum_{\text{sym}}\alpha^2\beta + 3\alpha\beta\gamma$
Subtract $3\times$ second from first:
$(\sum\alpha)^3 - 3\sum\alpha\sum\alpha\beta = \alpha^3+\beta^3+\gamma^3 - 3\alpha\beta\gamma$
Rearrange. $\square$

---

## Polynomial Division Algorithm

**Theorem:** For polynomials $P(x), D(x) \neq 0$, $\exists! Q(x), R(x)$ such that
$P(x) = D(x)Q(x) + R(x)$ with $\deg R < \deg D$ or $R(x) = 0$.

**Proof by induction on $\deg P$:**
If $\deg P < \deg D$, take $Q=0, R=P$.
If $\deg P \ge \deg D$, let leading terms be $a_n x^n$ and $b_m x^m$.
Subtract $\frac{a_n}{b_m} x^{n-m} D(x)$ from $P(x)$ to cancel leading term.
New polynomial has degree $< \deg P$. Apply induction. $\square$

---

## Rational Root Theorem

If $\frac{p}{q}$ (in lowest terms) is a root of $a_n x^n + \cdots + a_0 = 0$ with integer coefficients, then $p \mid a_0$ and $q \mid a_n$.

**Proof:**
$a_n(p/q)^n + \cdots + a_1(p/q) + a_0 = 0$
Multiply by $q^n$: $a_n p^n + a_{n-1} p^{n-1} q + \cdots + a_1 p q^{n-1} + a_0 q^n = 0$
$p(a_n p^{n-1} + \cdots + a_1 q^{n-1}) = -a_0 q^n$
Since $\gcd(p,q)=1$, $p \mid a_0$.
Similarly $q \mid a_n$. $\square$

---

## Descartes' Rule of Signs

**Positive roots:** Number of positive real roots $\le$ number of sign changes in coefficients of $P(x)$, and differs by an even number.

**Negative roots:** Apply to $P(-x)$.

**Proof sketch:** Induction on degree, using $P(x) = (x-\alpha)Q(x)$ and tracking sign changes. $\square$

---

## Complex Conjugate Root Theorem

If $P(x)$ has real coefficients and $\alpha = u+iv$ is a root, then $\bar{\alpha} = u-iv$ is also a root.

**Proof:**
$P(\alpha) = 0 \Rightarrow \overline{P(\alpha)} = \overline{0} = 0$.
But $\overline{P(\alpha)} = P(\overline{\alpha})$ since coefficients are real.
So $P(\bar{\alpha}) = 0$. $\square$

---

## Discriminant

**Quadratic:** $ax^2+bx+c$: $\Delta = b^2 - 4ac$
- $\Delta > 0$: two distinct real roots
- $\Delta = 0$: repeated real root
- $\Delta < 0$: two complex conjugate roots

**Proof:** Roots = $\frac{-b \pm \sqrt{b^2-4ac}}{2a}$. $\square$

**Cubic:** $ax^3+bx^2+cx+d$: $\Delta = 18abcd - 4b^3d + b^2c^2 - 4ac^3 - 27a^2d^2$
- $\Delta > 0$: three distinct real roots
- $\Delta = 0$: multiple root
- $\Delta < 0$: one real, two complex

---

## Bounds on Roots

**Cauchy's bound:** All roots satisfy $|x| \le 1 + \max\left|\frac{a_{n-1}}{a_n}\right|, \ldots, \left|\frac{a_0}{a_n}\right|$

**Proof:** If $|x| > 1$, then $|x|^n \le \sum_{k=0}^{n-1} |a_k/a_n| |x|^k < \max|a_k/a_n| \frac{|x|^n-1}{|x|-1}$. Rearrange. $\square$

**Lagrange's bound:** All roots satisfy $|x| \le \max\left(1, \sum_{k=0}^{n-1} \left|\frac{a_k}{a_n}\right|\right)$