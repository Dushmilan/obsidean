# Observational Studies vs Experiments

## Definition

The key difference is **who decides the treatment**:

- **Observational study:** the researcher records who is exposed; individuals/nature chose. Establishes **association**, not causation — confounders are uncontrolled.
- **Experiment:** the researcher **randomly assigns** subjects to treatment/control. Random assignment balances confounders across groups, so outcome differences can be attributed to the treatment.

## The Intuition

Coffee drinkers live longer — does coffee *cause* it? Coffee drinkers may also exercise more, smoke less, or earn more. Those are **confounders** tangled with the treatment. Experiments cut the knot by *controlling* who gets it.

## Method

1. Ask: *how was treatment assigned?* Random assignment → experiment; self-selection/nature → observational.
2. For observational data, always ask: *what else could explain this association?*
3. Regression on observational data finds associations — only randomised experiments support causal slopes.

## Worked Examples

**Setup:** People sleeping >8 h have higher mortality than 7 h sleepers.

**Solution:** Observational — illness may cause both the long sleep and mortality (confounder). An experiment forcing long sleep would be unethical and still wouldn't settle it.

**Key insight:** A hidden variable can cause both treatment and outcome.

---

**Setup:** A farmer randomly assigns 10 plots to new fertilizer, 10 to old.

**Solution:** Randomised comparative experiment — soil differences balance across groups; chance is the main competing explanation.

**Key insight:** Random assignment *balances* confounders, it doesn't eliminate them.

## Common Traps

- A *large* observational study can't overcome confounding — size helps precision, never validity
- "Correlation doesn't imply causation" is structural, not a slogan
- Confounding can hide even when the association is strong and repeatable

## Connections

- [[Sampling Methods]] · [[Linear Regression]] — association vs causation
- [[Experimental Design]] — the experiment counterpart
