---
date: 2026-07-19
type: proof
tags: [maths, pure, proof, algebra, indices]
parent: [[Pure/01-Algebra/02-Indices]]
---

# Indices (Exponents) — Full Derivations

## Laws of Indices

### Law 1: $a^m \cdot a^n = a^{m+n}$

**For $m,n \in \mathbb{N}$:**
$a^m a^n = \underbrace{a \cdots a}_{m} \cdot \underbrace{a \cdots a}_{n} = \underbrace{a \cdots a}_{m+n} = a^{m+n}$. $\square$

**For $m,n \in \mathbb{Z}$:**
$a^m a^n = a^m a^n$ (already proven for natural numbers).
Extend using $a^{-k} = 1/a^k$:
$a^m a^{-n} = a^m \cdot \frac{1}{a^n} = \frac{a^m}{a^n} = a^{m-n}$ (if $m \ge n$).
$a^{-m} a^{-n} = \frac{1}{a^m} \frac{1}{a^n} = \frac{1}{a^{m+n}} = a^{-(m+n)}$.

**For $m,n \in \mathbb{Q}$:** Let $m = p/q$, $n = r/s$.
$a^{p/q} a^{r/s} = \sqrt[q]{a^p} \sqrt[s]{a^r} = \sqrt[qs]{a^{ps}} \sqrt[qs]{a^{rq}} = \sqrt[qs]{a^{ps+rq}} = a^{(ps+rq)/qs} = a^{p/q + r/s}$. $\square$

**For real $x,y$:** By continuity/limit of rationals. $a^x a^y = a^{x+y}$.

---

### Law 2: $\frac{a^m}{a^n} = a^{m-n}$

**Proof:** $a^m = a^{m-n} a^n \Rightarrow \frac{a^m}{a^n} = a^{m-n}$. $\square$

---

### Law 3: $(a^m)^n = a^{mn}$

**For $m,n \in \mathbb{N}$:**
$(a^m)^n = \underbrace{a^m \cdots a^m}_{n} = a^{\underbrace{m + \cdots + m}_{n}} = a^{mn}$. $\square$

**For integers:** Use definition of negative exponents and $a^0 = 1$.

**Caution:** $(a^m)^n = a^{mn}$ holds for real $a > 0$ and all real $m,n$.
But fails for $a < 0$ with fractional exponents:
$((-1)^2)^{1/2} = 1^{1/2} = 1 \neq (-1)^1 = -1$.

---

### Law 4: $(ab)^n = a^n b^n$

**For $n \in \mathbb{N}$:**
$(ab)^n = \underbrace{ab \cdots ab}_{n} = \underbrace{a \cdots a}_{n} \underbrace{b \cdots b}_{n} = a^n b^n$. $\square$

**For $n \in \mathbb{Z}$:** Use definition of negative exponent.
**For $n \in \mathbb{Q}$:** Requires $a,b > 0$ (principal root).

---

### Law 5: $\left(\frac{a}{b}\right)^n = \frac{a^n}{b^n}$

**Proof:** Similar to Law 4. $\square$

---

## Zero and Negative Exponents

### $a^0 = 1$ ($a \neq 0$)

**Proof:** $a^0 = a^{1-1} = \frac{a^1}{a^1} = 1$. $\square$

### $a^{-n} = \frac{1}{a^n}$ ($a \neq 0$)

**Proof:** By Law 1: $a^n a^{-n} = a^0 = 1 \Rightarrow a^{-n} = \frac{1}{a^n}$. $\square$

---

## Rational Exponents

### Definition: $a^{p/q} = \sqrt[q]{a^p} = (\sqrt[q]{a})^p$ ($a > 0$, $q \in \mathbb{N}$, $\gcd(p,q)=1$)

**Consistency:** If $p/q = r/s$ (in lowest terms), then $a^{p/q} = a^{r/s}$.
Proof: $ps = qr$. $a^{p/q} = \sqrt[q]{a^p} = \sqrt[qs]{a^{ps}} = \sqrt[qs]{a^{qr}} = \sqrt[s]{a^r} = a^{r/s}$. $\square$

---

## Exponential Function

### Definition: $a^x = \lim_{r \to x, r \in \mathbb{Q}} a^r$ for $a > 0$

**Continuity:** $f(x) = a^x$ is continuous on $\mathbb{R}$ for $a > 0$.

**Monotonicity:**
- $a > 1$: strictly increasing
- $0 < a < 1$: strictly decreasing
- $a = 1$: constant

**Proof for $a > 1$:** If $x < y$, choose rationals $r,s$ with $x < r < s < y$. Then $a^x \le a^r < a^s \le a^y$. $\square$

---

## Number $e$

### Definition: $e = \lim_{n \to \infty} \left(1 + \frac{1}{n}\right)^n$

**Alternative:** $e = \sum_{k=0}^\infty \frac{1}{k!}$

**Proof of equivalence:**
$\left(1 + \frac{1}{n}\right)^n = \sum_{k=0}^n \binom{n}{k} \frac{1}{n^k} = \sum_{k=0}^n \frac{n(n-1)\cdots(n-k+1)}{n^k} \frac{1}{k!} = \sum_{k=0}^n \left(1 - \frac{1}{n}\right)\cdots\left(1 - \frac{k-1}{n}\right) \frac{1}{k!}$
As $n \to \infty$, each term $\to 1/k!$. By monotone convergence, limit is $\sum 1/k!$. $\square$

**Derivative property:** $\frac{d}{dx} e^x = e^x$

**Proof:** $\frac{d}{dx} e^x = \lim_{h \to 0} \frac{e^{x+h} - e^x}{h} = e^x \lim_{h \to 0} \frac{e^h - 1}{h}$.
But $\lim_{h \to 0} \frac{e^h - 1}{h} = \lim_{n \to \infty} n(e^{1/n} - 1) = \lim_{n \to \infty} n(\sqrt[n]{e} - 1) = 1$ (since $\sqrt[n]{e} = 1 + \frac{1}{n} + O(1/n^2)$). $\square$

---

## Exponential Growth/Decay

### Differential equation: $\frac{dN}{dt} = kN$

**Solution:** $N(t) = N_0 e^{kt}$

**Proof:** Separate variables: $\frac{dN}{N} = k dt \Rightarrow \ln N = kt + C \Rightarrow N = Ce^{kt} = N_0 e^{kt}$. $\square$

### Half-life ($k < 0$):
$t_{1/2} = \frac{\ln 2}{|k|}$

**Proof:** $N_0/2 = N_0 e^{-|k| t_{1/2}} \Rightarrow e^{-|k| t_{1/2}} = 1/2 \Rightarrow -|k| t_{1/2} = -\ln 2$. $\square$

### Doubling time ($k > 0$):
$t_2 = \frac{\ln 2}{k}$

---

## Change of Base Formula

$\log_a b = \frac{\log_c b}{\log_c a}$

**Proof:** Let $x = \log_a b \Rightarrow a^x = b$.
Take $\log_c$: $x \log_c a = \log_c b \Rightarrow x = \frac{\log_c b}{\log_c a}$. $\square$

---

## Irrationality of $e$

**Theorem:** $e$ is irrational.

**Proof:** Suppose $e = p/q$ in lowest terms.
$q! \cdot e = q! \sum_{k=0}^\infty \frac{1}{k!} = \sum_{k=0}^q \frac{q!}{k!} + \sum_{k=q+1}^\infty \frac{q!}{k!}$
First sum is integer. Second sum:
$\sum_{k=q+1}^\infty \frac{q!}{k!} = \frac{1}{q+1} + \frac{1}{(q+1)(q+2)} + \cdots < \frac{1}{q+1} + \frac{1}{(q+1)^2} + \cdots = \frac{1/(q+1)}{1 - 1/(q+1)} = \frac{1}{q} < 1$
So second sum is strictly between 0 and 1. Contradiction. $\square$

---

## Comparison of Growth Rates

For any $a > 1$, $b > 0$: $\lim_{x \to \infty} \frac{x^b}{a^x} = 0$ (exponential beats polynomial).

**Proof:** $a^x = e^{x \ln a}$. $\frac{x^b}{e^{x \ln a}} \to 0$ by L'Hôpital $b$ times or ratio test. $\square$

For any $a > 1$: $\lim_{x \to \infty} \frac{\ln x}{x^a} = 0$ (polynomial beats logarithm).