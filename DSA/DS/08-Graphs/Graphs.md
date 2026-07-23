---
date: 2026-07-20
type: concept
tags:
  - dsa
  - data-structures
  - graphs
---

# Graphs

## Definition
A set of vertices (nodes) connected by edges. $G = (V, E)$.

## Types
- **Directed vs Undirected** — edges have direction or not
- **Weighted vs Unweighted** — edges have costs or not
- **Cyclic vs Acyclic** — contains cycles or not
- **Connected vs Disconnected** — all vertices reachable or not

## Representations

| Representation | Space | Edge Check |
|----------------|-------|------------|
| Adjacency Matrix | $O(V^2)$ | $O(1)$ |
| Adjacency List | $O(V + E)$ | $O(\deg(v))$ |
| Edge List | $O(E)$ | $O(E)$ |

## Time Complexity

| Operation | Adjacency List | Adjacency Matrix |
|-----------|---------------|------------------|
| Add vertex | $O(1)$ | $O(V^2)$ |
| Add edge | $O(1)$ | $O(1)$ |
| Remove edge | $O(\deg(v))$ | $O(1)$ |
| Remove vertex | $O(V + E)$ | $O(V^2)$ |

## Key Patterns
- [[08-Graphs-Patterns.md#Adjacency List|Adjacency list representation]]
- [[08-Graphs-Patterns.md#Graph Traversal — BFS|BFS]]
- [[08-Graphs-Patterns.md#Graph Traversal — DFS|DFS]]

## Applications
- Social networks
- Maps / GPS navigation
- Web crawling
- Dependency resolution
- Network routing

---

**See also:** [[../../Algo/09-Graph-Algos/Graph-Algos.md|Graph Algorithms]], [[../07-Trees/Trees.md|Trees]]
