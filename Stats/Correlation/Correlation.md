---
date: 2026-07-23
type: stats-concept
source: manual
status: reviewed
tags: [stats, correlation]
---

### What is a correlation coefficient?

The correlation coefficient ‍\[r\] measures the direction and strength of a linear relationship. Calculating ‍\[r\] is pretty complex, so we usually rely on technology for the computations. We focus on understanding what ‍\[r\] says about a scatterplot.

Calculating Coefficient 
$r = \frac{1}{n - 1} \sum \left( \frac{x_i - \bar{x}}{s_x} \right) \left( \frac{y_i - \bar{y}}{s_y} \right)$

$r = \frac{1}{n - 1} \sum z_{x_i} z_{y_i}$

Here are some facts about ‍\[r\]:

- It always has a value between ‍\[-1\] and ‍\[1\].
- Strong positive linear relationships have values of ‍\[r\] closer to ‍\[1\].
- Strong negative linear relationships have values of ‍\[r\] closer to ‍\[-1\].
- Weaker relationships have values of ‍\[r\] closer to ‍\[0\].