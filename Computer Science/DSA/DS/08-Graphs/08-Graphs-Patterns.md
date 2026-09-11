---
date: 2026-07-20
type: code-patterns
tags:
  - dsa
  - graphs
  - python
  - java
---

# Graphs — Patterns

## Adjacency List

**Python**
```python
from collections import defaultdict

# Build adjacency list
def build_graph(edges: list[tuple[int, int]], directed=False):
    graph = defaultdict(list)
    for u, v in edges:
        graph[u].append(v)
        if not directed:
            graph[v].append(u)
    return graph

# Example: [[0,1], [1,2], [2,0]]
# {0: [1, 2], 1: [0, 2], 2: [1, 0]}
```

**Java**
```java
import java.util.*;

public class AdjacencyList {
    public static Map<Integer, List<Integer>> buildGraph(int[][] edges, boolean directed) {
        Map<Integer, List<Integer>> graph = new HashMap<>();
        for (int[] edge : edges) {
            int u = edge[0], v = edge[1];
            graph.computeIfAbsent(u, k -> new ArrayList<>()).add(v);
            if (!directed) {
                graph.computeIfAbsent(v, k -> new ArrayList<>()).add(u);
            }
        }
        return graph;
    }

    public static void main(String[] args) {
        int[][] edges = {{0,1}, {1,2}, {2,0}};
        Map<Integer, List<Integer>> graph = buildGraph(edges, false);
        System.out.println(graph); // {0=[1, 2], 1=[0, 2], 2=[1, 0]}
    }
}
```

## Graph Traversal — BFS

**Python**
```python
from collections import deque

def bfs(graph, start):
    visited = set()
    q = deque([start])
    visited.add(start)
    result = []
    while q:
        node = q.popleft()
        result.append(node)
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                q.append(neighbor)
    return result
```

**Java**
```java
import java.util.*;

public class BFS {
    public static List<Integer> bfs(Map<Integer, List<Integer>> graph, int start) {
        Set<Integer> visited = new HashSet<>();
        Queue<Integer> q = new LinkedList<>();
        List<Integer> result = new ArrayList<>();
        q.offer(start);
        visited.add(start);
        while (!q.isEmpty()) {
            int node = q.poll();
            result.add(node);
            for (int neighbor : graph.getOrDefault(node, new ArrayList<>())) {
                if (!visited.contains(neighbor)) {
                    visited.add(neighbor);
                    q.offer(neighbor);
                }
            }
        }
        return result;
    }

    public static void main(String[] args) {
        Map<Integer, List<Integer>> graph = new HashMap<>();
        graph.put(0, List.of(1, 2));
        graph.put(1, List.of(0, 3));
        graph.put(2, List.of(0, 3));
        graph.put(3, List.of(1, 2));
        System.out.println(bfs(graph, 0)); // [0, 1, 2, 3]
    }
}
```

## Graph Traversal — DFS

**Python**
```python
def dfs_recursive(graph, node, visited=None):
    if visited is None:
        visited = set()
    visited.add(node)
    result = [node]
    for neighbor in graph[node]:
        if neighbor not in visited:
            result.extend(dfs_recursive(graph, neighbor, visited))
    return result

def dfs_iterative(graph, start):
    visited = set()
    stack = [start]
    result = []
    while stack:
        node = stack.pop()
        if node not in visited:
            visited.add(node)
            result.append(node)
            for neighbor in graph[node]:
                if neighbor not in visited:
                    stack.append(neighbor)
    return result
```

**Java**
```java
import java.util.*;

public class DFS {
    public static List<Integer> dfsIterative(Map<Integer, List<Integer>> graph, int start) {
        Set<Integer> visited = new HashSet<>();
        Deque<Integer> stack = new ArrayDeque<>();
        List<Integer> result = new ArrayList<>();
        stack.push(start);
        while (!stack.isEmpty()) {
            int node = stack.pop();
            if (!visited.contains(node)) {
                visited.add(node);
                result.add(node);
                for (int neighbor : graph.getOrDefault(node, new ArrayList<>())) {
                    if (!visited.contains(neighbor)) {
                        stack.push(neighbor);
                    }
                }
            }
        }
        return result;
    }

    public static void dfsRecursive(Map<Integer, List<Integer>> graph, int node, Set<Integer> visited, List<Integer> result) {
        visited.add(node);
        result.add(node);
        for (int neighbor : graph.getOrDefault(node, new ArrayList<>())) {
            if (!visited.contains(neighbor)) {
                dfsRecursive(graph, neighbor, visited, result);
            }
        }
    }

    public static void main(String[] args) {
        Map<Integer, List<Integer>> graph = new HashMap<>();
        graph.put(0, List.of(1, 2));
        graph.put(1, List.of(0, 3));
        graph.put(2, List.of(0, 3));
        graph.put(3, List.of(1, 2));
        System.out.println(dfsIterative(graph, 0));
    }
}
```
