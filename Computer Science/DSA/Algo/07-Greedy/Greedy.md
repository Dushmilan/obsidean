Greedy algorithms make the locally optimal choice at each step, hoping it leads to a globally optimal solution. They work when the problem has optimal substructure and the greedy choice property — meaning you can commit to a local choice without reconsidering.

**The Intuition:** Imagine you're picking up coins from a table. A greedy approach is to always grab the closest coin to your hand. If the coins are arranged so that each closest coin is also the best overall choice (like in fractional knapsack), you end up with the optimal collection. But if the coin layout is more complex (like 0/1 knapsack), greedy fails and you need dynamic programming.

**The Math:**

- **Time:** Typically $O(n \log n)$ (due to sorting) or $O(n)$
- **When it works:**
  - **Optimal substructure** — optimal solution contains optimal sub-solutions
  - **Greedy choice property** — globally optimal solution reachable via locally optimal choices

**Key Patterns:** [[07-Greedy-Patterns]], [[07-Greedy-Patterns]], [[07-Greedy-Patterns]], [[07-Greedy-Patterns]], [[07-Greedy-Patterns]], [[07-Greedy-Patterns]]

**Applications:** Scheduling (job sequencing, activity selection), compression (Huffman coding), minimum spanning tree (Prim's, Kruskal's), shortest path (Dijkstra's).

---
**See also:** [[Dynamic-Programming]], [[Graph-Algos]]
