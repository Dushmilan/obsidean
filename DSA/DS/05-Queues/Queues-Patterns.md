---
date: 2026-07-20
type: code-patterns
tags:
  - dsa
  - queues
  - python
  - java
---

# Queues — Patterns

## Basic Queue Operations

**Python**
```python
from collections import deque

queue = deque()
queue.append(1)        # enqueue
queue.append(2)
front = queue[0]       # peek
popped = queue.popleft()  # dequeue
is_empty = len(queue) == 0
```

**Java**
```java
import java.util.LinkedList;
import java.util.Queue;

public class QueueBasics {
    public static void main(String[] args) {
        Queue<Integer> queue = new LinkedList<>();
        queue.offer(1);     // enqueue
        queue.offer(2);
        int front = queue.peek();   // peek
        int popped = queue.poll();  // dequeue
        boolean empty = queue.isEmpty();
    }
}
```

## Circular Queue

**Python**
```python
class CircularQueue:
    def __init__(self, k: int):
        self.arr = [0] * k
        self.front = self.rear = 0
        self.size = 0
        self.capacity = k

    def enqueue(self, val: int) -> bool:
        if self.is_full():
            return False
        self.arr[self.rear] = val
        self.rear = (self.rear + 1) % self.capacity
        self.size += 1
        return True

    def dequeue(self) -> bool:
        if self.is_empty():
            return False
        self.front = (self.front + 1) % self.capacity
        self.size -= 1
        return True

    def front(self) -> int:
        return -1 if self.is_empty() else self.arr[self.front]

    def rear(self) -> int:
        return -1 if self.is_empty() else self.arr[(self.rear - 1) % self.capacity]

    def is_empty(self) -> bool:
        return self.size == 0

    def is_full(self) -> bool:
        return self.size == self.capacity
```

**Java**
```java
public class CircularQueue {
    private int[] arr;
    private int front, rear, size, capacity;

    public CircularQueue(int k) {
        arr = new int[k];
        capacity = k;
        front = 0;
        rear = 0;
        size = 0;
    }

    public boolean enqueue(int val) {
        if (isFull()) return false;
        arr[rear] = val;
        rear = (rear + 1) % capacity;
        size++;
        return true;
    }

    public boolean dequeue() {
        if (isEmpty()) return false;
        front = (front + 1) % capacity;
        size--;
        return true;
    }

    public int front() {
        return isEmpty() ? -1 : arr[front];
    }

    public int rear() {
        return isEmpty() ? -1 : arr[(rear - 1 + capacity) % capacity];
    }

    public boolean isEmpty() { return size == 0; }
    public boolean isFull()  { return size == capacity; }

    public static void main(String[] args) {
        CircularQueue cq = new CircularQueue(3);
        cq.enqueue(1); cq.enqueue(2); cq.enqueue(3);
        System.out.println(cq.isFull());  // true
        cq.dequeue();
        cq.enqueue(4);
        System.out.println(cq.rear());    // 4
    }
}
```

## Deque as Stack / Queue

**Python**
```python
from collections import deque

# As queue (FIFO)
dq = deque()
dq.append(1)       # add to right
dq.append(2)
left = dq.popleft()  # remove from left

# As stack (LIFO)
dq.append(3)
top = dq.pop()     # remove from right

# Deque specific
dq.appendleft(0)   # add to left
right = dq.pop()   # remove from right
```

**Java**
```java
import java.util.ArrayDeque;
import java.util.Deque;

public class DequeDemo {
    public static void main(String[] args) {
        Deque<Integer> dq = new ArrayDeque<>();

        // As queue (FIFO)
        dq.addLast(1);
        dq.addLast(2);
        int front = dq.removeFirst(); // 1

        // As stack (LIFO)
        dq.addLast(3);
        int top = dq.removeLast();    // 3

        // Deque specific
        dq.addFirst(0);
        int last = dq.removeLast();
    }
}
```

## First Non-Repeating Character in Stream

**Python**
```python
from collections import deque

def first_non_repeating(stream: str) -> str:
    freq = {}
    dq = deque()
    result = []

    for ch in stream:
        freq[ch] = freq.get(ch, 0) + 1
        dq.append(ch)
        while dq and freq[dq[0]] > 1:
            dq.popleft()
        result.append(dq[0] if dq else '#')

    return "".join(result)
```

**Java**
```java
import java.util.ArrayDeque;
import java.util.Deque;
import java.util.HashMap;
import java.util.Map;

public class FirstNonRepeating {
    public static String firstNonRepeating(String stream) {
        Map<Character, Integer> freq = new HashMap<>();
        Deque<Character> dq = new ArrayDeque<>();
        StringBuilder result = new StringBuilder();

        for (char ch : stream.toCharArray()) {
            freq.put(ch, freq.getOrDefault(ch, 0) + 1);
            dq.addLast(ch);
            while (!dq.isEmpty() && freq.get(dq.peekFirst()) > 1) {
                dq.removeFirst();
            }
            result.append(dq.isEmpty() ? '#' : dq.peekFirst());
        }
        return result.toString();
    }

    public static void main(String[] args) {
        System.out.println(firstNonRepeating("aabc")); // a#bc
    }
}
```

## Sliding Window Maximum

**Python**
```python
from collections import deque

def max_sliding_window(arr: list[int], k: int) -> list[int]:
    dq = deque()
    result = []

    for i in range(len(arr)):
        while dq and dq[0] < i - k + 1:
            dq.popleft()
        while dq and arr[dq[-1]] < arr[i]:
            dq.pop()
        dq.append(i)
        if i >= k - 1:
            result.append(arr[dq[0]])

    return result
```

**Java**
```java
import java.util.ArrayDeque;
import java.util.Arrays;
import java.util.Deque;

public class SlidingWindowMax {
    public static int[] maxSlidingWindow(int[] arr, int k) {
        int n = arr.length;
        int[] result = new int[n - k + 1];
        Deque<Integer> dq = new ArrayDeque<>();
        int ri = 0;

        for (int i = 0; i < n; i++) {
            while (!dq.isEmpty() && dq.peekFirst() < i - k + 1) {
                dq.pollFirst();
            }
            while (!dq.isEmpty() && arr[dq.peekLast()] < arr[i]) {
                dq.pollLast();
            }
            dq.offerLast(i);
            if (i >= k - 1) {
                result[ri++] = arr[dq.peekFirst()];
            }
        }
        return result;
    }

    public static void main(String[] args) {
        int[] arr = {1, 3, -1, -3, 5, 3, 6, 7};
        System.out.println(Arrays.toString(maxSlidingWindow(arr, 3)));
        // [3, 3, 5, 5, 6, 7]
    }
}
```
