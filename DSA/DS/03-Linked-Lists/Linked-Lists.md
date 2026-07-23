---
date: 2026-07-20
type: concept
tags:
  - dsa
  - data-structures
  - linked-lists
---

# Linked Lists

## Definition
A linear data structure where elements (nodes) are connected via pointers. Each node contains data and a reference to the next node.

## Types
- **Singly linked** — each node points to the next node only
- **Doubly linked** — each node points to both next and prev
- **Circular linked** — last node points back to the head

## Time Complexity

| Operation | Singly | Doubly |
|-----------|--------|--------|
| Access by index | $O(n)$ | $O(n)$ |
| Search | $O(n)$ | $O(n)$ |
| Insert at head | $O(1)$ | $O(1)$ |
| Insert at tail | $O(n)$ / $O(1)$* | $O(1)$ |
| Delete at head | $O(1)$ | $O(1)$ |
| Delete by value | $O(n)$ | $O(n)$ |

\* $O(1)$ with tail pointer

## Key Patterns
- [[03-Linked-Lists-Patterns.md#Traversal|Traversal]]
- [[03-Linked-Lists-Patterns.md#Reverse a Linked List|Reverse]]
- [[03-Linked-Lists-Patterns.md#Detect Cycle (Floyd's)|Cycle detection]]
- [[03-Linked-Lists-Patterns.md#Find Middle Node|Middle node]]
- [[03-Linked-Lists-Patterns.md#Merge Two Sorted Lists|Merge two sorted lists]]
- [[03-Linked-Lists-Patterns.md#Remove Nth Node From End|Remove nth from end]]
- [[03-Linked-Lists-Patterns.md#Intersection of Two Lists|Intersection]]

## Applications
- Dynamic memory allocation (free lists)
- Undo/redo in editors (doubly linked)
- LRU cache
- Adjacency lists for graphs

---

**See also:** [[../04-Stacks/Stacks.md|Stacks]], [[../08-Graphs/Graphs.md|Graphs]]
