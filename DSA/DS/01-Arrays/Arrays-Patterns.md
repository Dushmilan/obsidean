---
date: 2026-07-20
type: code-patterns
tags:
  - dsa
  - arrays
  - python
  - java
---

# Arrays — Patterns

## Traversal

**Python**
```python
# Forward
for i in range(len(arr)):
    print(arr[i])

# Backward
for i in range(len(arr) - 1, -1, -1):
    print(arr[i])

# For-each (read-only)
for val in arr:
    print(val)
```

**Java**
```java
public class ArrayTraversal {
    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4, 5};

        // Forward
        for (int i = 0; i < arr.length; i++) {
            System.out.println(arr[i]);
        }

        // Backward
        for (int i = arr.length - 1; i >= 0; i--) {
            System.out.println(arr[i]);
        }

        // For-each (read-only)
        for (int val : arr) {
            System.out.println(val);
        }
    }
}
```

## Reverse In-Place
Swap elements symmetrically from both ends.

**Python**
```python
def reverse(arr):
    left, right = 0, len(arr) - 1
    while left < right:
        arr[left], arr[right] = arr[right], arr[left]
        left += 1
        right -= 1
```

**Java**
```java
public class ArrayReverse {
    public static void reverse(int[] arr) {
        int left = 0, right = arr.length - 1;
        while (left < right) {
            int temp = arr[left];
            arr[left] = arr[right];
            arr[right] = temp;
            left++;
            right--;
        }
    }

    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4, 5};
        reverse(arr);
        // arr = [5, 4, 3, 2, 1]
    }
}
```

## Rotate
Rotate array left/right by k positions.

**Python**
```python
def rotate_left(arr, k):
    k %= len(arr)
    arr[:] = arr[k:] + arr[:k]

def rotate_right(arr, k):
    k %= len(arr)
    arr[:] = arr[-k:] + arr[:-k]
```

**Java**
```java
public class ArrayRotate {
    public static void rotateLeft(int[] arr, int k) {
        int n = arr.length;
        k %= n;
        reverse(arr, 0, n - 1);
        reverse(arr, 0, n - k - 1);
        reverse(arr, n - k, n - 1);
    }

    public static void rotateRight(int[] arr, int k) {
        int n = arr.length;
        k %= n;
        reverse(arr, 0, n - 1);
        reverse(arr, 0, k - 1);
        reverse(arr, k, n - 1);
    }

    private static void reverse(int[] arr, int l, int r) {
        while (l < r) {
            int temp = arr[l];
            arr[l++] = arr[r];
            arr[r--] = temp;
        }
    }

    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4, 5};
        rotateLeft(arr, 2);
        // arr = [3, 4, 5, 1, 2]
    }
}
```

## Prefix Sum
Precompute cumulative sums for $O(1)$ range sum queries.

**Python**
```python
def prefix_sum(arr):
    pref = [0] * (len(arr) + 1)
    for i in range(len(arr)):
        pref[i + 1] = pref[i] + arr[i]
    return pref

# Range sum arr[l..r] inclusive = pref[r + 1] - pref[l]
```

**Java**
```java
public class PrefixSum {
    public static int[] computePrefixSum(int[] arr) {
        int n = arr.length;
        int[] pref = new int[n + 1];
        for (int i = 0; i < n; i++) {
            pref[i + 1] = pref[i] + arr[i];
        }
        return pref;
    }

    // Range sum arr[l..r] inclusive = pref[r + 1] - pref[l]

    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4, 5};
        int[] pref = computePrefixSum(arr);
        System.out.println(pref[4] - pref[1]); // sum arr[1..3] = 9
    }
}
```

## Two Sum
Find two indices whose values sum to target.

**Python**
```python
def two_sum(arr, target):
    seen = {}
    for i, val in enumerate(arr):
        complement = target - val
        if complement in seen:
            return [seen[complement], i]
        seen[val] = i
    return []
```

**Java**
```java
import java.util.HashMap;
import java.util.Map;

public class TwoSum {
    public static int[] twoSum(int[] arr, int target) {
        Map<Integer, Integer> seen = new HashMap<>();
        for (int i = 0; i < arr.length; i++) {
            int complement = target - arr[i];
            if (seen.containsKey(complement)) {
                return new int[]{seen.get(complement), i};
            }
            seen.put(arr[i], i);
        }
        return new int[]{};
    }

    public static void main(String[] args) {
        int[] arr = {2, 7, 11, 15};
        int[] result = twoSum(arr, 9);
        System.out.println(result[0] + " " + result[1]); // 0 1
    }
}
```

## Maximum Subarray (Kadane's)
Find the contiguous subarray with the largest sum.

**Python**
```python
def max_subarray(arr):
    max_ending = max_so_far = arr[0]
    for x in arr[1:]:
        max_ending = max(x, max_ending + x)
        max_so_far = max(max_so_far, max_ending)
    return max_so_far
```

**Java**
```java
public class Kadane {
    public static int maxSubarray(int[] arr) {
        int maxEnding = arr[0], maxSoFar = arr[0];
        for (int i = 1; i < arr.length; i++) {
            maxEnding = Math.max(arr[i], maxEnding + arr[i]);
            maxSoFar = Math.max(maxSoFar, maxEnding);
        }
        return maxSoFar;
    }

    public static void main(String[] args) {
        int[] arr = {-2, 1, -3, 4, -1, 2, 1, -5, 4};
        System.out.println(maxSubarray(arr)); // 6
    }
}
```

## Dutch National Flag
Sort an array of 0s, 1s, 2s in $O(n)$ with three pointers.

**Python**
```python
def sort_colors(arr):
    low, mid, high = 0, 0, len(arr) - 1
    while mid <= high:
        if arr[mid] == 0:
            arr[low], arr[mid] = arr[mid], arr[low]
            low += 1
            mid += 1
        elif arr[mid] == 1:
            mid += 1
        else:
            arr[mid], arr[high] = arr[high], arr[mid]
            high -= 1
```

**Java**
```java
public class DutchFlag {
    public static void sortColors(int[] arr) {
        int low = 0, mid = 0, high = arr.length - 1;
        while (mid <= high) {
            if (arr[mid] == 0) {
                int temp = arr[low];
                arr[low] = arr[mid];
                arr[mid] = temp;
                low++;
                mid++;
            } else if (arr[mid] == 1) {
                mid++;
            } else {
                int temp = arr[mid];
                arr[mid] = arr[high];
                arr[high] = temp;
                high--;
            }
        }
    }

    public static void main(String[] args) {
        int[] arr = {2, 0, 2, 1, 1, 0};
        sortColors(arr);
        // arr = [0, 0, 1, 1, 2, 2]
    }
}
```
