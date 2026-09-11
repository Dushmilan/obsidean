# Experimental Design

## Definition

The four principles that make experiments convincing:

1. **Control** — hold other variables constant (environment, placebo group).
2. **Randomize** — random assignment balances lurking variables across groups.
3. **Replicate** — enough subjects so random variation can be estimated.
4. **Block** — group similar subjects and randomize *within* groups.

**Blinding:** single-blind (subjects don't know) · double-blind (neither subjects nor researchers know).

## The Intuition

To test a drug you can't just give it to everyone — you need a fair comparison. Groups as alike as possible, *only* the drug differing. Control everything except the one thing you're testing.

## Method

1. Choose the experimental unit and the treatment assignment unit — randomization happens at the *unit* level, not by batch/day.
2. Add blinding to protect the measurement (just as randomization protects assignment).
3. Check replication is adequate before trusting the SE.

## Worked Examples

**Setup:** Website A shown to 2,000 Tuesday visitors; B to 2,000 Wednesday visitors.

**Solution:** Not well designed — layout is confounded with day. Randomly assign *each visitor* to A or B, simultaneously.

**Key insight:** Confounding sneaks in whenever treatment and another variable change together.

---

**Setup:** A drug trial where evaluators know which patients got the drug.

**Solution:** Unblinded evaluation → measurement bias. Double-blind fixes it.

**Key insight:** Blinding protects outcome measurement; randomization protects treatment assignment.

## Common Traps

- "Random" ≠ "haphazard" — it uses a chance mechanism
- Randomization doesn't fix small samples or unblinded measurement
- Blocking ≠ stratifying — blocking is for experiments, stratification for sampling
- Confusing the experimental unit with the assignment unit

## Connections

- [[Observational Studies vs Experiments]] — why design matters
- [[Least Squares regression]] — replication gives the SE meaning
- [[Bias in Sampling]] — measurement bias parallels
