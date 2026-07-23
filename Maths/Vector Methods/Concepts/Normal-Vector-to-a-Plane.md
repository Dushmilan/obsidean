---
date: 2026-07-23
type: concept
tags: [maths, vector-methods, stewart-calculus, chapter-12, 12-5-lines-planes, normal-vector-to-a-plane]
parent: [[Vector Methods/12.5-Equations-of-Lines-and-Planes.md]]
---

# Normal Vector to a Plane

> **Stewart Calculus, Chapter 12, Section 12.5**

## Definition
A normal vector $\mathbf{n} = \langle a, b, c \rangle$ is perpendicular to every vector lying in the plane. Any scalar multiple $k\mathbf{n}$ ($k \neq 0$) defines the same plane. To find $\mathbf{n}$, take the cross product of two non-parallel vectors in the plane:
$$\mathbf{n} = \mathbf{u} \times \mathbf{v}$$

## Key Properties
- $\mathbf{n} \cdot \mathbf{u} = 0$ for every vector $\mathbf{u}$ in the plane
- The magnitude $\|\mathbf{n}\|$ scales the equation but does not change the plane
- Parallel planes have proportional normals: $\mathbf{n}_1 = k\mathbf{n}_2$
- The sign of $\mathbf{n}$ determines orientation (two choices, both valid)

## Worked Example
Plane through $A(1,0,0)$, $B(0,1,0)$, $C(0,0,1)$:
$$\overrightarrow{AB} = \langle -1, 1, 0 \rangle, \quad \overrightarrow{AC} = \langle -1, 0, 1 \rangle$$
$$\mathbf{n} = \overrightarrow{AB} \times \overrightarrow{AC} = \begin{vmatrix} \mathbf{i} & \mathbf{j} & \mathbf{k} \\ -1 & 1 & 0 \\ -1 & 0 & 1 \end{vmatrix} = \langle 1, 1, 1 \rangle$$
Plane equation: $x + y + z = 1$.

## Related Concepts
- [[Scalar-Equation-of-a-Plane]]
- [[Cross-Product-Definition]]

---

*Part of [[Vector Methods/12.5-Equations-of-Lines-and-Planes.md|12.5 Equations of Lines and Planes]]*
