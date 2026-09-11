---
date: 2026-08-16
type: format-guide
tags: [physics, format, template, style-guide]
---

# Physics Note Format — Template & Style Guide

The standard structure for every Physics concept note in this vault. It shares the **8-block skeleton** with the [[Math-Note-Template]] but is tuned for physics: physical quantities first, then the law, then units & measurements, then problem method.

---

## The 8 blocks (in order)

```text
1.  Definition           — the physical quantity / law, precisely stated + SI units
2.  The Intuition        — mental model / real-world analogy
3.  The Physics          — key equations + units table: Quantity | Equation | Units
4.  Derivation           — where the equation comes from (or link to Derivations/)
5.  Method               — step-by-step problem-solving procedure
6.  Worked Examples      — Setup → Solution → Key insight (easy → tricky)
7.  Common Traps         — sign errors, unit errors, idealisations that break
8.  Connections          — wikilinks to related physics/maths notes
```

---

## Block-by-block rules

### 1. Definition
- State the **physical quantity** (or law) precisely.
- Always include **SI units** and the key variables' meanings.
- Example: "Acceleration is the rate of change of velocity: a = dv/dt, units m·s⁻²."

### 2. The Intuition
- A real-world analogy or mental model, 1–3 sentences.
- Examples: "voltage is water pressure", "a capacitor is a water tank".

### 3. The Physics
- A table: `| Quantity | Equation | Units |`
- Every formula lives here — never scattered in prose.
- Include the valid-when/idealisations in the table or as a note under it.

### 4. Derivation
- Short inline derivation (≤ 6 lines) of the key result — OR
- A wikilink to the full derivation in `Derivations/` (`[[../Derivations/...]]`).

### 5. Method
- Numbered, repeatable procedure ("how to attack problems").
- Include decision points and sign conventions.

### 6. Worked Examples
- `**Setup:**` → `**Solution:**` → `**Key insight:**`
- Use `$$...$$` display math.
- Include at least one numerical example with units tracked.

### 7. Common Traps
- Sign conventions (e.g. direction of E-field, induced EMF sign)
- Unit mismatches (J vs kWh, m vs cm)
- Idealisation failures (no air resistance, smooth surfaces, massless strings)
- Vector vs scalar confusion

### 8. Connections
- 2+ wikilinks: related physics topics + the maths that powers them.
- Example: `[[02.2-Dynamics]]` · `[[Maths/Pure/04-Calculus/02-Differentiation/04.2-Differentiation]]`

---

## LaTeX & unit conventions

| Use | Syntax |
|-----|--------|
| Inline math | `$F = ma$` |
| Display math | `$$F = \frac{GMm}{r^2}$$` |
| Units | `m·s⁻²` or `$\text{m s}^{-2}$` |
| Scientific notation | `$6.67 \times 10^{-11}$` |
| Greek | `\omega, \lambda, \phi, \rho, \mu` |

---

## File & frontmatter rules

- **Concept notes:** NO YAML frontmatter — start directly with `# Title`.
- **Index/hub files:** YAML with `type: hub` / `type: section-index`.
- **Derivations:** YAML with `type: proof` and `parent: [[concept note]]`.
- **Naming:** notes live in unit folders; index = `<Folder>_Index.md`.

---

## Filled-in example

```markdown
# Newton's Second Law

## Definition
Net force equals the rate of change of momentum: F = dp/dt = ma for constant
mass. SI units: newton (N) = kg·m·s⁻². m = inertial mass, a = acceleration.

## The Intuition
A shopping trolley: push harder → faster acceleration; heavier trolley →
slower. F = ma is the "push ↔ response" rule for everything that moves.

## The Physics
| Quantity | Equation | Units |
|----------|----------|-------|
| Force | F = ma | N (kg·m·s⁻²) |
| Momentum | p = mv | kg·m·s⁻¹ |
| Weight | W = mg | N |
| Impulse | J = FΔt = Δp | N·s |

Valid when: inertial frame; m constant (else use F = dp/dt).

## Derivation
F = dp/dt is the fundamental statement; with constant mass it reduces to
F = ma. [full: [[Derivations/02-Mechanics/02.1-...]]]

## Method
1. Draw the free-body diagram
2. Choose axes along motion
3. Write F = ma per axis
4. Solve

## Worked Examples
**Setup:** A 2 kg block is pushed by 10 N on a smooth floor. Find a.
**Solution:** a = F/m = 10/2 = 5 m·s⁻².
**Key insight:** On smooth surfaces, the normal balances weight — only the
horizontal push accelerates the block.

## Common Traps
- Using F = ma where mass changes (use dp/dt)
- Mixing weight (N) and mass (kg)
- Forgetting inertial frames

## Connections
[[02.2-Dynamics]] · [[04.2-Differentiation]] · [[02.1-Kinematics]]
```

---

## Quick checklist

- [ ] Definition has SI units
- [ ] All equations in the table
- [ ] Method is numbered
- [ ] At least one numerical example
- [ ] Traps cover sign/unit/idealisation errors
- [ ] 2+ Connections

---

*Created: 2026-08-16 · Applies to all Physics concept notes*
