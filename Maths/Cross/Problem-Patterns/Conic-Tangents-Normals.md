---
date: 2026-08-16
type: problem-pattern
tags: [maths, pure, patterns, geometry]
parent: [[02-Analytical-Geometry_Index]]
---

# Conic Sections: Tangents & Normals — Problem Patterns

Every conic has a "point form" tangent: replace $x^2 \to xx_1$, $y^2 \to yy_1$, $x \to \frac{x+x_1}{2}$, $y \to \frac{y+y_1}{2}$, and keep constants unchanged. If you learn those replacements, tangents stop being memorization.

## Pattern 1: Parabola $y^2 = 4ax$

- **Tangent at $(x_1,y_1)$:** $yy_1 = 2a(x + x_1)$
- **Tangent with slope $m$:** $y = mx + \frac{a}{m}$ (any $m \neq 0$)
- **Normal at $(x_1,y_1)$:** $y - y_1 = -\frac{y_1}{2a}(x - x_1)$

**Example:** Find the equation of the tangent to $y^2 = 8x$ at $(2,4)$.

**Setup:** Point-form tangent.

**Solution:** $a=2$. Point form: $4y = 4(x+2)$, i.e. $y = x + 2$.

**Key insight:** The slope form is the fast route when the problem gives — or asks for — the slope.

## Pattern 2: Ellipse $\frac{x^2}{a^2} + \frac{y^2}{b^2} = 1$

- **Tangent:** $\frac{xx_1}{a^2} + \frac{yy_1}{b^2} = 1$
- **Normal:** $\frac{a^2x}{x_1} - \frac{b^2y}{y_1} = a^2 - b^2$ (if $x_1y_1 \neq 0$)

## Pattern 3: Hyperbola $\frac{x^2}{a^2} - \frac{y^2}{b^2} = 1$

- **Tangent:** $\frac{xx_1}{a^2} - \frac{yy_1}{b^2} = 1$
- **Normal:** $\frac{a^2x}{x_1} + \frac{b^2y}{y_1} = a^2 + b^2$

**Example:** Find the tangent to $\frac{x^2}{9} - \frac{y^2}{16} = 1$ at $\left(6, 4\sqrt{3}\right)$.

**Setup:** Hyperbola tangent.

**Solution:** $\frac{6x}{9} - \frac{4\sqrt{3}y}{16} = 1 \Rightarrow \frac{2x}{3} - \frac{\sqrt{3}y}{4} = 1$.

**Key insight:** The same replacement rule works for ellipse and hyperbola — only the sign differs.

## Pattern 4: Tangents from an external point

**The move:** Write the tangent in slope form (parabola) or use the **chord of contact**: for an external point $P(x_1,y_1)$, the chord of contact is given by the same point-form replacement evaluated at $P$.

**Example:** Find the chord of contact of tangents from $(5,3)$ to $y^2 = 4x$.

**Setup:** Chord of contact.

**Solution:** Point form with $a=1$, $(x_1,y_1)=(5,3)$: $3y = 2(x+5)$, i.e. $2x - 3y + 10 = 0$.

**Key insight:** The chord of contact is the point-form tangent formula evaluated at the *external* point — the two tangent points both lie on this line.

**When the slope is unknown:** substitute $y = mx + c$ into the conic and set the discriminant to zero. That's the general tangency condition.
