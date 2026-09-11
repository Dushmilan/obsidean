---
date: 2026-08-23
type: supplement
course: GP1
base: Physics/03-Thermal-Physics
tags: [university, gp1, physics, thermal, engines, heat-pumps, carnot, supplement]
aliases: [Thermal Machines, Heat Engines and Refrigerators]
---

> **Classification:** [[Physics/03-Thermal-Physics/03-Thermal-Physics_Index|Thermal Physics]] · [[Physics/03-Thermal-Physics/03.5-Second-Law|03.5 Second Law & Entropy]] · [[Physics/03-Thermal-Physics/03.1-Temperature-Heat|03.1 Temperature & Heat]] | **Base:** `Physics/03-Thermal-Physics/` | **Up:** [[03-Thermal-Physics_Index]] → [[Physics_Index]] → [[Vault-Index]] | **Origin:** [[Physics/Cross/GP1-Syllabus-Map|GP1 Syllabus Map]]

# GP1 Supplement — Engines, Refrigerators & Heat Pumps

Run the Second Law forwards and you get engines; run it backwards and you get refrigerators and heat pumps. Same loop, opposite goal — and the arithmetic of efficiency flips meaning along the way.

**The Intuition:** An engine is a water wheel for heat: it rides the fall from hot to cold, extracting useful work on the way down. A refrigerator is the wheel driven backwards: you pay work to pump heat from a cold interior up to a warm room. A heat pump is the same backwards wheel judged by a different scoreboard — instead of asking "how much heat did I remove from the cold side?" it asks "how much warmth did I deliver to my room?" Identical hardware, different accounting, wildly different-looking percentages.

**The Math:**

**Heat engine:** absorbs $Q_H$ from a hot reservoir at $T_H$, dumps $Q_C$ to a cold one at $T_C$, delivers

$$W = Q_H - Q_C, \qquad \eta = \frac{W}{Q_H} = 1 - \frac{Q_C}{Q_H}.$$

**Carnot limit** (best possible, reversible): since $Q_C/Q_H \ge T_C/T_H$,

$$\eta_{\max} = 1 - \frac{T_C}{T_H} \quad (\text{Kelvin!})$$

No engine beats this; equality only for a perfectly reversible cycle.

**Refrigerator (goal: cool the cold side):**

$$W = Q_H - Q_C, \qquad \text{COP}_{\text{R}} = \frac{Q_C}{W} = \frac{Q_C}{Q_H - Q_C}.$$

For a Carnot refrigerator, $\text{COP}_{\text{R}} = \dfrac{T_C}{T_H - T_C}$.

**Heat pump (goal: warm the hot side):**

$$\text{COP}_{\text{HP}} = \frac{Q_H}{W} = \frac{Q_H}{Q_H - Q_C} = \frac{T_H}{T_H - T_C} = \text{COP}_{\text{R}} + 1.$$

Unlike $\eta < 1$ always, COP routinely exceeds 1 — moving heat is cheaper than making it.

**Setup:** Engine takes $500\,$J from a $600\,$K reservoir, exhausts $300\,$J at $300\,$K. Actual vs maximum efficiency?

**Solution:** Actual: $\eta = (500-300)/500 = 0.40$. Carnot: $\eta_{\max} = 1 - 300/600 = 0.50$. Actual is below the limit ⇒ physically allowed.

**Key insight:** Always compare against Carnot using Kelvin temperatures — Celsius values give wrong ratios ($-273$ offsets matter).

**Setup:** Fridge removes $200\,$J per cycle from its interior using $80\,$J of work. COP and rejected heat?

**Solution:** $\text{COP}_R = 200/80 = 2.5$. Rejected: $Q_H = 200 + 80 = 280\,$J per cycle into the kitchen.

**Key insight:** Energy conservation forces $Q_H = Q_C + W$: a fridge heats the room more than it cools its box.

**Setup:** Same device as heat pump: COP?

**Solution:** $\text{COP}_{HP} = Q_H/W = 280/80 = 3.5 = 2.5 + 1$.

**Key insight:** The identity $\text{COP}_{HP} = \text{COP}_R + 1$ is pure bookkeeping ($Q_H$ vs $Q_C$ numerator) — instant exam points.

**Setup:** Can a heat pump have $\text{COP}_{HP} = 10$? Conditions?

**Solution:** Need $T_H/(T_H - T_C) = 10$, i.e. $T_H - T_C = T_H/10$. For $T_H = 293\,$K: gap $\approx 29\,$K. Plausible on mild days; hopeless in deep winter — exactly why heat pumps struggle in extreme cold.

**Key insight:** COP collapses as the temperature gap widens; the denominator $(T_H - T_C)$ punishes ambition.

---

### Additional Notes

Everything here sits on the Second Law's two statements — Kelvin–Planck ("no engine converts heat to work with certainty") and Clausius ("heat never flows cold→hot unaided") — which are logically equivalent. See [[Physics/03-Thermal-Physics/03.5-Second-Law|Second Law]] for entropy framing, [[Physics/03-Thermal-Physics/03.6-Kinetic-Theory|Kinetic Theory]] for where $Q$ lives microscopically, and [[GP1-Supplement-Zeroth-Law|the Zeroth Law]] for why $T$ labels exist at all.

> **Attached to:** [[Physics/03-Thermal-Physics/03-Thermal-Physics_Index]] (as 03.7 Thermal Machines — extends 03.5) · [[Physics/Cross/Physics-Cross-Index]] · [[Physics/03-Thermal-Physics/03.1-Temperature-Heat|03.1 Temperature & Heat]]

---
