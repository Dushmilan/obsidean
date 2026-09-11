
## Definition

- **Implicit:** differentiate equations where $y$ isn't isolated ($x^2 + xy + y^2 = 7$); every $y$-term gets a $\frac{dy}{dx}$ factor.
- **Logarithmic:** take $\ln$ of both sides to turn powers/products into sums, then differentiate — the standard for $f(x)^{g(x)}$.

## The Intuition

When $y$ is buried inside the equation, treat it as $y(x)$: differentiating $y^2$ gives $2y\cdot y'$ (chain rule). Taking logs converts $y = x^x$ into $\ln y = x\ln x$ — products become sums, exponents become factors.

## The Toolkit

| Technique | When | Key move |
|-----------|------|----------|
| Implicit | $y$ not isolated | every $y$ → $y'$ factor |
| Logarithmic | $f(x)^{g(x)}$, heavy products | $\ln$ both sides, differentiate, solve for $y'$ |
| Parametric | $x(t), y(t)$ | $\frac{dy}{dx} = \frac{dy/dt}{dx/dt}$ |

## Derivation

Both are the chain rule. Implicit: $\frac{d}{dx}[y^2] = 2y\frac{dy}{dx}$. Log: $\frac{d}{dx}[\ln y] = \frac{1}{y}y'$, so after differentiating, solve for $y'$. [Full derivations: 04.2-Differentiation-Proofs]

## Method

1. Implicit: differentiate both sides, collect $y'$ terms, factor, solve.
2. Log: $\ln$ both sides → differentiate → multiply by $y$ to isolate $y'$.
3. Parametric: divide the two derivatives.

## Worked Examples

**Setup:** Find $y'$ for $x^2 + xy + y^2 = 7$.

**Solution:** $2x + y + xy' + 2yy' = 0 \Rightarrow y' = -\frac{2x+y}{x+2y}$.

**Key insight:** Every $y$-term picks up a $y'$ factor.

---

**Setup:** Differentiate $y = x^x$ ($x>0$).

**Solution:** $\ln y = x\ln x$; $\frac{y'}{y} = \ln x + 1$; $y' = x^x(\ln x + 1)$.

**Key insight:** Logs turn the variable exponent into a product.

## Common Traps

- Missing the $y'$ factor in implicit differentiation
- $y = x^x$ isn't power rule material — the exponent is a variable
- Log method requires $y > 0$ (or use $|y|$)
- Forgetting to multiply by $y$ at the end

## Connections

- Rules-of-Differentiation · Higher-Derivatives
- 03-Logarithms — the log rules
- 04.9-Multiple-Integrals — implicit in multiple variables
