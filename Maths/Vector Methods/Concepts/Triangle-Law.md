---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-2-vectors, geometric-addition]
parent: [[Vector Methods/12.2-Vectors.md]]
---

# Triangle Law

> **Stewart Calculus, Chapter 12, Section 12.2**

## Definition
The **triangle law** is a geometric method for adding two vectors:
1. Place the **tail** of the second vector at the **tip** of the first
2. The **resultant** (sum) is the vector from the tail of the first to the tip of the second

$$\mathbf{u} + \mathbf{v} = \text{closing side of the triangle}$$

## Key Properties
- The three vectors $\mathbf{u}$, $\mathbf{v}$, and $\mathbf{u} + \mathbf{v}$ form a triangle (possibly degenerate)
- Order does not matter: $\mathbf{u} + \mathbf{v} = \mathbf{v} + \mathbf{u}$ (commutativity)
- Extends naturally to three or more vectors by chaining tip-to-tail

## Worked Example
Given $\mathbf{u} = \langle 2, 0 \rangle$ and $\mathbf{v} = \langle 1, 3 \rangle$:
- Draw $\mathbf{u}$ from origin to $(2, 0)$
- Place tail of $\mathbf{v}$ at $(2, 0)$; tip lands at $(3, 3)$
- Resultant: $\mathbf{u} + \mathbf{v} = \langle 3, 3 \rangle$ (from origin to $(3,3)$)

## Related Concepts
- [[Vector-Addition]]
- [[Parallelogram-Law]]
- [[Vector-Definition]]

---

*Part of [[Vector Methods/12.2-Vectors.md|12.2 Vectors]]*
