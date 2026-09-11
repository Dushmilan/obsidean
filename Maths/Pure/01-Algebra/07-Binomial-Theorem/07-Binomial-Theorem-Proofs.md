---
date: 2026-07-19
type: proof
tags: [maths, pure, proof, algebra, binomial-theorem]
parent: [[07-Binomial-Theorem]]
---

# Binomial Theorem — Full Derivations

## Integer Index

### Theorem: $(x+y)^n = \sum_{r=0}^n \binom{n}{r} x^{n-r} y^r$ for $n \in \mathbb{N}$

**Proof by Induction:**

**Base ($n=1$):** $(x+y)^1 = x+y = \binom{1}{0}x + \binom{1}{1}y$. ✓

**Inductive Step:** Assume true for $n=k$:
$(x+y)^k = \sum_{r=0}^k \binom{k}{r} x^{k-r} y^r$

Prove for $n=k+1$:
$(x+y)^{k+1} = (x+y) \sum_{r=0}^k \binom{k}{r} x^{k-r} y^r$
$= \sum_{r=0}^k \binom{k}{r} x^{k+1-r} y^r + \sum_{r=0}^k \binom{k}{r} x^{k-r} y^{r+1}$

Reindex second sum: $s = r+1$:
$= \binom{k}{0}x^{k+1} + \sum_{r=1}^k \left[ \binom{k}{r} + \binom{k}{r-1} \right] x^{k+1-r} y^r + \binom{k}{k} y^{k+1}$

By Pascal's identity: $\binom{k}{r} + \binom{k}{r-1} = \binom{k+1}{r}$
$= \sum_{r=0}^{k+1} \binom{k+1}{r} x^{k+1-r} y^r$. ✓

---

## Generalised Binomial Coefficient

For $n \in \mathbb{R}$, $r \in \mathbb{N} \cup \{0\}$:
$\binom{n}{r} = \frac{n(n-1)\cdots(n-r+1)}{r!}$, $\binom{n}{0} = 1$

---

## Rational Index (Generalised Binomial Theorem)

### Theorem: $(1+x)^n = \sum_{r=0}^\infty \binom{n}{r} x^r$ for $|x| < 1$, $n \in \mathbb{Q}$

**Proof via Taylor Series:**
$f(x) = (1+x)^n$
$f^{(r)}(x) = n(n-1)\cdots(n-r+1)(1+x)^{n-r}$
$f^{(r)}(0) = n(n-1)\cdots(n-r+1) = r! \binom{n}{r}$

Taylor series at $0$:
$(1+x)^n = \sum_{r=0}^\infty \frac{f^{(r)}(0)}{r!} x^r = \sum_{r=0}^\infty \binom{n}{r} x^r$

**Convergence:** Ratio test on $a_r = \binom{n}{r} x^r$:
$\left| \frac{a_{r+1}}{a_r} \right| = \left| \frac{n-r}{r+1} x \right| \to |x|$ as $r \to \infty$.
Converges for $|x| < 1$, diverges for $|x| > 1$. $\square$

---

## Proof of Convergence Radius

**Ratio Test:**
$\binom{n}{r} = \frac{n(n-1)\cdots(n-r+1)}{r!}$
$\frac{\binom{n}{r+1}}{\binom{n}{r}} = \frac{n-r}{r+1}$
$\lim_{r \to \infty} \left| \frac{a_{r+1}}{a_r} \right| = |x| \lim_{r \to \infty} \frac{n-r}{r+1} = |x|$
Converges if $|x| < 1$. $\square$

---

## Standard Expansions (Derivations)

### 1. $(1+x)^{-1} = 1 - x + x^2 - x^3 + \cdots$

$\binom{-1}{r} = \frac{(-1)(-2)\cdots(-r)}{r!} = \frac{(-1)^r r!}{r!} = (-1)^r$

### 2. $(1-x)^{-1} = 1 + x + x^2 + x^3 + \cdots$

Substitute $-x$ for $x$ in (1).

### 3. $(1+x)^{-2} = 1 - 2x + 3x^2 - 4x^3 + \cdots$

$\binom{-2}{r} = \frac{(-2)(-3)\cdots(-r-1)}{r!} = (-1)^r \frac{(r+1)!}{r!} = (-1)^r (r+1)$

### 4. $(1-x)^{-2} = 1 + 2x + 3x^2 + 4x^3 + \cdots$

Substitute $-x$ in (3).

### 5. $(1+x)^{1/2} = 1 + \frac{1}{2}x - \frac{1}{8}x^2 + \frac{1}{16}x^3 - \cdots$

$\binom{1/2}{r} = \frac{(1/2)(-1/2)(-3/2)\cdots((3-2r)/2)}{r!}$
$= \frac{(-1)^{r-1}(2r-3)!!}{2^r r!}$ for $r \ge 2$

### 6. $(1+x)^{-1/2} = 1 - \frac{1}{2}x + \frac{3}{8}x^2 - \frac{5}{16}x^3 + \cdots$

---

## Binomial Series with General Term

For $n \in \mathbb{R}$, $(1+x)^n = \sum_{r=0}^\infty \binom{n}{r} x^r$

**First few terms:**
- $T_1 = 1$
- $T_2 = nx$
- $T_3 = \frac{n(n-1)}{2}x^2$
- $T_4 = \frac{n(n-1)(n-2)}{6}x^3$
- $T_5 = \frac{n(n-1)(n-2)(n-3)}{24}x^4$

---

## Approximation: $(1+x)^n \approx 1+nx$ for small $x$

**Proof (Binomial):**
$(1+x)^n = 1 + nx + \frac{n(n-1)}{2}x^2 + \cdots$
For $|x| \ll 1$, $x^2$ and higher terms negligible. $\square$

**Error bound:** $|(1+x)^n - (1+nx)| \le \frac{|n(n-1)|}{2}x^2$ for small $x$ (by alternating series or Taylor remainder).

---

## Change of Form: $(a+bx)^n$

$(a+bx)^n = a^n \left(1 + \frac{b}{a}x\right)^n = a^n \sum_{r=0}^\infty \binom{n}{r} \left(\frac{b}{a}\right)^r x^r$
Valid for $\left|\frac{b}{a}x\right| < 1 \iff |x| < \left|\frac{a}{b}\right|$

---

## Finding Specific Coefficients

### Example: Coefficient of $x^4$ in $(2-3x)^{-2}$

$(2-3x)^{-2} = 2^{-2}(1 - \frac{3}{2}x)^{-2} = \frac{1}{4} \sum_{r=0}^\infty \binom{-2}{r} \left(-\frac{3}{2}x\right)^r$
$\binom{-2}{r} = (-1)^r (r+1)$
Coefficient of $x^4$: $\frac{1}{4} \cdot (-1)^4 \cdot 5 \cdot \left(-\frac{3}{2}\right)^4 = \frac{5}{4} \cdot \frac{81}{16} = \frac{405}{64}$

---

## Binomial Identity Proofs

### 1. $\sum_{r=0}^n \binom{n}{r} = 2^n$
Set $x=1$ in $(1+x)^n$. $\square$

### 2. $\sum_{r=0}^n (-1)^r \binom{n}{r} = 0$ ($n \ge 1$)
Set $x=-1$ in $(1+x)^n$. $\square$

### 3. $\sum_{r=0}^n r \binom{n}{r} = n 2^{n-1}$
Differentiate $(1+x)^n$: $n(1+x)^{n-1} = \sum r \binom{n}{r} x^{r-1}$. Set $x=1$. $\square$

### 4. $\sum_{r=0}^n r(r-1) \binom{n}{r} = n(n-1)2^{n-2}$
Differentiate twice: $n(n-1)(1+x)^{n-2} = \sum r(r-1) \binom{n}{r} x^{r-2}$. Set $x=1$. $\square$

### 5. Vandermonde: $\sum_{k=0}^r \binom{m}{k} \binom{n}{r-k} = \binom{m+n}{r}$
$(1+x)^{m+n} = (1+x)^m (1+x)^n$. Compare $x^r$ coefficients. $\square$

---

## Validity Range for Rational Index

$(1+x)^n = \sum_{r=0}^\infty \binom{n}{r} x^r$ converges for:
- $|x| < 1$ (always)
- $x = 1$ if $n > -1$
- $x = -1$ if $n > 0$

**Proof:** At $x=1$, series is $\sum \binom{n}{r}$. Terms $\sim \frac{n^r}{r!}$ for large $r$ if $n$ not integer. Converges by ratio test if $n > -1$ (since $\binom{n}{r} \sim r^{-n-1}$). At $x=-1$, alternating series test. $\square$

---

## Extension: $(a+bx)^n$ for $a,b \in \mathbb{R}$

$(a+bx)^n = a^n (1 + \frac{b}{a}x)^n = a^n \sum_{r=0}^\infty \binom{n}{r} \left(\frac{b}{a}x\right)^r$
Valid for $\left|\frac{b}{a}x\right| < 1$.

If $a < 0$, write $(-a(1 - \frac{b}{a}x))^n$ and handle sign carefully (complex branch cuts for non-integer $n$).