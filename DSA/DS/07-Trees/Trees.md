---
date: 2026-07-20
type: concept
tags:
  - dsa
  - data-structures
  - trees
---

# Trees

## Definition
A hierarchical data structure with a root node and child nodes forming a parent-child relationship.

## Terminology
- **Root** — topmost node (no parent)
- **Leaf** — node with no children
- **Subtree** — a node and all its descendants
- **Height** — longest path from root to leaf
- **Depth** — edges from root to the node

## Types

### Binary Tree
Each node has at most 2 children (left, right).

| Traversal | Order |
|-----------|-------|
| Inorder | Left → Root → Right |
| Preorder | Root → Left → Right |
| Postorder | Left → Right → Root |
| Level-order | BFS by level |

### Binary Search Tree (BST)
For every node: left subtree < node < right subtree.

| Operation | Average | Worst |
|-----------|---------|-------|
| Search | $O(\log n)$ | $O(n)$ |
| Insert | $O(\log n)$ | $O(n)$ |
| Delete | $O(\log n)$ | $O(n)$ |

### Heap (Priority Queue)
Complete binary tree where parent is always >= (max-heap) or <= (min-heap) its children.

## Key Patterns
- [[07-Trees-Patterns.md#Binary Tree Traversals|Tree traversals (recursive & iterative)]]
- [[07-Trees-Patterns.md#Maximum Depth of Binary Tree|Max depth]]
- [[07-Trees-Patterns.md#Validate BST|Validate BST]]
- [[07-Trees-Patterns.md#Level Order Traversal|Level order traversal]]
- [[07-Trees-Patterns.md#Lowest Common Ancestor|Lowest common ancestor]]
- [[07-Trees-Patterns.md#Heap Operations|Heap / priority queue]]

## Applications
- File systems
- HTML DOM
- Database indexes (B-trees)
- Expression parsing
- Huffman coding

---

**See also:** [[../08-Graphs/Graphs.md|Graphs]], [[../../Algo/09-Graph-Algos/Graph-Algos.md|Graph Algos]]
