---
date: 2026-07-20
type: code-patterns
tags:
  - dsa
  - two-pointers
  - python
  - java
---

# Two Pointers — Patterns

## Two Sum (Sorted)

**Python**
```python
def two_sum_sorted(arr, target):
    lo, hi = 0, len(arr) - 1
    while lo < hi:
        curr = arr[lo] + arr[hi]
        if curr == target:
            return [lo, hi]
        elif curr < target:
            lo += 1
        else:
            hi -= 1
    return []
```

**Java**
```java
import java.util.Arrays;

public class TwoSumSorted {
    public static int[] twoSumSorted(int[] arr, int target) {
        int lo = 0, hi = arr.length - 1;
        while (lo < hi) {
            int sum = arr[lo] + arr[hi];
            if (sum == target) return new int[]{lo, hi};
            else if (sum < target) lo++;
            else hi--;
        }
        return new int[]{};
    }

    public static void main(String[] args) {
        int[] arr = {2, 7, 11, 15};
        System.out.println(Arrays.toString(twoSumSorted(arr, 9))); // [0, 1]
    }
}
```

## Remove Duplicates (Sorted Array)

**Python**
```python
def remove_duplicates(arr):
    if not arr:
        return 0
    i = 0
    for j in range(1, len(arr)):
        if arr[j] != arr[i]:
            i += 1
            arr[i] = arr[j]
    return i + 1
```

**Java**
```java
import java.util.Arrays;

public class RemoveDuplicates {
    public static int removeDuplicates(int[] arr) {
        if (arr.length == 0) return 0;
        int i = 0;
        for (int j = 1; j < arr.length; j++) {
            if (arr[j] != arr[i]) {
                i++;
                arr[i] = arr[j];
            }
        }
        return i + 1;
    }

    public static void main(String[] args) {
        int[] arr = {1, 1, 2, 2, 3};
        int k = removeDuplicates(arr);
        System.out.println(k); // 3
    }
}
```

## Three Sum

**Python**
```python
def three_sum(arr):
    arr.sort()
    res = []
    n = len(arr)
    for i in range(n - 2):
        if i > 0 and arr[i] == arr[i - 1]:
            continue
        lo, hi = i + 1, n - 1
        while lo < hi:
            s = arr[i] + arr[lo] + arr[hi]
            if s == 0:
                res.append([arr[i], arr[lo], arr[hi]])
                while lo < hi and arr[lo] == arr[lo + 1]: lo += 1
                while lo < hi and arr[hi] == arr[hi - 1]: hi -= 1
                lo += 1
                hi -= 1
            elif s < 0:
                lo += 1
            else:
                hi -= 1
    return res
```

**Java**
```java
import java.util.*;

public class ThreeSum {
    public static List<List<Integer>> threeSum(int[] arr) {
        Arrays.sort(arr);
        List<List<Integer>> res = new ArrayList<>();
        int n = arr.length;
        for (int i = 0; i < n - 2; i++) {
            if (i > 0 && arr[i] == arr[i - 1]) continue;
            int lo = i + 1, hi = n - 1;
            while (lo < hi) {
                int sum = arr[i] + arr[lo] + arr[hi];
                if (sum == 0) {
                    res.add(Arrays.asList(arr[i], arr[lo], arr[hi]));
                    while (lo < hi && arr[lo] == arr[lo + 1]) lo++;
                    while (lo < hi && arr[hi] == arr[hi - 1]) hi--;
                    lo++; hi--;
                } else if (sum < 0) lo++;
                else hi--;
            }
        }
        return res;
    }

    public static void main(String[] args) {
        int[] arr = {-1, 0, 1, 2, -1, -4};
        System.out.println(threeSum(arr)); // [[-1, -1, 2], [-1, 0, 1]]
    }
}
```

## Container With Most Water

**Python**
```python
def max_area(height):
    lo, hi = 0, len(height) - 1
    max_water = 0
    while lo < hi:
        h = min(height[lo], height[hi])
        w = hi - lo
        max_water = max(max_water, h * w)
        if height[lo] < height[hi]:
            lo += 1
        else:
            hi -= 1
    return max_water
```

**Java**
```java
public class ContainerMostWater {
    public static int maxArea(int[] height) {
        int lo = 0, hi = height.length - 1, maxWater = 0;
        while (lo < hi) {
            int h = Math.min(height[lo], height[hi]);
            int w = hi - lo;
            maxWater = Math.max(maxWater, h * w);
            if (height[lo] < height[hi]) lo++;
            else hi--;
        }
        return maxWater;
    }

    public static void main(String[] args) {
        int[] height = {1, 8, 6, 2, 5, 4, 8, 3, 7};
        System.out.println(maxArea(height)); // 49
    }
}
```

## Trapping Rain Water

**Python**
```python
def trap(heights):
    lo, hi = 0, len(heights) - 1
    left_max = right_max = water = 0
    while lo < hi:
        if heights[lo] < heights[hi]:
            if heights[lo] >= left_max:
                left_max = heights[lo]
            else:
                water += left_max - heights[lo]
            lo += 1
        else:
            if heights[hi] >= right_max:
                right_max = heights[hi]
            else:
                water += right_max - heights[hi]
            hi -= 1
    return water
```

**Java**
```java
public class TrapRainWater {
    public static int trap(int[] heights) {
        int lo = 0, hi = heights.length - 1;
        int leftMax = 0, rightMax = 0, water = 0;
        while (lo < hi) {
            if (heights[lo] < heights[hi]) {
                if (heights[lo] >= leftMax) leftMax = heights[lo];
                else water += leftMax - heights[lo];
                lo++;
            } else {
                if (heights[hi] >= rightMax) rightMax = heights[hi];
                else water += rightMax - heights[hi];
                hi--;
            }
        }
        return water;
    }

    public static void main(String[] args) {
        int[] heights = {0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1};
        System.out.println(trap(heights)); // 6
    }
}
```
