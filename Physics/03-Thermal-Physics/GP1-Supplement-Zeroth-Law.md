---
date: 2026-08-23
type: supplement
course: GP1
base: Physics/03-Thermal-Physics
tags: [university, gp1, physics, thermal, zeroth-law, temperature, supplement]
aliases: [Zeroth Law of Thermodynamics]
---

> **Classification:** [[Physics/03-Thermal-Physics/03-Thermal-Physics_Index|Thermal Physics]] · [[Physics/03-Thermal-Physics/03.1-Temperature-Heat|03.1 Temperature & Heat]] · [[Maths/Abstract-Algebra-1/01-Sets-Relations-Functions|Abstract Algebra 1 — Equivalence Relations]] | **Base:** `Physics/03-Thermal-Physics/` | **Up:** [[03-Thermal-Physics_Index]] → [[Physics_Index]] → [[Vault-Index]] | **Origin:** [[Physics/Cross/GP1-Syllabus-Map|GP1 Syllabus Map]]

# GP1 Supplement — The Zeroth Law of Thermodynamics

The Zeroth Law is the quiet foundation under all of thermodynamics: it is the reason "temperature" is a meaningful, measurable quantity at all. It earned its odd number because it was recognized *after* the First and Second Laws — but logically it comes first.

**The Intuition:** If a cup of coffee tells a thermometer it's 70°C, and that same thermometer tells a bucket of water it's 70°C, you can be certain coffee dropped into the bucket would settle at equilibrium instantly — no heat flows either way. The thermometer is a matchmaker: two bodies agreeing through a third are in agreement with each other. Without this law, every temperature reading would be suspect.

**The Math:** If body $A$ is in **thermal equilibrium** with body $C$ (the thermometer), and body $B$ is in thermal equilibrium with $C$, then $A$ and $B$ are in thermal equilibrium with each other:

$$
(A \sim C) \;\wedge\; (B \sim C) \;\Longrightarrow\; A \sim B.
$$

Thermal equilibrium "$\sim$" is therefore an **equivalence relation** (reflexive, symmetric, transitive — same structure as [[Maths/Abstract-Algebra-1/01-Sets-Relations-Functions|equivalence relations in algebra]]). Its equivalence classes are exactly what we call *temperature*: all bodies in one class share one number $T$. A thermometer works because it can be brought into equilibrium with anything without disturbing it much (tiny heat capacity).

Consequences:
- Heat flows spontaneously only between bodies in different classes, from higher $T$ to lower $T$.
- Defining an empirical scale (Celsius, Kelvin) = choosing how to label the classes; the Kelvin scale is special because it ties labels to efficiency ([[GP1-Supplement-Thermal-Machines]]).

**Setup:** Body A touches B: no heat flows. B touches C: no heat flows. A and C are then brought into contact. What happens?

**Solution:** Nothing observable — both are already at B's temperature, so $T_A = T_B = T_C$ and there is no temperature difference to drive a flow.

**Key insight:** "No heat flow" is the experimental signature of equality of temperature; the Zeroth Law guarantees this relation transfers pairwise.

**Setup:** Why does a small fever thermometer reach equilibrium quickly without cooling you measurably?

**Solution:** Its heat capacity is tiny compared to your body's, so the heat exchanged to equalize $T$ barely changes your state while fully determining its own — ideal thermometer behavior.

**Key insight:** A good thermometer must interact strongly enough to equilibrate but weakly enough not to perturb — the measurement postulate of thermodynamics.

---

### Additional Notes

Chain of thermodynamics for this course: Zeroth Law makes $T$ definable → First Law ([[Physics/03-Thermal-Physics/03.3-First-Law|03.3]]) accounts energy changes via $\Delta U = Q - W$ → Second Law ([[Physics/03-Thermal-Physics/03.5-Second-Law|03.5]]) dictates which directions are possible and sets engine limits → machines exploit these rules ([[GP1-Supplement-Thermal-Machines]]).

> **Attached to:** [[Physics/03-Thermal-Physics/03-Thermal-Physics_Index]] (as 03.0 Foundation — precedes 03.1) · [[Physics/Cross/Physics-Cross-Index|Physics Cross]] · [[Maths/Maths|Mathematics — Equivalence Relations]]

---
