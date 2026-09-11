# Trees & Graph Traversals

Trees are the simplest connected structures — and the most useful: file systems, XML/JSON, expression trees, spanning trees, decision trees. Traversals (BFS/DFS) are the algorithms that explore graphs, powering shortest paths, connectivity, and search in AI (CS 188).

**The Intuition:** A tree is a graph with no cycles — one way in, one way out between any two nodes. That single property makes everything simpler: no visited-set needed for some algorithms, unique paths, and $m = n - 1$. Traversals are the "visit every node exactly once" recipes — BFS explores in expanding rings (like ripples), DFS dives deep before backtracking.

## Trees — the definitions

```text
Tree:  connected + acyclic
Forest: acyclic (disjoint union of trees)

Properties (any two imply the third for connected graphs):
  - connected
  - acyclic
  - m = n - 1

Every tree with n ≥ 2 has at least 2 leaves (degree-1 nodes).

Rooted tree: pick a root → parent/child/ancestor/descendant,
             depth (distance from root), height (max depth), level, subtree.

m-ary tree: each node has ≤ m children
Binary tree: m = 2; full if every node has 0 or 2 children
```

## Proof that a tree has n−1 edges (induction)

```text
Base: single vertex, 0 edges = 1 - 1 ✓
Step: a tree on n vertices has a leaf (degree 1). Remove it —
      still a tree on n-1 vertices → n-2 edges by IH.
      The removed leaf contributed 1 edge → total (n-2) + 1 = n-1 ✓
```

## Spanning trees

A **spanning tree** of a connected graph: a subgraph that's a tree touching every vertex. Every connected graph has one (BFS/DFS tree). The **minimum spanning tree** minimizes total edge weight (Kruskal/Prim — see DSA Graph Algorithms).

## BFS vs DFS

| | BFS | DFS |
|--|-----|-----|
| Data structure | Queue | Stack (or recursion) |
| Order | Level by level (expanding rings) | Deep dive, then backtrack |
| Paths found | **Shortest paths** (unweighted) | Any path |
| Components | Works for connected components | Works too |
| Space | $O(\text{width})$ — can blow up | $O(\text{height})$ — usually smaller |
| Typical uses | Shortest path, bipartite check, level order | Topological sort, cycle detection, maze solving, SCCs |

## BFS — the pattern

```text
BFS(start):
  queue ← {start}; visited ← {start}
  while queue not empty:
    v ← dequeue
    for each neighbor u of v:
      if u not visited:
        mark u visited
        record dist[u] = dist[v] + 1, parent[u] = v
        enqueue u
```

**Key facts:**
- First time a vertex is *discovered* = shortest path distance (in an unweighted graph)
- `parent[]` array reconstructs the path by backtracking
- Complexity $O(V + E)$ with adjacency lists

## DFS — the pattern

```text
DFS(v):
  mark v visited
  for each neighbor u of v:
    if u not visited: DFS(u)
```
Recursion uses the call stack implicitly; iterative version uses an explicit stack.

**Cycle detection (undirected):** during DFS, an edge to an *already visited* non-parent vertex = a cycle.
**Topological sort (DAG):** DFS post-order, reversed.

---

**Setup:** BFS on a grid (maze) — find the shortest path.

**Solution:** Treat each cell as a vertex; neighbors are the 4 adjacent cells. BFS with a `dist` array gives the shortest number of moves. The path is reconstructed from `parent`.

**Key insight:** Grids are graphs in disguise — this is the "shortest path in a maze" problem from DSA, and BFS is the canonical answer for unweighted moves. Same pattern powers flood fill and spreading infection simulations.

---

**Setup:** Prove: a connected graph with $n$ vertices and $n-1$ edges is a tree.

**Solution:** Since it's connected, it has a spanning tree — $n - 1$ edges. But the graph itself has $n-1$ edges, so the spanning tree uses *all* of them — the graph IS its spanning tree, hence acyclic. Connected + acyclic = tree.

**Key insight:** The "spanning tree has exactly $n-1$ edges" fact does the proof — connected graph, edge count minimal ⇒ tree.

---

**Setup:** Count the number of binary trees with 3 nodes.

**Solution:** Let root split into left (L nodes) and right (R nodes), L+R = 2:
```text
L=2,R=0: 2 trees (two shapes of left subtree) × 1 = 2
L=1,R=1: 1 × 1 = 1
L=0,R=2: 2
Total = 2 + 1 + 2 = 5
```
The sequence $1, 1, 2, 5, 14, \dots$ is the **Catalan numbers** $C_n = \frac{1}{n+1}\binom{2n}{n}$.

**Key insight:** Recursive structure (subtrees) + the multiplication principle = Catalan counts. Same numbers count balanced parentheses, triangulations, and stack permutations — the same decomposition recurs everywhere.

---

**Setup:** Detect whether an undirected graph is bipartite.

**Solution:** BFS coloring: color start 0; each neighbor gets the opposite color; if a neighbor is already colored the *same*, the graph is not bipartite (odd cycle found).

**Key insight:** Bipartite ⟺ no odd cycles. BFS coloring both decides and produces the partition — it's how scheduling problems (tasks vs resources) get verified.

---

## Practice (try before peeking)

1. A forest of 3 trees has 20 vertices — how many edges?
2. BFS or DFS for shortest path on a weighted graph?
3. A full binary tree of height 3 (root = height 0) — how many nodes max?

<details><summary>Answers</summary>

1. Each tree with $n_i$ vertices has $n_i - 1$ edges → total $20 - 3 = 17$.
2. Neither alone — weighted shortest paths need Dijkstra (or Bellman-Ford for negatives). BFS is for unweighted.
3. $2^{4} - 1 = 15$ — levels 0..3, each level doubling.

</details>

---

**Common traps:**
- BFS doesn't give shortest paths in *weighted* graphs — that's Dijkstra
- DFS recursion depth — deep trees/ graphs overflow the call stack (use explicit stack)
- Confusing pre-order/in-order/post-order with BFS "level order"
- A graph can have multiple spanning trees; the MST is the minimum one
- Trees are acyclic — but a graph with a self-loop or parallel edges is a multigraph, different rules

---
