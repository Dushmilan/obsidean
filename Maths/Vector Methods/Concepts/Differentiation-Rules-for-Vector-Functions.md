---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-13, 13-2-derivatives-integrals, differentiation-rules-for-vector-functions]
parent: [[Vector Methods/13.2-Derivatives-and-Integrals.md]]
---

# Differentiation Rules for Vector Functions

> **Stewart Calculus, Chapter 13, Section 13.2**

## Definition
Differentiation rules for vector functions extend scalar calculus rules to vector-valued functions. Let $c(t)$ be a scalar function and $\mathbf{u}(t)$, $\mathbf{v}(t)$ be vector functions.

**Product with a scalar function:**
$$
\frac{d}{dt}[c(t)\mathbf{u}(t)] = c'(t)\mathbf{u}(t) + c(t)\mathbf{u}'(t)
$$

**Dot product:**
$$
\frac{d}{dt}[\mathbf{u}(t) \cdot \mathbf{v}(t)] = \mathbf{u}'(t) \cdot \mathbf{v}(t) + \mathbf{u}(t) \cdot \mathbf{v}'(t)
$$

**Cross product:**
$$
\frac{d}{dt}[\mathbf{u}(t) \times \mathbf{v}(t)] = \mathbf{u}'(t) \times \mathbf{v}(t) + \mathbf{u}(t) \times \mathbf{v}'(t)
$$

## Key Properties
- The dot product rule yields a scalar result
- The cross product rule preserves the order of factors; cross product is anti-commutative, so $\mathbf{u}(t) \times \mathbf{v}(t) \neq \mathbf{v}(t) \times \mathbf{u}(t)$
- Chain rule applies: if $\mathbf{r}(t) = \mathbf{u}(g(t))$, then $\mathbf{r}'(t) = \mathbf{u}'(g(t)) \cdot g'(t)$

## Worked Example
Let $\mathbf{u}(t) = \langle t, t^2, 0 \rangle$ and $\mathbf{v}(t) = \langle \sin t, \cos t, 0 \rangle$. Compute $(\mathbf{u} \times \mathbf{v})'$.

First, compute the derivatives:
- $\mathbf{u}'(t) = \langle 1, 2t, 0 \rangle$
- $\mathbf{v}'(t) = \langle \cos t, -\sin t, 0 \rangle$

Compute each term of the product rule:
- $\mathbf{u}'(t) \times \mathbf{v}(t) = \langle 0, 0, -\sin t - 2t\cos t \rangle$
- $\mathbf{u}(t) \times \mathbf{v}'(t) = \langle 0, 0, t(-\sin t) - t^2 \cos t \rangle$

Therefore:
$$
(\mathbf{u} \times \mathbf{v})' = \langle 0, 0, -\sin t - 2t\cos t - t\sin t - t^2\cos t \rangle
$$

## Related Concepts
- [[Derivative-of-a-Vector-Function]]
- [[Vector-Valued-Functions]]

---

*Part of [[Vector Methods/13.2-Derivatives-and-Integrals.md|13.2 Derivatives and Integrals]]*
