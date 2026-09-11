
## Definition

$f$ is **continuous at $a$** iff all three hold:

1. $f(a)$ is defined
2. $\lim_{x\to a}f(x)$ exists (one-sided limits agree)
3. $\lim_{x\to a}f(x) = f(a)$

**Intermediate Value Theorem (IVT):** if $f$ is continuous on $[a,b]$ and $f(a) \ne f(b)$, then $f$ takes every value between them at some $c\in(a,b)$.

## The Intuition

Continuity = no breaks, jumps, or holes. Draw the graph without lifting your pen. A discontinuity is a jump (step), a hole (removable), or a blow-up (infinite).

## The Toolkit

| Type | Example | Removable? |
|------|---------|-----------|
| Removable (hole) | $\frac{x^2-1}{x-1}$ at 1 | yes — define $f(1)=2$ |
| Jump | floor, sign | no |
| Infinite | $\frac1x$ at 0 | no |
| Polynomials/trig/exp | everywhere continuous | — |

## Derivation

Continuity is the limit definition applied at the point itself; IVT follows from the completeness of the real line (no gaps allowed). [Full derivations: 04.1-Limits-Continuity-Proofs]

## Method

1. Test the three conditions at the point of interest.
2. Composites of continuous functions are continuous (justify by composition).
3. IVT: verify continuity on $[a,b]$, then existence of a root/value follows.

## Worked Examples

**Setup:** Show $f(x) = x^3 - x - 1$ has a root in $(1,2)$.

**Solution:** $f$ continuous; $f(1) = -1$, $f(2) = 5$ — signs differ, so IVT gives a root in $(1,2)$.

**Key insight:** IVT turns sign change into existence — the basis of root-finding.

---

**Setup:** Is $\frac{x^2-1}{x-1}$ continuous at 1?

**Solution:** $\lim = 2$ but $f(1)$ undefined — a removable discontinuity.

**Key insight:** Fix it by assigning $f(1) = 2$.

## Common Traps

- All three conditions needed — a defined point and a limit isn't enough
- IVT needs continuity on the *closed* interval
- Polynomials are continuous everywhere — quotients only where the denominator is nonzero

## Connections

- Computing-Limits · 04.1-Limits-Continuity
- 04.3-Applications-of-Differentiation — differentiability implies continuity
- 04-Polynomials — root location
