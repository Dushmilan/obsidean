# Higher-Order Linear & Cauchy–Euler

Everything from second-order linear theory lifts to order $n$ with almost no new ideas: the solution space becomes $n$-dimensional, the characteristic polynomial degree $n$, and each root type contributes its own building block. The Cauchy–Euler equation is the grand finale — a variable-coefficient family that converts into constant coefficients by one clever change of variables.

**The Intuition:** A linear $n$-th order system has $n$ "degrees of freedom": position, velocity, acceleration history — $n$ initial readings pin down a unique future. So you need exactly $n$ independent solutions. Each distinct characteristic root supplies one; repeated roots supply their derivatives-with-respect-to-$r$ (powers of $x$ appear); complex pairs supply oscillations. Nothing conceptually new — just more roots to catalogue. The Cauchy–Euler trick: its coefficients are powers of $x$ matched to derivative orders, which is precisely what makes $x^r$ behave like $e^{rx}$ did before.


**Order $n$:**
$$a_n y^{(n)} + \cdots + a_1 y' + a_0 y = g(x), \qquad y_c = c_1Y_1 + \cdots + c_nY_n,$$
with $W(Y_1,\dots,Y_n) \ne 0$ certifying independence ($n\times n$ Wronskian). Existence/uniqueness and superposition carry over verbatim (06-Second-Order-Linear-Theory-Wronskian).

**Characteristic roots → basis functions** (root $m$ multiplicity):

| Root | Contribution |
|---|---|
| distinct real $r$ | $e^{rx}$ |
| real $r$, mult. $m$ | $e^{rx},\, xe^{rx},\, \dots,\, x^{m-1}e^{rx}$ |
| $\alpha \pm i\beta$, mult. $k$ | $e^{\alpha x}\cos\beta x,\ e^{\alpha x}\sin\beta x,\ x e^{\alpha x}\cos\beta x,\ \dots$ (up to $x^{k-1}$) |

Nonhomogeneous equations: $y_p$ via 08-Undetermined-Coefficients (if input fits) or 09-Variation-of-Parameters (always).

**Cauchy–Euler:**
$$ax^2y'' + bxy' + cy = 0.$$
Try $y = x^r$:
$$ar(r-1) + br + c = 0 \quad\Longrightarrow\quad ar^2 + (b-a)r + c = 0.$$

| Roots | Solution on $x>0$ |
|---|---|
| distinct real $r_1, r_2$ | $c_1x^{r_1} + c_2x^{r_2}$ |
| repeated $r$ | $(c_1 + c_2\ln|x|)\,x^{r}$ |
| $\alpha \pm i\beta$ | $x^{\alpha}\big(c_1\cos(\beta \ln|x|) + c_2\sin(\beta\ln|x|)\big)$ |

Substitute $t = \ln|x|$ (then $x = e^t$): every $x^ky^{(k)}$ becomes a *constant*-coefficient combination of $d^ky/dt^k$, reducing the whole equation to 07-Homogeneous-Constant-Coefficients.

**Setup:** Solve $y''' - y'' - 4y' + 4y = 0$.

**Solution:** Characteristic: try $r=2$: $8 - 4 - 8 + 4 = 0$. ✓ Factor:
$r^3 - r^2 - 4r + 4 = (r-2)(r^2+ r - 2)\cdot$… divide: $(r-2)$ goes in once; quotient $r^2 + r - 2 = (r-1)(r+2)$. Roots: $2, 1, -2$. So
$$y = c_1e^{2x} + c_2e^{x} + c_3e^{-2x}.$$

**Key insight:** Rational-root guessing plus polynomial long division factors any cubic with integer roots — no formula needed.

**Setup:** Solve $y^{(4)} + 2y'' + y = 0$.

**Solution:** Characteristic: $r^4 + 2r^2 + 1 = (r^2+1)^2 = 0$: roots $\pm i$ each with multiplicity 2. Basis:
$\cos x, \sin x, x\cos x, x\sin x$.
$$y = (c_1 + c_3x)\cos x + (c_2 + c_4x)\sin x.$$

**Key insight:** Missing odd powers in the characteristic polynomial is normal for even-order equations — factor in $r^2$ as a block.

**Setup:** Solve $x^2y'' + xy' + y = 0$ for $x>0$.

**Solution:** Trial $x^r$: $r(r-1) + r + 1 = r^2 + 1 = 0 \Rightarrow r = \pm i$. Complex pair $\alpha=0, \beta=1$:
$$y = c_1\cos(\ln x) + c_2\sin(\ln x).$$

**Key insight:** Cauchy–Euler answers look like stretched/squeezed waves in log-time — oscillations that slow down as $x$ grows.

---

### Additional Notes

This closes the methods arc of the course: classify (01-ODEs-Terminology-and-Classification) → solve homogeneous by characteristic algebra → build particular solutions → assemble $y = y_c + y_p$. Consolidate everything — first-order toolbox plus linear architecture — in 11-Quiz-Synthesis. For deeper study beyond this course: Laplace transforms handle discontinuous forcing, power series handle non-Cauchy–Euler variable coefficients, and systems $X' = AX$ generalize the entire story to matrices.

---
