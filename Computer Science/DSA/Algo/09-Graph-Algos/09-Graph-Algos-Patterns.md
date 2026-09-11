---
date: 2026-07-20
type: code-patterns
tags:
  - dsa
  - graph-algorithms
  - python
  - java
---

# Graph Algorithms — Patterns

## BFS Shortest Path (Unweighted)

**Python**
```python
from collections import deque

def bfs_shortest_path(graph, start, target):
    q = deque([(start, 0)])
    visited = {start}
    while q:
        node, dist = q.popleft()
        if node == target:
            return dist
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                q.append((neighbor, dist + 1))
    return -1
```

**Java**
```java
import java.util.*;

public class BFSShortestPath {
    public static int bfsShortestPath(Map<Integer, List<Integer>> graph, int start, int target) {
        Queue<int[]> q = new LinkedList<>();
        Set<Integer> visited = new HashSet<>();
        q.offer(new int[]{start, 0});
        visited.add(start);
        while (!q.isEmpty()) {
            int[] curr = q.poll();
            int node = curr[0], dist = curr[1];
            if (node == target) return dist;
            for (int neighbor : graph.getOrDefault(node, new ArrayList<>())) {
                if (!visited.contains(neighbor)) {
                    visited.add(neighbor);
                    q.offer(new int[]{neighbor, dist + 1});
                }
            }
        }
        return -1;
    }

    public static void main(String[] args) {
        Map<Integer, List<Integer>> graph = new HashMap<>();
        graph.put(0, List.of(1, 2));
        graph.put(1, List.of(0, 3));
        graph.put(2, List.of(0, 3));
        graph.put(3, List.of(1, 2));
        System.out.println(bfsShortestPath(graph, 0, 3)); // 2
    }
}
```

## DFS — Cycle Detection (Directed)

**Python**
```python
WHITE, GRAY, BLACK = 0, 1, 2

def has_cycle(graph):
    color = {node: WHITE for node in graph}

    def dfs(node):
        if color[node] == GRAY:
            return True
        if color[node] == BLACK:
            return False
        color[node] = GRAY
        for neighbor in graph.get(node, []):
            if dfs(neighbor):
                return True
        color[node] = BLACK
        return False

    for node in graph:
        if color[node] == WHITE:
            if dfs(node):
                return True
    return False
```

**Java**
```java
import java.util.*;

public class CycleDetection {
    public static boolean hasCycle(Map<Integer, List<Integer>> graph) {
        Map<Integer, Integer> color = new HashMap<>();
        for (int node : graph.keySet()) color.put(node, 0); // WHITE

        for (int node : graph.keySet()) {
            if (color.get(node) == 0 && dfs(graph, node, color)) {
                return true;
            }
        }
        return false;
    }

    private static boolean dfs(Map<Integer, List<Integer>> graph, int node, Map<Integer, Integer> color) {
        color.put(node, 1); // GRAY
        for (int neighbor : graph.getOrDefault(node, new ArrayList<>())) {
            if (color.getOrDefault(neighbor, 0) == 1) return true;
            if (color.getOrDefault(neighbor, 0) == 0 && dfs(graph, neighbor, color)) return true;
        }
        color.put(node, 2); // BLACK
        return false;
    }

    public static void main(String[] args) {
        Map<Integer, List<Integer>> graph = new HashMap<>();
        graph.put(0, List.of(1));
        graph.put(1, List.of(2));
        graph.put(2, List.of(0));
        System.out.println(hasCycle(graph)); // true
    }
}
```

## Dijkstra's Algorithm

**Python**
```python
import heapq

def dijkstra(graph, start):
    dist = {node: float('inf') for node in graph}
    dist[start] = 0
    pq = [(0, start)]
    while pq:
        d, node = heapq.heappop(pq)
        if d > dist[node]:
            continue
        for neighbor, weight in graph[node]:
            nd = d + weight
            if nd < dist[neighbor]:
                dist[neighbor] = nd
                heapq.heappush(pq, (nd, neighbor))
    return dist
```

**Java**
```java
import java.util.*;

public class Dijkstra {
    public static Map<Integer, Integer> dijkstra(Map<Integer, List<int[]>> graph, int start) {
        Map<Integer, Integer> dist = new HashMap<>();
        for (int node : graph.keySet()) dist.put(node, Integer.MAX_VALUE);
        dist.put(start, 0);
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[1] - b[1]);
        pq.offer(new int[]{start, 0});
        while (!pq.isEmpty()) {
            int[] curr = pq.poll();
            int node = curr[0], d = curr[1];
            if (d > dist.get(node)) continue;
            for (int[] edge : graph.getOrDefault(node, new ArrayList<>())) {
                int neighbor = edge[0], weight = edge[1];
                int nd = d + weight;
                if (nd < dist.get(neighbor)) {
                    dist.put(neighbor, nd);
                    pq.offer(new int[]{neighbor, nd});
                }
            }
        }
        return dist;
    }

    public static void main(String[] args) {
        Map<Integer, List<int[]>> graph = new HashMap<>();
        graph.put(0, List.of(new int[]{1, 4}, new int[]{2, 1}));
        graph.put(1, List.of(new int[]{3, 1}));
        graph.put(2, List.of(new int[]{1, 2}, new int[]{3, 5}));
        graph.put(3, new ArrayList<>());
        System.out.println(dijkstra(graph, 0)); // {0=0, 1=3, 2=1, 3=4}
    }
}
```

## Topological Sort (Kahn's Algorithm)

**Python**
```python
from collections import deque

def topological_sort(graph):
    in_degree = {node: 0 for node in graph}
    for node in graph:
        for neighbor in graph[node]:
            in_degree[neighbor] = in_degree.get(neighbor, 0) + 1

    q = deque([node for node, deg in in_degree.items() if deg == 0])
    result = []

    while q:
        node = q.popleft()
        result.append(node)
        for neighbor in graph.get(node, []):
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                q.append(neighbor)

    return result if len(result) == len(graph) else []
```

**Java**
```java
import java.util.*;

public class TopologicalSort {
    public static List<Integer> topologicalSort(Map<Integer, List<Integer>> graph) {
        Map<Integer, Integer> inDegree = new HashMap<>();
        for (int node : graph.keySet()) inDegree.put(node, 0);
        for (int node : graph.keySet()) {
            for (int neighbor : graph.get(node)) {
                inDegree.put(neighbor, inDegree.getOrDefault(neighbor, 0) + 1);
            }
        }

        Queue<Integer> q = new LinkedList<>();
        for (Map.Entry<Integer, Integer> entry : inDegree.entrySet()) {
            if (entry.getValue() == 0) q.offer(entry.getKey());
        }

        List<Integer> result = new ArrayList<>();
        while (!q.isEmpty()) {
            int node = q.poll();
            result.add(node);
            for (int neighbor : graph.getOrDefault(node, new ArrayList<>())) {
                inDegree.put(neighbor, inDegree.get(neighbor) - 1);
                if (inDegree.get(neighbor) == 0) q.offer(neighbor);
            }
        }
        return result.size() == graph.size() ? result : new ArrayList<>();
    }

    public static void main(String[] args) {
        Map<Integer, List<Integer>> graph = new HashMap<>();
        graph.put(0, List.of(1));
        graph.put(1, new ArrayList<>());
        graph.put(2, List.of(0));
        System.out.println(topologicalSort(graph));
    }
}
```

## Detect Cycle — Union-Find (DSU)

**Python**
```python
class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):
        px, py = self.find(x), self.find(y)
        if px == py:
            return False  # cycle detected
        if self.rank[px] < self.rank[py]:
            self.parent[px] = py
        elif self.rank[px] > self.rank[py]:
            self.parent[py] = px
        else:
            self.parent[py] = px
            self.rank[px] += 1
        return True
```

**Java**
```java
public class DSU {
    private int[] parent, rank;

    public DSU(int n) {
        parent = new int[n];
        rank = new int[n];
        for (int i = 0; i < n; i++) parent[i] = i;
    }

    public int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);
        }
        return parent[x];
    }

    public boolean union(int x, int y) {
        int px = find(x), py = find(y);
        if (px == py) return false;
        if (rank[px] < rank[py]) parent[px] = py;
        else if (rank[px] > rank[py]) parent[py] = px;
        else { parent[py] = px; rank[px]++; }
        return true;
    }

    public static void main(String[] args) {
        DSU dsu = new DSU(5);
        dsu.union(0, 1);
        dsu.union(2, 3);
        System.out.println(dsu.find(0) == dsu.find(1)); // true
        System.out.println(dsu.find(0) == dsu.find(2)); // false
    }
}
```

## Kruskal's MST

**Python**
```python
def kruskal_mst(edges, n):
    edges.sort(key=lambda x: x[2])
    dsu = DSU(n)
    mst_weight = 0
    mst_edges = []

    for u, v, w in edges:
        if dsu.union(u, v):
            mst_weight += w
            mst_edges.append((u, v, w))

    return mst_weight, mst_edges
```

**Java**
```java
import java.util.*;

public class KruskalMST {
    public static int kruskalMST(int[][] edges, int n) {
        Arrays.sort(edges, (a, b) -> a[2] - b[2]);
        DSU dsu = new DSU(n);
        int weight = 0;
        for (int[] edge : edges) {
            if (dsu.union(edge[0], edge[1])) {
                weight += edge[2];
            }
        }
        return weight;
    }

    public static void main(String[] args) {
        int[][] edges = {{0, 1, 10}, {0, 2, 6}, {0, 3, 5}, {1, 3, 15}, {2, 3, 4}};
        System.out.println(kruskalMST(edges, 4)); // 19
    }
}
```

## Prim's MST

**Python**
```python
import heapq

def prim_mst(graph, start=0):
    visited = set()
    pq = [(0, start)]
    mst_weight = 0
    while pq:
        w, node = heapq.heappop(pq)
        if node in visited:
            continue
        visited.add(node)
        mst_weight += w
        for neighbor, nw in graph[node]:
            if neighbor not in visited:
                heapq.heappush(pq, (nw, neighbor))
    return mst_weight
```

**Java**
```java
import java.util.*;

public class PrimMST {
    public static int primMST(Map<Integer, List<int[]>> graph) {
        Set<Integer> visited = new HashSet<>();
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[1] - b[1]);
        pq.offer(new int[]{0, 0});
        int weight = 0;
        while (!pq.isEmpty()) {
            int[] curr = pq.poll();
            int node = curr[0], w = curr[1];
            if (visited.contains(node)) continue;
            visited.add(node);
            weight += w;
            for (int[] edge : graph.getOrDefault(node, new ArrayList<>())) {
                if (!visited.contains(edge[0])) {
                    pq.offer(new int[]{edge[0], edge[1]});
                }
            }
        }
        return weight;
    }

    public static void main(String[] args) {
        Map<Integer, List<int[]>> graph = new HashMap<>();
        graph.put(0, List.of(new int[]{1, 10}, new int[]{2, 6}, new int[]{3, 5}));
        graph.put(1, List.of(new int[]{0, 10}, new int[]{3, 15}));
        graph.put(2, List.of(new int[]{0, 6}, new int[]{3, 4}));
        graph.put(3, List.of(new int[]{0, 5}, new int[]{1, 15}, new int[]{2, 4}));
        System.out.println(primMST(graph)); // 19
    }
}
```
