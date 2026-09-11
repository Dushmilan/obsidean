Graph algorithms operate on structures of vertices and edges — traversing, finding shortest paths, detecting cycles, and computing connectivity. They're essential for networks, maps, social graphs, and dependency resolution.

**The Intuition:** Think of a graph like a city map. BFS is like flooding the city from a starting point — you visit all intersections at distance 1 first, then distance 2, and so on (guaranteeing shortest path in unweighted graphs). DFS is like exploring one street all the way to the end before backtracking. Dijkstra adds road distances and always expands the closest unvisited intersection.

**The Math:**

| Algorithm | Use Case | Time Complexity |
|-----------|----------|-----------------|
| BFS | Shortest path (unweighted), level order | $O(V + E)$ |
| DFS | Connectivity, topological sort, cycles | $O(V + E)$ |
| Dijkstra | Shortest path (weighted, non-negative) | $O((V+E) \log V)$ |
| Bellman-Ford | Shortest path (negative weights) | $O(VE)$ |
| Floyd-Warshall | All-pairs shortest path | $O(V^3)$ |
| Kruskal's | Minimum spanning tree | $O(E \log V)$ |
| Prim's | Minimum spanning tree | $O(E \log V)$ |
| Topological Sort | DAG ordering | $O(V + E)$ |

**Key Patterns:** [[09-Graph-Algos-Patterns]], [[09-Graph-Algos-Patterns]], [[09-Graph-Algos-Patterns]], [[09-Graph-Algos-Patterns]], [[09-Graph-Algos-Patterns]], [[09-Graph-Algos-Patterns]], [[09-Graph-Algos-Patterns]]

---
**See also:** [[Graphs]], [[Trees]]
