# Bias in Sampling

## Definition

Bias is **systematic error** pushing a statistic away from the true value in a consistent direction. Unlike sampling variability, it does **not** shrink when you collect more data:

$$\bar{x} = \mu + \text{bias} + \text{sampling error}$$

| Type | Source | Example |
|------|--------|---------|
| **Selection** | frame misses part of the population | phone polls before cell phones |
| **Nonresponse** | non-responders differ from responders | busy professionals skip the survey |
| **Response** | wording/interviewer distorts answers | leading questions |
| **Voluntary response** | motivated people opt in | call-in shows, online polls |

## The Intuition

A scale off by 3 kg: every reading is wrong in the same direction no matter how many times you weigh yourself — that's bias. Random error is a scale sometimes high, sometimes low — it averages out. Bias doesn't.

## Method

1. Check the **frame** first: does the list contain the whole target population?
2. Identify which bias type each design flaw produces.
3. Remember: no formula can fix a biased sample.

## Worked Examples

**Setup:** A sleep-habits study emails only students enrolled in an 8 am class.

**Solution:** **Selection bias** — 8 am enrollees aren't representative of all students.

**Key insight:** If the frame doesn't match the population, no sampling method fixes it.

---

**Setup:** Landline poll 9am–5pm weekdays.

**Solution:** **Selection bias** (mobile-only people excluded) **+ nonresponse bias** (employed people don't answer).

**Key insight:** Real surveys stack several biases, often compounding in the same direction.

## Common Traps

- Confusing bias with sampling variability — variability shrinks with $n$, bias doesn't
- "Random" sampling from the *wrong frame* is still biased
- Trusting a confidence interval when bias is present — it's centred in the wrong place

## Connections

- [[Sampling Methods]] · [[Introduction]] — what intervals can and can't do
- [[Physics/01-Measurement/01.2-Errors-Uncertainties]] — systematic vs random error
