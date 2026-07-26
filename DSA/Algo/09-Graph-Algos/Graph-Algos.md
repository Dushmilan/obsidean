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

**Key Patterns:** [[09-Graph-Algos-Patterns#BFS Shortest Path (Unweighted)|BFS shortest path]], [[09-Graph-Algos-Patterns#DFS — Cycle Detection (Directed)|Cycle detection (DFS)]], [[09-Graph-Algos-Patterns#Dijkstra's Algorithm|Dijkstra's algorithm]], [[09-Graph-Algos-Patterns#Topological Sort (Kahn's)|Topological sort]], [[09-Graph-Algos-Patterns#Detect Cycle — Union-Find (DSU)|Union-Find / DSU]], [[09-Graph-Algos-Patterns#Kruskal's MST|Kruskal's MST]], [[09-Graph-Algos-Patterns#Prim's MST|Prim's MST]]

---
**See also:** [[../../DS/08-Graphs/Graphs.md|Graphs (DS)]], [[../../DS/07-Trees/Trees.md|Trees]]
