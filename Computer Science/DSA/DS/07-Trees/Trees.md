A tree is a hierarchical data structure with a root node and child nodes forming parent-child relationships. Trees model anything with nested structure — file systems, HTML DOM, organizational charts, and decision processes.

**The Intuition:** Think of a family tree. Your grandparents are at the top (root), their children branch below, and so on. Each person (node) has at most two children in a binary tree. A binary search tree adds an ordering rule: everyone to the left has a "smaller" name, everyone to the right has a "bigger" name — making search efficient.

**The Math:**

| Traversal | Order | Use Case |
|-----------|-------|----------|
| Inorder | Left → Root → Right | Sorted order in BST |
| Preorder | Root → Left → Right | Copy/serialize tree |
| Postorder | Left → Right → Root | Delete tree, evaluate expression |
| Level-order | BFS by level | Level-based processing |

**BST Operations:**

| Operation | Average | Worst (skewed) |
|-----------|---------|----------------|
| Search | $O(\log n)$ | $O(n)$ |
| Insert | $O(\log n)$ | $O(n)$ |
| Delete | $O(\log n)$ | $O(n)$ |

**Key Patterns:** [[07-Trees-Patterns]], [[07-Trees-Patterns]], [[07-Trees-Patterns]], [[07-Trees-Patterns]], [[07-Trees-Patterns]], [[07-Trees-Patterns]]

**Applications:** File systems, HTML DOM, database indexes (B-trees), expression parsing, Huffman coding.

---
**See also:** [[Graphs]], [[Graph-Algos]]
