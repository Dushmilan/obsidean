---
date: 2026-07-20
type: code-patterns
tags:
  - dsa
  - linked-lists
  - python
  - java
---

# Linked Lists — Patterns

## Node Definition

**Python**
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class DoublyListNode:
    def __init__(self, val=0, prev=None, next=None):
        self.val = val
        self.prev = prev
        self.next = next
```

**Java**
```java
public class ListNode {
    int val;
    ListNode next;
    ListNode() {}
    ListNode(int val) { this.val = val; }
    ListNode(int val, ListNode next) { this.val = val; this.next = next; }
}

public class DoublyListNode {
    int val;
    DoublyListNode prev;
    DoublyListNode next;
    DoublyListNode(int val) { this.val = val; }
}
```

## Traversal

**Python**
```python
def traverse(head: ListNode) -> None:
    curr = head
    while curr:
        print(curr.val)
        curr = curr.next
```

**Java**
```java
public class LinkedListTraversal {
    public static void traverse(ListNode head) {
        ListNode curr = head;
        while (curr != null) {
            System.out.println(curr.val);
            curr = curr.next;
        }
    }

    public static void main(String[] args) {
        ListNode head = new ListNode(1, new ListNode(2, new ListNode(3)));
        traverse(head);
    }
}
```

## Reverse a Linked List

**Python**
```python
def reverse(head: ListNode) -> ListNode:
    prev, curr = None, head
    while curr:
        temp = curr.next
        curr.next = prev
        prev = curr
        curr = temp
    return prev
```

**Java**
```java
public class ReverseList {
    public static ListNode reverse(ListNode head) {
        ListNode prev = null, curr = head;
        while (curr != null) {
            ListNode temp = curr.next;
            curr.next = prev;
            prev = curr;
            curr = temp;
        }
        return prev;
    }

    public static void main(String[] args) {
        ListNode head = new ListNode(1, new ListNode(2, new ListNode(3)));
        ListNode reversed = reverse(head);
        // 3 -> 2 -> 1
    }
}
```

## Detect Cycle (Floyd's Tortoise & Hare)

**Python**
```python
def has_cycle(head: ListNode) -> bool:
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            return True
    return False
```

**Java**
```java
public class CycleDetection {
    public static boolean hasCycle(ListNode head) {
        ListNode slow = head, fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) return true;
        }
        return false;
    }

    public static void main(String[] args) {
        ListNode head = new ListNode(1);
        head.next = new ListNode(2);
        head.next.next = new ListNode(3);
        head.next.next.next = head.next; // cycle
        System.out.println(hasCycle(head)); // true
    }
}
```

## Find Middle Node

**Python**
```python
def middle_node(head: ListNode) -> ListNode:
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    return slow
```

**Java**
```java
public class MiddleNode {
    public static ListNode middleNode(ListNode head) {
        ListNode slow = head, fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }

    public static void main(String[] args) {
        ListNode head = new ListNode(1, new ListNode(2, new ListNode(3, new ListNode(4))));
        ListNode mid = middleNode(head);
        System.out.println(mid.val); // 3
    }
}
```

## Merge Two Sorted Lists

**Python**
```python
def merge_two_lists(l1: ListNode, l2: ListNode) -> ListNode:
    dummy = ListNode()
    curr = dummy
    while l1 and l2:
        if l1.val < l2.val:
            curr.next = l1
            l1 = l1.next
        else:
            curr.next = l2
            l2 = l2.next
        curr = curr.next
    curr.next = l1 or l2
    return dummy.next
```

**Java**
```java
public class MergeSortedLists {
    public static ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode();
        ListNode curr = dummy;
        while (l1 != null && l2 != null) {
            if (l1.val < l2.val) {
                curr.next = l1;
                l1 = l1.next;
            } else {
                curr.next = l2;
                l2 = l2.next;
            }
                curr = curr.next;
        }
        curr.next = (l1 != null) ? l1 : l2;
        return dummy.next;
    }

    public static void main(String[] args) {
        ListNode l1 = new ListNode(1, new ListNode(3, new ListNode(5)));
        ListNode l2 = new ListNode(2, new ListNode(4, new ListNode(6)));
        ListNode merged = mergeTwoLists(l1, l2);
        // 1 -> 2 -> 3 -> 4 -> 5 -> 6
    }
}
```

## Remove Nth Node From End

**Python**
```python
def remove_nth_from_end(head: ListNode, n: int) -> ListNode:
    dummy = ListNode(0, head)
    slow = fast = dummy
    for _ in range(n + 1):
        fast = fast.next
    while fast:
        slow = slow.next
        fast = fast.next
    slow.next = slow.next.next
    return dummy.next
```

**Java**
```java
public class RemoveNthFromEnd {
    public static ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0, head);
        ListNode slow = dummy, fast = dummy;
        for (int i = 0; i <= n; i++) {
            fast = fast.next;
        }
        while (fast != null) {
            slow = slow.next;
            fast = fast.next;
        }
        slow.next = slow.next.next;
        return dummy.next;
    }

    public static void main(String[] args) {
        ListNode head = new ListNode(1, new ListNode(2, new ListNode(3, new ListNode(4, new ListNode(5)))));
        ListNode result = removeNthFromEnd(head, 2);
        // 1 -> 2 -> 3 -> 5
    }
}
```

## Intersection of Two Lists

**Python**
```python
def get_intersection_node(headA: ListNode, headB: ListNode) -> ListNode:
    a, b = headA, headB
    while a != b:
        a = a.next if a else headB
        b = b.next if b else headA
    return a
```

**Java**
```java
public class Intersection {
    public static ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode a = headA, b = headB;
        while (a != b) {
            a = (a != null) ? a.next : headB;
            b = (b != null) ? b.next : headA;
        }
        return a;
    }

    public static void main(String[] args) {
        ListNode common = new ListNode(8, new ListNode(10));
        ListNode headA = new ListNode(3, new ListNode(7, common));
        ListNode headB = new ListNode(99, new ListNode(1, common));
        System.out.println(getIntersectionNode(headA, headB).val); // 8
    }
}
```
