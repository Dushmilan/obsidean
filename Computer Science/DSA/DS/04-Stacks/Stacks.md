A stack is a LIFO (Last In, First Out) data structure — elements are added and removed from the same end (top). It's the natural structure for undo operations, recursion, and parsing expressions.

**The Intuition:** Think of a stack of plates. You can only take the top plate off or put a new one on top. You can't grab a plate from the middle or bottom without first removing everything above it. That constraint is what makes stacks useful — the most recently added item is always the first to be processed.

**The Math:**

| Operation | Time |
|-----------|------|
| Push | $O(1)$ |
| Pop | $O(1)$ |
| Peek / Top | $O(1)$ |
| Search | $O(n)$ |

**Key Patterns:** [[04-Stacks-Patterns]], [[04-Stacks-Patterns]], [[04-Stacks-Patterns]], [[04-Stacks-Patterns]], [[04-Stacks-Patterns]], [[04-Stacks-Patterns]]

**Applications:** Function call stack (recursion), expression evaluation (infix/postfix), undo/redo, backtracking (DFS), syntax parsing.

---
**See also:** [[Queues]], [[Recursion-Backtracking]]
