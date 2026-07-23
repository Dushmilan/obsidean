---
date: 2026-07-20
type: code-patterns
tags:
  - dsa
  - searching
  - python
  - java
---

# Searching — Patterns

## Linear Search

**Python**
```python
def linear_search(arr, target):
    for i, val in enumerate(arr):
        if val == target:
            return i
    return -1
```

**Java**
```java
public class LinearSearch {
    public static int linearSearch(int[] arr, int target) {
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == target) return i;
        }
        return -1;
    }

    public static void main(String[] args) {
        int[] arr = {2, 3, 4, 10, 40};
        System.out.println(linearSearch(arr, 10)); // 3
    }
}
```

## Binary Search — Basic

**Python**
```python
def binary_search(arr, target):
    lo, hi = 0, len(arr) - 1
    while lo <= hi:
        mid = lo + (hi - lo) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1
    return -1
```

**Java**
```java
public class BinarySearch {
    public static int binarySearch(int[] arr, int target) {
        int lo = 0, hi = arr.length - 1;
        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;
            if (arr[mid] == target) return mid;
            else if (arr[mid] < target) lo = mid + 1;
            else hi = mid - 1;
        }
        return -1;
    }

    public static void main(String[] args) {
        int[] arr = {2, 3, 4, 10, 40};
        System.out.println(binarySearch(arr, 10)); // 3
    }
}
```

## Binary Search — First/Last Occurrence

**Python**
```python
def first_occurrence(arr, target):
    lo, hi = 0, len(arr) - 1
    result = -1
    while lo <= hi:
        mid = lo + (hi - lo) // 2
        if arr[mid] == target:
            result = mid
            hi = mid - 1
        elif arr[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1
    return result

def last_occurrence(arr, target):
    lo, hi = 0, len(arr) - 1
    result = -1
    while lo <= hi:
        mid = lo + (hi - lo) // 2
        if arr[mid] == target:
            result = mid
            lo = mid + 1
        elif arr[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1
    return result
```

**Java**
```java
public class Occurrence {
    public static int firstOccurrence(int[] arr, int target) {
        int lo = 0, hi = arr.length - 1, result = -1;
        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;
            if (arr[mid] == target) {
                result = mid;
                hi = mid - 1;
            } else if (arr[mid] < target) lo = mid + 1;
            else hi = mid - 1;
        }
        return result;
    }

    public static int lastOccurrence(int[] arr, int target) {
        int lo = 0, hi = arr.length - 1, result = -1;
        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;
            if (arr[mid] == target) {
                result = mid;
                lo = mid + 1;
            } else if (arr[mid] < target) lo = mid + 1;
            else hi = mid - 1;
        }
        return result;
    }

    public static void main(String[] args) {
        int[] arr = {1, 2, 2, 2, 3, 4};
        System.out.println(firstOccurrence(arr, 2)); // 1
        System.out.println(lastOccurrence(arr, 2));  // 3
    }
}
```

## Binary Search — Closest Element

**Python**
```python
def closest_element(arr, target):
    lo, hi = 0, len(arr) - 1
    if target <= arr[lo]: return arr[lo]
    if target >= arr[hi]: return arr[hi]
    while lo <= hi:
        mid = lo + (hi - lo) // 2
        if arr[mid] == target:
            return arr[mid]
        elif arr[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1
    # After loop, lo is the insertion point
    if abs(arr[lo] - target) < abs(arr[hi] - target):
        return arr[lo]
    return arr[hi]
```

**Java**
```java
public class ClosestElement {
    public static int closestElement(int[] arr, int target) {
        int lo = 0, hi = arr.length - 1;
        if (target <= arr[lo]) return arr[lo];
        if (target >= arr[hi]) return arr[hi];
        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;
            if (arr[mid] == target) return arr[mid];
            else if (arr[mid] < target) lo = mid + 1;
            else hi = mid - 1;
        }
        return (Math.abs(arr[lo] - target) < Math.abs(arr[hi] - target))
            ? arr[lo] : arr[hi];
    }

    public static void main(String[] args) {
        int[] arr = {1, 3, 5, 7, 9};
        System.out.println(closestElement(arr, 6)); // 5 or 7
    }
}
```

## Binary Search on Rotated Sorted Array

**Python**
```python
def search_rotated(arr, target):
    lo, hi = 0, len(arr) - 1
    while lo <= hi:
        mid = lo + (hi - lo) // 2
        if arr[mid] == target:
            return mid
        # Left half is sorted
        if arr[lo] <= arr[mid]:
            if arr[lo] <= target < arr[mid]:
                hi = mid - 1
            else:
                lo = mid + 1
        # Right half is sorted
        else:
            if arr[mid] < target <= arr[hi]:
                lo = mid + 1
            else:
                hi = mid - 1
    return -1
```

**Java**
```java
public class SearchRotated {
    public static int searchRotated(int[] arr, int target) {
        int lo = 0, hi = arr.length - 1;
        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;
            if (arr[mid] == target) return mid;
            if (arr[lo] <= arr[mid]) {
                if (arr[lo] <= target && target < arr[mid]) hi = mid - 1;
                else lo = mid + 1;
            } else {
                if (arr[mid] < target && target <= arr[hi]) lo = mid + 1;
                else hi = mid - 1;
            }
        }
        return -1;
    }

    public static void main(String[] args) {
        int[] arr = {4, 5, 6, 7, 0, 1, 2};
        System.out.println(searchRotated(arr, 0)); // 4
    }
}
```

## Binary Search — Square Root

**Python**
```python
def sqrt(x: int) -> int:
    if x < 2: return x
    lo, hi = 1, x // 2
    while lo <= hi:
        mid = lo + (hi - lo) // 2
        if mid * mid == x:
            return mid
        elif mid * mid < x:
            lo = mid + 1
        else:
            hi = mid - 1
    return hi
```

**Java**
```java
public class Sqrt {
    public static int sqrt(int x) {
        if (x < 2) return x;
        int lo = 1, hi = x / 2;
        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;
            long sq = (long) mid * mid;
            if (sq == x) return mid;
            else if (sq < x) lo = mid + 1;
            else hi = mid - 1;
        }
        return hi;
    }

    public static void main(String[] args) {
        System.out.println(sqrt(8));  // 2
        System.out.println(sqrt(16)); // 4
    }
}
```
