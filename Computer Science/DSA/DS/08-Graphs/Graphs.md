A graph is a set of vertices (nodes) connected by edges, formally $G = (V, E)$. It's the most general data structure for modeling relationships — social networks, road maps, dependency chains, and network topology.

**The Intuition:** Think of a graph like a metro system. Stations are vertices, tracks are edges. Some tracks are one-way (directed), some are two-way (undirected). Some have ticket prices (weighted), some don't (unweighted). An adjacency list is like each station having a list of connected stations. An adjacency matrix is like a giant spreadsheet saying which stations connect.

**The Math:**

| Representation | Space | Edge Check |
|----------------|-------|------------|
| Adjacency Matrix | $O(V^2)$ | $O(1)$ |
| Adjacency List | $O(V + E)$ | $O(\deg(v))$ |
| Edge List | $O(E)$ | $O(E)$ |

| Operation | Adjacency List | Adjacency Matrix |
|-----------|---------------|------------------|
| Add vertex | $O(1)$ | $O(V^2)$ |
| Add edge | $O(1)$ | $O(1)$ |
| Remove edge | $O(\deg(v))$ | $O(1)$ |
| Remove vertex | $O(V + E)$ | $O(V^2)$ |

**Types:** Directed vs undirected, weighted vs unweighted, cyclic vs acyclic, connected vs disconnected.

**Key Patterns:** [[08-Graphs-Patterns]], [[08-Graphs-Patterns]], [[08-Graphs-Patterns]]

**Applications:** Social networks, maps / GPS navigation, web crawling, dependency resolution, network routing.

---
**See also:** [[Graph-Algos]], [[Trees]]
