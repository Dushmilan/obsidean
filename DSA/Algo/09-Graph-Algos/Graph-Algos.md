---
date: 2026-07-20
type: concept
tags:
  - dsa
  - algorithms
  - graphs
---

# Graph Algorithms

## Definition
Algorithms that operate on graph structures — traversing, finding paths, detecting cycles, and computing connectivity.

## Key Algorithms

| Algorithm | Use Case | Time Complexity |
|-----------|----------|-----------------|
| BFS | Shortest path (unweighted), level order | $O(V + E)$ |
| DFS | Connectivity, topological sort, cycles | $O(V + E)$ |
| Dijkstra | Shortest path (weighted, non-negative) | $O((V+E) \log V)$ |
| Bellman-Ford | Shortest path (negative weights allowed) | $O(VE)$ |
| Floyd-Warshall | All-pairs shortest path | $O(V^3)$ |
| Kruskal's | Minimum spanning tree | $O(E \log V)$ |
| Prim's | Minimum spanning tree | $O(E \log V)$ |
| Topological Sort | DAG ordering | $O(V + E)$ |

## Key Patterns
- [[09-Graph-Algos-Patterns.md#BFS Shortest Path (Unweighted)|BFS shortest path]]
- [[09-Graph-Algos-Patterns.md#DFS — Cycle Detection (Directed)|Cycle detection (DFS)]]
- [[09-Graph-Algos-Patterns.md#Dijkstra's Algorithm|Dijkstra's algorithm]]
- [[09-Graph-Algos-Patterns.md#Topological Sort (Kahn's)|Topological sort]]
- [[09-Graph-Algos-Patterns.md#Detect Cycle — Union-Find (DSU)|Union-Find / DSU]]
- [[09-Graph-Algos-Patterns.md#Kruskal's MST|Kruskal's MST]]
- [[09-Graph-Algos-Patterns.md#Prim's MST|Prim's MST]]

---

**See also:** [[../../DS/08-Graphs/Graphs.md|Graphs (DS)]], [[../../DS/07-Trees/Trees.md|Trees]]
