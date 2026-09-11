
## Definition

Partial fractions decompose a **proper** rational function into a sum of simpler fractions:

$$\frac{P(x)}{Q(x)} = \sum \text{simpler fractions}, \qquad \deg(P) < \deg(Q)$$

If $\deg(P) \ge \deg(Q)$, perform polynomial long division **first** (the fraction is improper).

## The Intuition

"Un-mixing" a combined fraction. Integrating $\frac{5x+1}{(x-1)(x+2)}$ directly is hard; integrating $\frac{2}{x-1} + \frac{3}{x+2}$ is trivial. The decomposition turns a hard rational function into easy pieces. Every real polynomial factors into linear and irreducible quadratic factors, so the form of the decomposition is fully determined by the factors of $Q(x)$.

## The Toolkit

| Factor of $Q(x)$ | Contribution to decomposition |
|---|---|
| Distinct linear $(ax+b)$ | $\dfrac{A}{ax+b}$ |
| Repeated linear $(ax+b)^n$ | $\dfrac{A_1}{ax+b} + \cdots + \dfrac{A_n}{(ax+b)^n}$ |
| Irreducible quadratic $(ax^2+bx+c)$, $b^2-4ac<0$ | $\dfrac{Ax+B}{ax^2+bx+c}$ |
| Repeated quadratic $(ax^2+bx+c)^n$ | $\dfrac{A_1x+B_1}{ax^2+bx+c} + \cdots + \dfrac{A_nx+B_n}{(ax^2+bx+c)^n}$ |
| Cover-up (distinct linear $(x-a)$) | $A = \dfrac{P(a)}{Q'(a)}$, where $Q(x) = (x-a)R(x)$ |

## Derivation

The identity holds for all $x$: multiply through by $Q(x)$, expand, and equate coefficients of equal powers. For distinct linear factors the cover-up trick is the shortcut — substitute $x = a$ and every term vanishes except the one with $(x-a)$ in its numerator position. [Full derivations: 05-Partial-Fractions-Proofs]

## Method

1. **Check proper** — if $\deg P \ge \deg Q$, divide first.
2. **Factor** $Q(x)$ completely (linear + irreducible quadratic factors).
3. **Write the template** from the Toolkit table.
4. **Find constants:**
   - Distinct linear factors → cover-up (fastest).
   - Repeated factors → cover-up for the highest-power constant, then equate coefficients for the rest.
   - Irreducible quadratics → multiply out and equate coefficients by powers of $x$.
5. **Verify** by recombining the fractions (30 seconds, catches most errors).

## Worked Examples

**Setup:** Decompose $\frac{5x+1}{(x-1)(x+2)}$.

**Solution:** Form $\frac{A}{x-1} + \frac{B}{x+2}$. Cover-up for $A$: $A = \frac{5(1)+1}{1+2} = 2$. Cover-up for $B$: $B = \frac{5(-2)+1}{-2-1} = 3$. Result: $\frac{2}{x-1} + \frac{3}{x+2}$.

**Key insight:** Cover-up is the fastest method for distinct linear factors.

---

**Setup:** Decompose $\frac{3x^2+5x+2}{(x+1)^2(x-2)}$.

**Solution:** Form $\frac{A}{x+1} + \frac{B}{(x+1)^2} + \frac{C}{x-2}$. Set $x = -1$: $B = 0$. Set $x = 2$: $C = \frac{8}{3}$. Set $x = 0$: $A = \frac{1}{3}$. Result: $\frac{1}{3(x+1)} + \frac{8}{3(x-2)}$.

**Key insight:** For repeated factors, find the highest-power constant first with cover-up, then equate coefficients for the rest.

---

**Setup:** Decompose $\frac{2x^3+3x^2+4x+5}{(x^2+1)^2}$.

**Solution:** Form $\frac{Ax+B}{x^2+1} + \frac{Cx+D}{(x^2+1)^2}$. Multiply and equate: $A=2$, $B=3$, $C=2$, $D=2$. Result: $\frac{2x+3}{x^2+1} + \frac{2x+2}{(x^2+1)^2}$.

**Key insight:** For irreducible quadratics, equate coefficients systematically by comparing powers.

## Common Traps

- Decomposing an **improper** fraction without dividing first
- Missing a repeated-factor term in the template (need all powers)
- Forgetting the $Ax+B$ form (not just $A$) for irreducible quadratics
- Not checking that $b^2 - 4ac < 0$ before calling a quadratic irreducible
- Skipping the verification step — a wrong constant compounds downstream in integration

## Connections

- 04-Polynomials — factoring $Q(x)$ first
- 04.4-Integration — integrating rational functions
- 04.7-Series-Expansions — binomial series of rational functions
- 08-Mathematical-Induction — related algebraic identities


## Cross-Track Connections

*Reconstructed 2026-08-24 after the registry-loss incident — see [[Maths-MOC]].*

> Original links unrecoverable; topic-level mappings live in [[Maths-Cross_Index]].
