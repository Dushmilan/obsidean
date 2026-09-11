---
date: 2026-08-16
type: format-guide
tags: [maths, format, template, style-guide]
---

# Math Note Format — Template & Style Guide

The standard structure for every Maths concept note in this vault. It is **not** the CS "IT" format — it is purpose-built for mathematics: precision first (definitions + conditions), then intuition, then formulas, then method, then practice.

---

## The 8 blocks (in order)

```text
1.  Definition          — precise statement + all conditions/domain
2.  The Intuition       — mental model / analogy
3.  The Toolkit         — formula table: Result | Formula | Valid when
4.  Derivation          — "why it works" (short) or link to the Proofs note
5.  Method              — step-by-step problem-solving procedure / decision flow
6.  Worked Examples     — Setup → Solution → Key insight (easy → tricky)
7.  Common Traps        — domain errors, sign errors, extraneous roots, edge cases
8.  Connections         — wikilinks to related topics (calculus ↔ physics, etc.)
```

---

## Block-by-block rules

### 1. Definition
- One or two lines, mathematically precise.
- **Always** state the conditions / domain / "valid when" explicitly.
- Examples of conditions: `a > 0, a ≠ 1, x > 0`; `b² - 4ac ≥ 0`; "constant acceleration only".

### 2. The Intuition
- A real-world analogy or mental model, 1–3 sentences.
- Purpose: make the *shape* of the idea click before the algebra.
- No formulas here unless trivial.

### 3. The Toolkit
- A table: `| Result | Formula | Valid when |`
- Every formula in the note goes here — **never** scattered in prose.
- If a formula has restrictions, they live in the "Valid when" column.
- Keep rows short; the derivation (block 4) explains the "why".

### 4. Derivation
- Short inline derivation (≤ 6 lines) of the *key* result — OR
- A wikilink to the full proof in the sub-vault (``[[04.1-Limits-Continuity-Proofs]]``).
- If both: inline sketch + link to full proof.

### 5. Method
- A numbered, repeatable procedure ("how to attack problems").
- Include decision points: *"If X, do Y; if Z, do W."*
- This is the exam-strategy block — the most valuable one.

### 6. Worked Examples
- Format: `**Setup:**` → `**Solution:**` → `**Key insight:**`
- Order: easy → exam-style → tricky/edge case.
- Use `$$...$$` display math for solutions with multiple steps.
- After each: 1 sentence "Key insight" (the transferable lesson).

### 7. Common Traps
- Bullet list of classic mistakes:
  - Domain violations / extraneous roots
  - Sign conventions (e.g. upward positive → a = −g)
  - Degenerate cases (a = 0, repeated roots, undefined expressions)
  - "Only valid when…" reminders

### 8. Connections
- Bullet list of wikilinks to related notes.
- Both directions: what this topic uses, and what uses this topic.
- Examples: ``[[04.2-Differentiation]]`` · `[[Physics/02-Mechanics/02.3-Work-Energy-Power]]`

---

## LaTeX conventions

| Use | Syntax |
|-----|--------|
| Inline math | `$v = u + at$` |
| Display math | `$$y = e^{\alpha x}(A\cos\beta x + B\sin\beta x)$$` |
| Fractions | `\frac{a}{b}` |
| Sub/superscript | `x_1`, `x^2` |
| Named functions | `\ln`, `\log_a`, `\sin`, `\cos`, `\tan` |
| Greek | `\alpha, \beta, \omega, \theta, \phi` |
| Sets/conditions | `\in`, `\neq`, `\geq`, `\Rightarrow`, `\iff` |

---

## File & frontmatter rules

- **Concept notes:** NO YAML frontmatter — start directly with `# Title`.
- **Index/hub files:** YAML with `type: hub` / `type: section-index` and tags.
- **Proof files:** YAML with `type: proof` and `parent: [[concept note]]`.
- **Naming:** files live in numbered subtopic sub-vaults; index = `<Folder>_Index.md`.

---

## Filled-in example

```markdown
# First-Order Linear Differential Equations

## Definition
A first-order linear ODE has the form  dy/dx + P(x)y = Q(x),  where P and Q
are functions of x only (y and y' appear to the first power, never multiplied).

## The Intuition
You can't integrate y' + Py = Q directly — the left side isn't a plain
derivative. The integrating factor is a multiplier chosen so the left side
becomes exactly d/dx[μy], reducing the whole problem to one integration.

## The Toolkit
| Result | Formula | Valid when |
|--------|---------|-----------|
| Integrating factor | μ = e^{∫P dx} | any first-order linear |
| General solution | y = (1/μ)(∫μQ dx + C) | Q(x) integrable |

## Derivation
μ' = Pμ  ⟹  d/dx[μy] = μy' + μ' y = μ(y' + Py) = μQ.  [full: [[04.6-Differential-Equations-Proofs]]]

## Method
1. Put in standard form (coefficient of y' = 1)
2. Find μ = e^{∫P dx} (drop the constant)
3. Multiply through, recognize d/dx[μy]
4. Integrate both sides, solve for y
5. Apply initial condition if given

## Worked Examples
**Setup:** Solve dy/dx + 2xy = x.
**Solution:** μ = e^{x²};  d/dx[ye^{x²}] = xe^{x²} ⟹ y = ½ + Ce^{-x²}.
**Key insight:** the integrating factor turns the left side into a product rule.

## Common Traps
- Forgetting standard form (y' coefficient must be 1)
- Dropping the integration constant on the right
- Applying the method to nonlinear equations (y², sin y)

## Connections
`[[04.6.2-Second-Order-Linear-Homogeneous]]` · `[[SHM]]` · `[[Newton's cooling]]`
```

---

## Quick checklist before finishing a note

- [ ] Definition states ALL conditions/domain
- [ ] Every formula is in the Toolkit table (not buried in prose)
- [ ] Method is numbered and decision-oriented
- [ ] Examples follow Setup → Solution → Key insight
- [ ] Common Traps covers domain/sign/edge cases
- [ ] Connections has 2+ wikilinks
- [ ] All links resolve; LaTeX renders

---

*Created: 2026-08-16 · Applies to all Pure & Applied concept notes*
