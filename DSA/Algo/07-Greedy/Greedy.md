---
date: 2026-07-20
type: concept
tags:
  - dsa
  - algorithms
  - greedy
---

# Greedy

## Definition
A strategy that makes the locally optimal choice at each step, hoping it leads to a globally optimal solution.

## When Greedy Works
- **Optimal substructure** — optimal solution contains optimal solutions to subproblems
- **Greedy choice property** — a globally optimal solution can be arrived at by making a locally optimal choice

## Time Complexity
Varies by problem, typically $O(n \log n)$ (due to sorting) or $O(n)$.

## Key Patterns
- [[07-Greedy-Patterns.md#Activity Selection|Activity selection]]
- [[07-Greedy-Patterns.md#Coin Change (Greedy)|Coin change (greedy)]]
- [[07-Greedy-Patterns.md#Fractional KnapSack|Fractional knapsack]]
- [[07-Greedy-Patterns.md#Jump Game|Jump game]]
- [[07-Greedy-Patterns.md#Minimum Platforms|Minimum platforms]]
- [[07-Greedy-Patterns.md#Huffman Coding|Huffman coding]]

## Applications
- Scheduling (job sequencing, activity selection)
- Compression (Huffman coding)
- Minimum spanning tree (Prim's, Kruskal's)
- Shortest path (Dijkstra's)

---

**See also:** [[../08-Dynamic-Programming/Dynamic-Programming.md|Dynamic Programming]], [[../09-Graph-Algos/Graph-Algos.md|Graph Algorithms]]
