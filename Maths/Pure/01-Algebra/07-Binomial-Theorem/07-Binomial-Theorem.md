
## Definition

For integer $n$ (finite expansion, valid for all $x, y$):

$$(x+y)^n = \sum_{r=0}^{n} \binom{n}{r} x^{n-r} y^r, \qquad \binom{n}{r} = \frac{n!}{r!(n-r)!}$$

For rational $n$ (infinite series, converges only for $|x| < 1$):

$$(1+x)^n = \sum_{r=0}^{\infty} \binom{n}{r} x^r, \qquad \binom{n}{r} = \frac{n(n-1)\cdots(n-r+1)}{r!}$$

## The Intuition

Expanding $(x+y)^n$ means choosing, for each of the $n$ brackets, whether to take $x$ or $y$. The term $\binom{n}{r}x^{n-r}y^r$ counts the configurations with exactly $r$ copies of $y$ — imagine $n$ boxes each holding $x$ or $y$, and the coefficient counts how many arrangements have exactly $r$ of $y$.

## The Toolkit

| Result | Formula | Valid when |
|--------|---------|-----------|
| Integer expansion | $(x+y)^n = \sum_{r=0}^n \binom{n}{r}x^{n-r}y^r$ | $n \in \mathbb{N}$, all $x,y$ |
| Generalised expansion | $(1+x)^n = \sum_{r=0}^\infty \binom{n}{r}x^r$ | $n \in \mathbb{Q}$, $\|x\| < 1$ |
| General term | $T_{r+1} = \binom{n}{r}x^{n-r}y^r$ | $r = 0,1,\dots,n$ |
| Sum of coefficients | $(1+1)^n = 2^n$ | — |
| Alternating sum | $(1-1)^n = 0$ | $n \ge 1$ |
| Pascal's identity | $\binom{n}{r} = \binom{n-1}{r-1} + \binom{n-1}{r}$ | — |
| Linear approximation | $(1+x)^n \approx 1 + nx$ | $\|x\|$ small |

## Derivation

Combinatorial: $\binom{n}{r}$ counts the $x^{n-r}y^r$ configurations (see Intuition). For rational $n$, the coefficients come from $(1+x)^n$'s Taylor expansion about $x = 0$; the radius of convergence is 1. Pascal's identity: the $r$-th item is either included ($\binom{n-1}{r-1}$) or not ($\binom{n-1}{r}$). [Full derivations: 07-Binomial-Theorem-Proofs]


**Expand $(a+bx)^n$ to a few terms:**
1. Factor out the constant: $(a+bx)^n = a^n\left(1 + \frac{b}{a}x\right)^n$.
2. Apply the generalised theorem (check $\left|\frac{b}{a}x\right| < 1$ for rational $n$).
3. Compute terms with $\binom{n}{r}$ for $r = 0, 1, 2, \dots$

**Find a specific coefficient:** write the general term $T_{r+1}$, set the power of $x$ equal to the target, solve for $r$, evaluate.

**Approximation near 1:** $(1 + \varepsilon)^n \approx 1 + n\varepsilon$ (first-order) or keep the quadratic term for more accuracy.

## Worked Examples

**Setup:** Expand $(2-3x)^5$ up to $x^3$.

**Solution:** Factor out: $2^5\left(1 - \tfrac32 x\right)^5$. Then
$$32\left[1 + 5\left(-\tfrac32 x\right) + 10\left(-\tfrac32 x\right)^2 + 10\left(-\tfrac32 x\right)^3 + \cdots\right] = 32 - 240x + 720x^2 - 1080x^3 + \cdots$$

**Key insight:** Factor out the constant to get the standard $(1+u)^n$ form.

---

**Setup:** Approximate $\sqrt{1.02}$.

**Solution:** $\sqrt{1.02} = (1+0.02)^{1/2} \approx 1 + \tfrac12(0.02) - \tfrac18(0.02)^2 = 1 + 0.01 - 0.00005 = 1.00995$.

**Key insight:** Use the binomial expansion for small perturbations around 1.

---

**Setup:** Find the coefficient of $x^4$ in $\left(2x - \frac{3}{x}\right)^{10}$.

**Solution:** General term $T_{r+1} = \binom{10}{r}(2x)^{10-r}\left(-\frac{3}{x}\right)^r = \binom{10}{r}2^{10-r}(-3)^r x^{10-2r}$. Set $10-2r = 4 \Rightarrow r = 3$. Coefficient: $\binom{10}{3}2^7(-3)^3 = 120 \times 128 \times (-27) = -414720$.

**Key insight:** Match the exponent of $x$ to find which term contributes, then compute.

## Common Traps

- Forgetting $\binom{n}{r} = \binom{n}{n-r}$ symmetry (saves computation)
- Applying the integer theorem to rational $n$ without checking $|x| < 1$
- Off-by-one in the general term: the $k$-th term uses $r = k-1$
- Missing the sign when the second term is negative
- Generalised $\binom{n}{r}$ for rational $n$ is **not** $\frac{n!}{r!(n-r)!}$ — use the falling-product form

## Connections

- 06-Permutations-Combinations — the coefficients are combinations
- 05-Partial-Fractions — series expansion of rational functions
- 04.7-Series-Expansions — Taylor/Maclaurin series
- 04.1-Limits-Continuity — limits of $(1+x)^n$


## Cross-Track Connections

*Reconstructed 2026-08-24 after the registry-loss incident — see [[Maths-MOC]].*

> Original links unrecoverable; topic-level mappings live in [[Maths-Cross_Index]].
