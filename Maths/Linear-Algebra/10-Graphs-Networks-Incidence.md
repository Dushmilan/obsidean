---
date: 2026-07-21
type: linear-algebra-cluster
tags: [linear-algebra, strang]
lectures: [12]
prereq_clusters: ["08"]
status: complete
source: manual
---

# 10 — Graphs, Networks, and Incidence Matrices

## Concept Statement
Translate the abstract four-subspace blueprint into a physical network (electric circuit, fluid graph). The incidence matrix $A$ encodes the topology; the subspaces regain physical meaning as Kirchhoff-style laws.

## Lecture Sources
- Strang MIT 18.06, Lecture 12: *Graphs, Networks, and Incidence Matrices*

## Core Material

### Directed Graphs
A graph $G = (V, E)$:
- **Nodes** (vertices, $n$ of them).
- **Edges** (directed arrows, $m$ of them).

### The Incidence Matrix $A$
Dimensions: $m \times n$ ($m$ edges, $n$ nodes).

Row $k$ of $A$:
- $-1$ at the *departing* node of edge $k$.
- $+1$ at the *arriving* node of edge $k$.
- $0$ everywhere else.

### Meanings of Multiplications

**$A\mathbf{x}$ — potential differences.**
If $\mathbf{x}$ stores potentials (voltages, pressures) at each node, $A\mathbf{x}$ gives the potential difference across each edge.

**$N(A)$ — equalised potentials.**
$A\mathbf{x} = \mathbf{0}$ means zero drop across every edge. So all nodes share the same potential $\mathbf{x} = [c, c, c, \dots]^T$. For a fully connected graph, $\dim N(A) = 1$.

**$N(A^T)$ — Kirchhoff's Current Law.**
$A^T \mathbf{y} = \mathbf{0}$ means the net current entering every node is zero. The basis vectors of $N(A^T)$ correspond to *independent closed loops* in the graph.

### Physical Instantiation of the Four Subspaces

| Subspace | Physical meaning |
|----------|------------------|
| $C(A^T)$ | relationship between potentials and currents along edges |
| $N(A)$ | equilibrium potential configurations (all-equal) |
| $C(A)$ | edge-state vectors reachable from some assignment |
| $N(A^T)$ | current distributions satisfying KCL at every node |

## Cross-Cluster Links
- **Prereq**: [[08-Four-Fundamental-Subspaces]]
- **Forward**: [[11-Quiz-1-Synthesis]] (synthesis), [[12-Orthogonal-Vectors-Subspaces]] (orthogonal complement between $C(A^T)$ and $N(A)$)

## Thematic Summary
The incidence matrix is the canonical example where abstract linear algebra acquires physical traction. Kirchhoff's laws read out directly as nullspace statements: currents sum to zero at every node (left nullspace), potentials are flat when no current flows (nullspace). Mathematicians and electrical engineers meet here.

## Glossary

| Term | Definition |
|------|------------|
| **Directed Graph** | Nodes connected by arrows indicating direction of flow. |
| **Incidence Matrix** | An $m \times n$ matrix where row $k$ encodes the source/sink of edge $k$ with $-1$/$+1$. |
| **Kirchhoff's Current Law** | Net current into a node is zero; $\mathbf{y}^T A = 0$ at each node. |
| **Closed Loop** | A cycle in the graph; basis element of $N(A^T)$. |
