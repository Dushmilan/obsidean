---
date: 2026-07-19
type: proof
tags: [maths, pure, proof, algebra, surds]
parent: [[01-Real-Numbers-Surds]]
---

# Real Numbers & Surds — Full Derivations

## Surd Laws

### Law 1: $\sqrt[n]{a} \cdot \sqrt[n]{b} = \sqrt[n]{ab}$ ($a,b \ge 0$)

**Proof:**
Let $x = \sqrt[n]{a}$, $y = \sqrt[n]{b}$.
Then $x^n = a$, $y^n = b$.
$(xy)^n = x^n y^n = ab$.
Therefore $xy = \sqrt[n]{ab}$, i.e., $\sqrt[n]{a} \cdot \sqrt[n]{b} = \sqrt[n]{ab}$. $\square$

### Law 2: $\frac{\sqrt[n]{a}}{\sqrt[n]{b}} = \sqrt[n]{\frac{a}{b}}$ ($a \ge 0, b > 0$)

**Proof:**
Let $x = \sqrt[n]{a}$, $y = \sqrt[n]{b}$.
Then $x^n = a$, $y^n = b$.
$\left(\frac{x}{y}\right)^n = \frac{x^n}{y^n} = \frac{a}{b}$.
Therefore $\frac{x}{y} = \sqrt[n]{\frac{a}{b}}$, i.e., $\frac{\sqrt[n]{a}}{\sqrt[n]{b}} = \sqrt[n]{\frac{a}{b}}$. $\square$

### Law 3: $(\sqrt[n]{a})^m = \sqrt[n]{a^m} = a^{m/n}$

**Proof:**
Let $x = \sqrt[n]{a} \Rightarrow x^n = a$.
$x^m = (x^n)^{m/n} = a^{m/n}$.
Also $x^m = (\sqrt[n]{a})^m$.
Since $x^m = \sqrt[n]{a^m}$ (by definition of $n$th root), we have $(\sqrt[n]{a})^m = \sqrt[n]{a^m} = a^{m/n}$. $\square$

---

## Rationalising Denominators

### $\frac{1}{\sqrt{a}+\sqrt{b}} = \frac{\sqrt{a}-\sqrt{b}}{a-b}$ ($a \neq b$)

**Proof:**
$\frac{1}{\sqrt{a}+\sqrt{b}} \cdot \frac{\sqrt{a}-\sqrt{b}}{\sqrt{a}-\sqrt{b}} = \frac{\sqrt{a}-\sqrt{b}}{(\sqrt{a})^2-(\sqrt{b})^2} = \frac{\sqrt{a}-\sqrt{b}}{a-b}$. $\square$

### $\frac{1}{\sqrt{a}+\sqrt{b}+\sqrt{c}}$

**Proof:** Multiply by conjugate in stages:
$\frac{1}{\sqrt{a}+\sqrt{b}+\sqrt{c}} \cdot \frac{(\sqrt{a}+\sqrt{b})-\sqrt{c}}{(\sqrt{a}+\sqrt{b})-\sqrt{c}} = \frac{(\sqrt{a}+\sqrt{b})-\sqrt{c}}{a+b-c+2\sqrt{ab}}$
Then rationalise the remaining binomial denominator. $\square$

---

## Nested Surds

### Theorem: $\sqrt{a + 2\sqrt{b}} = \sqrt{x} + \sqrt{y}$ where $x+y=a$, $xy=b$

**Proof:**
Let $x, y$ be roots of $t^2 - at + b = 0$.
Then $x+y=a$, $xy=b$.
Since $a^2 - 4b \ge 0$, $x,y \in \mathbb{R}$ and $x \ge 0, y \ge 0$ (for $a \ge 0$).
$(\sqrt{x} + \sqrt{y})^2 = x + y + 2\sqrt{xy} = a + 2\sqrt{b}$.
Taking positive square root: $\sqrt{x} + \sqrt{y} = \sqrt{a + 2\sqrt{b}}$. $\square$

### Theorem: $\sqrt{a - 2\sqrt{b}} = \sqrt{x} - \sqrt{y}$ (with $x > y$, $x+y=a$, $xy=b$)

**Proof:**
$(\sqrt{x} - \sqrt{y})^2 = x + y - 2\sqrt{xy} = a - 2\sqrt{b}$.
Since $x > y \ge 0$, $\sqrt{x} - \sqrt{y} \ge 0$.
Taking positive square root gives the result. $\square$

---

## Absolute Value Properties

### 1. $|xy| = |x||y|$

**Proof:**
- If $x \ge 0, y \ge 0$: $xy \ge 0 \Rightarrow |xy| = xy = |x||y|$
- If $x \ge 0, y < 0$: $xy \le 0 \Rightarrow |xy| = -xy = x(-y) = |x||y|$
- If $x < 0, y \ge 0$: symmetric
- If $x < 0, y < 0$: $xy > 0 \Rightarrow |xy| = xy = (-x)(-y) = |x||y|$
All cases hold. $\square$

### 2. Triangle Inequality: $|x+y| \le |x| + |y|$

**Proof:**
$|x+y|^2 = (x+y)^2 = x^2 + 2xy + y^2 = |x|^2 + 2xy + |y|^2$
Since $xy \le |xy| = |x||y|$:
$|x+y|^2 \le |x|^2 + 2|x||y| + |y|^2 = (|x|+|y|)^2$
Taking square roots (both sides non-negative): $|x+y| \le |x|+|y|$. $\square$

### 3. $|x| < a \iff -a < x < a$ ($a > 0$)

**Proof:**
$|x| < a \iff \sqrt{x^2} < a \iff x^2 < a^2 \iff x^2 - a^2 < 0 \iff (x-a)(x+a) < 0 \iff -a < x < a$. $\square$

---

## Irrationality of Surds

### Theorem: $\sqrt{n}$ is irrational unless $n$ is a perfect square.

**Proof:**
Suppose $\sqrt{n} = \frac{p}{q}$ in lowest terms ($\gcd(p,q)=1$).
Then $n = \frac{p^2}{q^2} \Rightarrow p^2 = nq^2$.
If prime $r$ divides $n$ but $r^2 \nmid n$, then $r \mid p^2 \Rightarrow r \mid p$.
Then $r^2 \mid p^2 \Rightarrow r^2 \mid nq^2 \Rightarrow r \mid q^2 \Rightarrow r \mid q$.
Then $r \mid p$ and $r \mid q$, contradicting $\gcd(p,q)=1$.
Therefore no such $r$ exists, so $n$ is a perfect square. $\square$

---

## Completeness Axiom (Real Numbers)

**Axiom:** Every non-empty subset of $\mathbb{R}$ that is bounded above has a least upper bound (supremum) in $\mathbb{R}$.

**Consequence:** $\sqrt{2}$ exists as a real number.
Let $S = \{x \in \mathbb{Q} : x > 0, x^2 < 2\}$. $S$ is non-empty ($1 \in S$) and bounded above (by 2). By completeness, $\sup S$ exists in $\mathbb{R}$. Call it $\sqrt{2}$. Then $(\sqrt{2})^2 = 2$.

---

## Density of Rationals and Irrationals

**Theorem:** Between any two real numbers $a < b$, there exists a rational and an irrational.

**Proof:**
*Rational:* By Archimedean property, $\exists n \in \mathbb{N}$ s.t. $\frac{1}{n} < b-a$. Let $m = \lfloor na \rfloor + 1$. Then $\frac{m}{n} \in (a,b)$.
*Irrational:* If $r$ is rational in $(a,b)$, then $r + \frac{\sqrt{2}}{n}$ is irrational in $(a,b)$ for large enough $n$. $\square$