---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-13, 13-4-motion, speed]
parent: [[Vector Methods/13.4-Motion-in-Space.md]]
---

# Speed

> **Stewart Calculus, Chapter 13, Section 13.4**

## Definition
Speed is the scalar magnitude of the velocity vector:

$$v(t) = \|\mathbf{v}(t)\| = \|\mathbf{r}'(t)\| = \frac{ds}{dt}$$

It represents the rate of change of arc length with respect to time.

## Key Properties
- Always non-negative: $v(t) \geq 0$
- Equals $\frac{ds}{dt}$, the rate at which arc length is traversed
- Zero speed means the object is momentarily at rest
- Speed is a scalar; velocity is a vector

## Worked Example
Given $\mathbf{v}(t) = \langle 2t, 3t^2, 1 \rangle$, find the speed.

$$v(t) = \|\mathbf{v}(t)\| = \sqrt{(2t)^2 + (3t^2)^2 + 1^2} = \sqrt{4t^2 + 9t^4 + 1}$$

At $t = 0$, speed $= \sqrt{1} = 1$.

## Related Concepts
- [[Velocity-Vector]]
- [[Arc-Length-Function]]

---

*Part of [[Vector Methods/13.4-Motion-in-Space.md|13.4 Motion in Space]]*
