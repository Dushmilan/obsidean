# Undetermined Coefficients

The homogeneous machinery gives you the "free response" of a linear system. To capture the "forced response" — the particular solution driven by an external input like $\sin 3x$ or $e^{2x}$ — undetermined coefficients is the fast lane: *guess* a solution shaped like the input, then let algebra pin down the coefficients.

**The Intuition:** Linear systems echo their inputs. Push a system with a sine wave and it settles into a sine wave of the same frequency, maybe shifted; push it with an exponential and it responds with that exponential. So we submit a trial solution built from the same family as the input — sines/cosines for trig inputs, polynomials for polynomial inputs — leaving coefficients "undetermined" as unknowns. Substitution turns the ODE into linear equations for those unknowns. The one wrinkle: if your guess collides with a homogeneous solution, multiply by $x$ until it doesn't.

**The Math:** For

$$ay'' + by' + cy = g(x),$$

first solve the homogeneous part (07-Homogeneous-Constant-Coefficients) to get $y_c$. Then pick a trial $y_p$ from the table:

| If $g(x)$ is… | Trial $y_p$ |
|---|---|
| polynomial degree $n$ | $A_nx^n + \cdots + A_1x + A_0$ |
| $Ce^{kx}$ | $Ae^{kx}$ |
| $\cos kx$ or $\sin kx$ | $A\cos kx + B\sin kx$ (both!) |
| products/sums | corresponding products/sums |

**Duplication rule:** if any term of the trial solves the homogeneous equation, multiply the whole trial by $x^s$, smallest $s$ that removes all duplication.

Substitute $y_p$ into the ODE, equate coefficients of matching functions, solve. Full answer: $y = y_c + y_p$.

**Setup:** Solve $y'' - 5y' + 6y = 18e^{3x}$ — the resonant case.

**Solution:** Homogeneous: $(r-2)(r-3) = 0$, so $y_c = c_1e^{2x} + c_2e^{3x}$. Naive guess $Ae^{3x}$ duplicates $c_2e^{3x}$! Multiply by $x$: try $y_p = Axe^{3x}$. Then $y_p' = A e^{3x}(1 + 3x)$, $y_p'' = Ae^{3x}(6 + 9x)$. Substitute: $Ae^{3x}\big[(6+9x) - 5(1+3x) + 6x\big] = Ae^{3x}(1) = 18e^{3x}$. So $A = 18$:
$$y = c_1e^{2x} + c_2e^{3x} + 18xe^{3x}.$$

**Key insight:** The collision test happens against $y_c$, not against the equation — check before substituting, and bump by $x^s$ when needed.

**Setup:** Solve $y'' + 4y = \sin 3x$.

**Solution:** $r = \pm 2i$: $y_c = c_1\cos 2x + c_2\sin 2x$. No duplication with frequency 3. Try $y_p = A\cos3x + B\sin3x$: $y_p'' = -9y_p$, so $y_p'' + 4y_p = -5A\cos3x - 5B\sin3x \stackrel{!}{=} \sin3x$. Hence $A = 0$, $B = -\tfrac15$:
$$y = c_1\cos2x + c_2\sin2x - \tfrac{1}{5}\sin3x.$$

**Key insight:** Always include *both* sine and cosine in the trial even if only one appears in $g$ — derivatives mix them, so a half-guess generally fails.

**Setup:** Solve $y'' - 2y' + y = x$.

**Solution:** $(r-1)^2 = 0$: $y_c = (c_1 + c_2x)e^{x}$. Polynomial input: try $y_p = Ax + B$; no duplication. Then $y_p'' - 2y_p' + y_p = 0 - 2A + Ax + B \stackrel{!}{=} x$. Match: $A = 1$, $B - 2A = 0 \Rightarrow B = 2$:
$$y = (c_1+c_2x)e^{x} + x + 2.$$

**Key insight:** Constant terms are degree-0 polynomials — they belong in the trial even when $g$ shows only $x$.

---

### Additional Notes

Limits of validity: $g(x)$ must come from polynomials, exponentials, sines, cosines — finite families closed under differentiation. Inputs like $\tan x$, $\ln x$, $1/x$ need variation of parameters instead (09-Variation-of-Parameters). And when an input *is* itself a homogeneous solution, the $x^s$ bump is not optional bookkeeping — it is resonance showing up in algebra: the forcing pushes at a frequency the system already owns, so the response grows like $x$ times the natural mode.

---
