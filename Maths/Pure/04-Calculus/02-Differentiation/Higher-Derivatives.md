
## Definition

Differentiate repeatedly: $f''(x)$ (second derivative), $f'''(x)$, $\dots$, $f^{(n)}(x)$. Notation:

$$f', f'', f^{(n)} \quad \text{or} \quad \frac{dy}{dx}, \frac{d^2y}{dx^2}, \frac{d^ny}{dx^n}$$

**Meanings:** $f''$ = rate of change of the slope (curvature); $f'' > 0$ concave up, $f'' < 0$ concave down.

## The Intuition

If $f'$ is the speedometer, $f''$ is how fast the needle itself moves — acceleration. Higher derivatives measure ever-finer changes: jerk (3rd), snap (4th).

## The Toolkit

| Derivative | Meaning |
|------------|---------|
| $f'(x)$ | slope / rate of change |
| $f''(x)$ | curvature; concavity test |
| $f'''(x)$ | jerk (rate of change of acceleration) |
| $f^{(n)}(x)$ | n-th rate |

## Derivation

Each derivative is the limit definition applied to the previous one. There's no new theory — just repeated application and cleaner notation. [Full derivations: 04.2-Differentiation-Proofs]

## Method

1. Differentiate step by step; watch notation.
2. Polynomials: each differentiation lowers the degree — the $n$-th derivative of a degree-$n$ polynomial is constant.
3. $e^x$ and $\sin x$ cycle: $e^x$ stays, trig repeats every 4.

## Worked Examples

**Setup:** Find $f^{(3)}$ for $f(x) = x^4$.

**Solution:** $f' = 4x^3$; $f'' = 12x^2$; $f''' = 24x$.

**Key insight:** Each step drops the degree by 1.

---

**Setup:** $f(x) = \sin x$. What is $f^{(4)}(x)$?

**Solution:** $\cos \to -\sin \to -\cos \to \sin$: $f^{(4)} = \sin x$ — period 4.

**Key insight:** Trig derivatives cycle; useful for Taylor series.

## Common Traps

- Notational slips: $\frac{d^2y}{dx^2}$ is not $\left(\frac{dy}{dx}\right)^2$
- Forgetting the chain rule still applies at each level
- Mixing up concavity sign ($f'' > 0$ = concave up)

## Connections

- Rules-of-Differentiation · 04.3-Applications-of-Differentiation
- 04.7-Series-Expansions — Taylor coefficients use $f^{(n)}$


## Cross-Track Connections

*Reconstructed 2026-08-24 after the registry-loss incident — see [[Maths-MOC]].*

- [[01-Kinematics_Index]] — position → velocity → acceleration chain
