# Applications of First-Order ODEs

Every technique in the last three notes exists because nature keeps asking the same question: "you know the *rate* — what's the *amount*?" Four classic models cover most exam and textbook territory: growth/decay, cooling, mixing, and free-fall with drag.

**The Intuition:** First-order modeling is translation. English sentence → rate equation: "grows in proportion to itself" → $y' = ky$; "cooling at a rate proportional to the temperature gap" → $T' = k(T - T_s)$; "inflow minus outflow" → $S' = r_{in}c_{in} - r_{out}\frac{S}{V}$; "gravity down, drag up" → $v' = g - \frac{c}{m}v$. The mathematics never changes — only the costume. Solve once, reuse forever.

**The Math:**

**Exponential growth/decay:** $\dfrac{dN}{dt} = kN \Rightarrow N(t) = N_0 e^{kt}$.
Doubling time: $t_d = \dfrac{\ln 2}{k}$; half-life: $t_h = \dfrac{\ln 2}{|k|}$. Independent of $N_0$ — the signature of exponential behavior.

**Newton's law of cooling:** $\dfrac{dT}{dt} = k(T - T_s)$ for ambient $T_s$, $k<0$:
$$T(t) = T_s + (T_0 - T_s)e^{kt}.$$
Temperature decays exponentially toward ambient; the gap $T - T_s$ halves every $t_h$.

**Mixing:** tank volume $V$, salt amount $S(t)$, equal flow rates $r$:
$$\frac{dS}{dt} = c_{in}r - \frac{r}{V}S \quad\Longrightarrow\quad S \to c_{in}V \text{ as } t \to \infty.$$
Linear equation; solution approaches the inflow equilibrium regardless of initial salt (03-Linear-First-Order-and-Integrating-Factor).

**Free fall with drag:** $m\dfrac{dv}{dt} = mg - cv$:
$$v(t) = \frac{mg}{c} + \left(v_0 - \frac{mg}{c}\right)e^{-ct/m},$$
approaching terminal velocity $v_\infty = mg/c$: drag grows with speed until it exactly cancels weight.

**Setup:** A bacteria culture triples every 4 hours. When does it reach 10× its starting size?

**Solution:** Tripling: $e^{4k} = 3 \Rightarrow k = \tfrac{\ln 3}{4}$. Want $e^{kt} = 10$: $t = \dfrac{\ln 10}{\ln 3 / 4} = 4\dfrac{\ln 10}{\ln 3} \approx 8.38$ h.

**Key insight:** Never find $N_0$ first — ratios like "triples" pin down $k$ directly through $N(t)/N_0$.

**Setup:** Coffee at 90°C sits in a 20°C room; after 10 min it is 60°C. When is it 40°C?

**Solution:** Gap model: $G = T - 20$, $G' = kG$, so $G = G_0 e^{kt}$. From $G(0) = 70$, $G(10) = 40$: $e^{10k} = \tfrac47$. For $T = 40$: gap $= 20 = 70e^{kt} \Rightarrow kt = \ln\tfrac{2}{7}$. Then $t = 10\,\dfrac{\ln(2/7)}{\ln(4/7)} \approx 27.9$ min.

**Key insight:** Track the *gap* to ambient, not temperature itself — it satisfies pure exponential decay.

**Setup:** Skydiver, $m = 80$ kg, drag $c = 16$ kg/s, jumps from rest. Find speed after 5 s ($g = 9.8$).

**Solution:** Terminal velocity: $mg/c = 49$ m/s. With $v_0 = 0$:
$v(5) = 49\left(1 - e^{-16\cdot5/80}\right) = 49(1 - e^{-1}) \approx 30.98$ m/s.

**Key insight:** Time-scale here is $m/c = 5$ s — after one scale-time, roughly $63\%$ of the way to terminal velocity.

---

### Additional Notes

Modeling checklist used by all four examples above: name the quantity and its units → write the balance law ("rate = gains − losses") → identify parameters from given data → solve with the matching technique (02-Separable-Equations or 03-Linear-First-Order-and-Integrating-Factor) → sanity-check limits ($t\to\infty$, $t=0$). First-order tools now complete; next we build the general architecture that makes *all* linear equations tractable, starting from the theory of two-dimensional solution spaces in 06-Second-Order-Linear-Theory-Wronskian.

---
