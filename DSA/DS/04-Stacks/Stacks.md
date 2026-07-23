---
date: 2026-07-20
type: concept
tags:
  - dsa
  - data-structures
  - stacks
---

# Stacks

## Definition
A LIFO (Last In, First Out) data structure. Elements are added and removed from the same end (top).

## Time Complexity

| Operation | Complexity |
|-----------|------------|
| Push | $O(1)$ |
| Pop | $O(1)$ |
| Peek / Top | $O(1)$ |
| Search | $O(n)$ |

## Key Patterns
- [[04-Stacks-Patterns.md#Basic Stack Operations|Basic operations]]
- [[04-Stacks-Patterns.md#Valid Parentheses|Valid parentheses]]
- [[04-Stacks-Patterns.md#Min Stack|Min stack]]
- [[04-Stacks-Patterns.md#Next Greater Element|Next greater element]]
- [[04-Stacks-Patterns.md#Evaluate Reverse Polish Notation|Evaluate RPN]]
- [[04-Stacks-Patterns.md#Stock Span Problem|Stock span]]

## Applications
- Function call stack (recursion)
- Expression evaluation (infix/postfix)
- Undo/redo
- Backtracking (DFS)
- Syntax parsing

---

**See also:** [[../05-Queues/Queues.md|Queues]], [[../../Algo/05-Recursion-Backtracking/Recursion-Backtracking.md|Recursion]]
