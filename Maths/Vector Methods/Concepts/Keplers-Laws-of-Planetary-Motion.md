---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-13, 13-4-motion, keplers-laws]
parent: [[Vector Methods/13.4-Motion-in-Space.md]]
---

# Kepler's Laws of Planetary Motion

> **Stewart Calculus, Chapter 13, Section 13.4**

## Definition
Three empirical laws describing planetary orbits, derived from Newton's law of gravitation:

**First Law (Ellipses):** Planets orbit the Sun in ellipses with the Sun at one focus.

**Second Law (Equal Areas):** A line from the Sun to a planet sweeps equal areas in equal times. The areal velocity is constant:

$$\frac{dA}{dt} = \frac{1}{2}\|\mathbf{r} \times \mathbf{v}\| = \text{const}$$

**Third Law (Harmonic Law):** The square of the orbital period is proportional to the cube of the semi-major axis:

$$T^2 \propto a^3$$

## Key Properties
- Derived from $\mathbf{F} = -\frac{GMm}{r^3}\mathbf{r}$ (inverse-square gravitational force)
- Second law implies planets move faster when closer to the Sun (conservation of angular momentum)
- Third law applies to all objects orbiting the same central body
- Equal areas in equal times is a consequence of torque-free motion

## Worked Example
Earth's orbit has eccentricity $e \approx 0.017$, nearly circular. By the second law, Earth's orbital speed is nearly constant at $\approx 29.8$ km/s. The slight eccentricity means it moves slightly faster at perihelion (closest approach, early January) and slower at aphelion (farthest, early July).

By the third law, if $T_E = 1$ year and $a_E = 1$ AU, then for Mars ($a_M \approx 1.524$ AU):

$$T_M = T_E \left(\frac{a_M}{a_E}\right)^{3/2} = (1.524)^{3/2} \approx 1.88 \text{ years}$$

## Related Concepts
- [[Newtons-Second-Law-in-Vector-Form]]
- [[Acceleration-Vector]]

---

*Part of [[Vector Methods/13.4-Motion-in-Space.md|13.4 Motion in Space]]*
