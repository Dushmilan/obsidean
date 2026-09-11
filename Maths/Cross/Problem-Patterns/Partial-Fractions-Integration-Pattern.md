---
date: 2026-08-16
type: problem-pattern
tags: [maths, pure, patterns, algebra, calculus]
parent: [[04-Calculus_Index]]
---

# Partial Fractions — Integration Patterns

Splitting a rational function into simpler pieces turns one ugly integral into a sum of easy ones. The whole skill is matching the denominator's factor structure to the correct template.

## Template A: Distinct linear factors

$$\frac{P(x)}{(x-a)(x-b)(x-c)} = \frac{A}{x-a} + \frac{B}{x-b} + \frac{C}{x-c}$$

**Coefficients:** cover-up rule — for $A$, multiply through by $(x-a)$ and set $x=a$.

Integrate: $\int \frac{A}{x-a}\,dx = A\ln|x-a|$.

## Template B: Repeated linear factors

$$\frac{P(x)}{(x-a)^2(x-b)} = \frac{A}{x-a} + \frac{B}{(x-a)^2} + \frac{C}{x-b}$$

Integrate: $\int \frac{B}{(x-a)^2}\,dx = -\frac{B}{x-a}$.

## Template C: Irreducible quadratic factor

$$\frac{P(x)}{(x^2 + px + q)(x-a)} = \frac{Ax+B}{x^2+px+q} + \frac{C}{x-a}$$

Complete the square in the quadratic, then integrate via $\ln$ and $\arctan$:
$\int \frac{2x+p}{x^2+px+q}\,dx = \ln|x^2+px+q|$, and
$\int \frac{1}{(x-h)^2+k^2}\,dx = \frac{1}{k}\arctan\frac{x-h}{k}$.

## Example 1: Distinct linear

**Setup:** $\int \frac{5x+1}{(x-2)(x+1)}\,dx$.

**Solution:** $\frac{5x+1}{(x-2)(x+1)} = \frac{A}{x-2} + \frac{B}{x+1}$. Cover-up: $A = \frac{5(2)+1}{3} = \frac{11}{3}$; $B = \frac{5(-1)+1}{-3} = \frac{-4}{-3} = \frac{4}{3}$. Integral: $\frac{11}{3}\ln|x-2| + \frac{4}{3}\ln|x+1| + C$.

**Key insight:** Cover-up only works for distinct linear factors — keep it for that case alone.

## Example 2: Repeated linear

**Setup:** $\int \frac{4x+3}{(x-1)^2}\,dx$.

**Solution:** $\frac{4x+3}{(x-1)^2} = \frac{A}{x-1} + \frac{B}{(x-1)^2}$. Clear denominators: $4x+3 = A(x-1) + B$. Set $x=1$: $B=7$. Compare $x$-coefficients: $A=4$. Integral: $4\ln|x-1| - \frac{7}{x-1} + C$.

**Key insight:** Setting $x=1$ kills the $A$ term; comparing coefficients gives the rest.

## Example 3: Quadratic factor

**Setup:** $\int \frac{3x+4}{x^2+2x+5}\,dx$.

**Solution:** Complete the square: $x^2+2x+5 = (x+1)^2 + 4$. Write the numerator as $3(x+1) + 1$:
$\int \frac{3(x+1)}{(x+1)^2+4}\,dx + \int \frac{1}{(x+1)^2+4}\,dx = \frac{3}{2}\ln|(x+1)^2+4| + \frac{1}{2}\arctan\frac{x+1}{2} + C$.

**Key insight:** Force the derivative of the quadratic into the numerator; the leftover constant gives the $\arctan$.

**Remember:** If the fraction is improper (numerator degree ≥ denominator degree), divide first, then decompose the remainder.
