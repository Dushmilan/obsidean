# Graphs & Representations

Graphs model pairwise relationships: networks, dependencies, state spaces, social connections. A graph is just **vertices + edges**, but that simple structure is the most expressive model in discrete math — and the representation you choose (matrix vs list) determines what's fast.

**The Intuition:** A graph is dots and lines. Dots are vertices (nodes), lines are edges (connections). The questions — "is there a path?", "how short?", "is everything connected?" — power navigation, routing, scheduling, and social networks. Which representation you use in code decides whether operations take $O(1)$ or $O(n)$.

## The definitions

```text
Undirected graph G = (V, E):   edges are unordered pairs {u, v}
Directed graph (digraph):      edges are ordered pairs (u, v)
Weighted graph:                each edge has a cost w(u,v)

Order n = |V| (vertex count), size m = |E| (edge count)

Degree deg(v) = number of incident edges
  (in digraphs: in-degree = incoming, out-degree = outgoing)

Adjacent vertices: connected by an edge
Neighbors of v:   the vertices adjacent to v
```

## Special classes

| Class | Definition | Key fact |
|-------|-----------|----------|
| Complete $K_n$ | every pair adjacent | $m = \binom{n}{2} = n(n-1)/2$ |
| Bipartite | vertices split into $V_1, V_2$; edges only across | has no odd cycles |
| Complete bipartite $K_{a,b}$ | all $a·b$ cross-edges | $m = ab$ |
| Tree | connected + acyclic | $m = n - 1$; unique paths |
| Cycle $C_n$ | $n$ vertices in a ring | $m = n$ |
| Simple | no self-loops, no multi-edges | the default assumption |

## The handshaking lemma

> $\sum_{v \in V} \deg(v) = 2|E|$

Every edge contributes 1 to each endpoint's degree — so the degree sum is twice the edge count. **Corollary:** the number of odd-degree vertices is even.

**Setup:** A graph has 7 vertices with degrees 3,3,3,3,2,2,2. Is it possible?
**Solution:** Sum = 18, so $|E| = 9$. Fine — but note: possible *edge-wise*; other constraints (simple graph max degree ≤ 6) must also hold.

## Two representations

| | Adjacency matrix | Adjacency list |
|--|------------------|----------------|
| Memory | $O(V^2)$ always | $O(V + E)$ |
| Edge check $(u,v)$ | $O(1)$ — `M[u][v]` | $O(\deg(v))$ |
| Iterate neighbors of v | $O(V)$ | $O(\deg(v))$ |
| Add edge | $O(1)$ | $O(1)$ |
| Weighted | matrix holds weights | each entry stores weight |
| When | dense graphs ($m \approx V^2$), tiny $V$ | sparse graphs (most real graphs) |

```text
Adjacency list (most common):
  0: [1, 2]
  1: [0, 3]
  2: [0, 3]
  3: [1, 2]
```

## Isomorphism & invariants

Two graphs are **isomorphic** if relabeling vertices makes them identical. To disprove isomorphism, find an **invariant** that differs — number of vertices/edges, degree sequence, number of triangles, connected components.

**Setup:** Are a "square" ($C_4$) and a "diamond" ($K_4$ minus one edge) isomorphic?
**Solution:** $C_4$: all degrees 2. Diamond: degrees 2,2,3,3. Different degree sequences → not isomorphic.

**Key insight:** Invariants are the cheap "these differ" evidence. If every invariant matches, they might be isomorphic (proving it requires a labeling).

---

**Setup:** A company has 20 employees; each knows at least 1 other. Show at least two know the same number of people.

**Solution:** Degrees range 1..19 (20 employees, but degree 0 impossible — everyone knows someone). That's 19 possible degrees for 20 vertices → pigeonhole → two share a degree.

**Key insight:** Handshaking + pigeonhole working together. The degree sequence lives in a smaller box set than the vertex count.

---

**Setup:** Prove that in any group of 6 people, either 3 mutual acquaintances or 3 mutual strangers.

**Solution:** Pick person A. Of the other 5, at least 3 are all friends or all strangers with A (pigeonhole, 2 boxes, 5 items → some box has ≥3). WLOG A knows 3: B, C, D. If any two of B,C,D know each other → triangle with A. If none know each other → B,C,D are 3 mutual strangers. Either way, property holds.

**Key insight:** Ramsey theory — guaranteed structure beyond a size threshold. The proof is case analysis on a pigeonhole split. $R(3,3) = 6$: 6 vertices guarantee a monochromatic triangle in any 2-coloring.

---

**Setup:** Choose the representation for a social network with 10M users and ~20 friends each.

**Solution:** Sparse: $E \approx 10M \times 10 = 10^8$. Adjacency matrix would need $10^{14}$ entries — impossible. Adjacency list: $O(V+E) \approx 2 \times 10^8$ entries — fits. Iterating a user's friends (the hot operation) is $O(20)$.

**Key insight:** Real-world networks are sparse; adjacency lists are the default. The matrix wins only for dense small graphs (or when $O(1)$ edge checks dominate).

---

## Practice (try before peeking)

1. A tree with 10 vertices — how many edges?
2. Sum of degrees in a graph with 7 edges?
3. Can a simple graph have vertices of degree 0 and 6 both?

<details><summary>Answers</summary>

1. $n - 1 = 9$ — tree property.
2. $2 \times 7 = 14$ — handshaking lemma.
3. No (if n ≥ 2) — if one vertex has degree 6 (adjacent to all), no vertex can have degree 0. Handshaking/adjacency contradiction.

</details>

---

**Common traps:**
- Assuming a graph is connected — disconnected graphs have multiple components
- Trees vs graphs: trees are *connected acyclic graphs* — a forest is a disjoint union of trees
- Degree sum must be even — a degree list with odd sum can't be a graph
- Matrix vs list complexity confusion — memory vs operation trade-off
- Multigraph vs simple — parallel edges and loops are usually excluded unless stated

---
