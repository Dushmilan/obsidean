# Sampling Methods

## Definition

How you collect data determines everything after it — a bad sample produces bad conclusions no matter the analysis. Key term: the **sampling frame** — the list of all individuals the sample is drawn from.

| Method | How it works | Strength | Weakness |
|--------|--------------|----------|----------|
| **SRS** | every individual equal chance, random generator | unbiased, simple | can miss small subgroups by chance |
| **Stratified** | split into homogeneous strata, SRS *within each* | guarantees representation, more precise | needs a good stratification variable |
| **Cluster** | pick whole natural groups, sample everyone in them | cheap for spread clusters | less precise |
| **Systematic** | every $k$-th from an ordered list | simple, spread | can hit hidden periodic patterns |
| **Convenience** | whoever is easiest to reach | fast | almost always biased — avoid for inference |

## The Intuition

Tasting soup with one spoonful — it tells you about the whole pot *only if* it's stirred first. Sampling methods are different ways of "stirring" before you taste.

## The Physics/Math

The standard error $\text{SE}(\hat p) = \sqrt{\frac{\hat p(1-\hat p)}{n}}$ **assumes a random sample**. With a convenience sample that formula is meaningless — the CLT conditions fail because observations are no longer independent or representative.

## Method

1. Define the target population and build the sampling frame.
2. Choose the method by the structure (strata → stratified; geography → cluster).
3. Verify the sample is random before applying any inference formulas.

## Worked Examples

**Setup:** A school of 900 wants a stratified sample by grade (300/250/200/150).

**Solution:** Sample proportionally — 30, 25, 20, 15 students per grade = 90 total; combine.

**Key insight:** Stratification guarantees every grade is represented — less variability than a plain SRS.

---

**Setup:** A magazine survey card, ~2% mailed back.

**Solution:** Voluntary response sample — the motivated (angry or delighted) respond.

**Key insight:** Among the most biased methods; results are anecdotes, not estimates.

## Common Traps

- Assuming a *larger* sample fixes a *biased* method — bias doesn't shrink with $n$
- Precision (SE) and representativeness are different things
- "Random" ≠ "haphazard" — it needs a chance mechanism
- Frame mismatch: sampling list ≠ target population

## Connections

- [[Bias in Sampling]] · [[Central Limit Theorem]] — the conditions
- [[Estimating a population proportion]] — the SE formula
- [[Physics/01-Measurement/01.2-Errors-Uncertainties]] — measurement error parallels
