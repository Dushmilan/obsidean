---
date: 2026-07-20
type: code-patterns
tags:
  - dsa
  - divide-conquer
  - python
  - java
---

# Divide & Conquer — Patterns

## Merge Sort

**Python**
```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    return merge(left, right)

def merge(left, right):
    res = []
    i = j = 0
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            res.append(left[i])
            i += 1
        else:
            res.append(right[j])
            j += 1
    return res + left[i:] + right[j:]
```

**Java**
```java
import java.util.Arrays;

public class MergeSortDnC {
    public static void mergeSort(int[] arr, int l, int r) {
        if (l < r) {
            int mid = l + (r - l) / 2;
            mergeSort(arr, l, mid);
            mergeSort(arr, mid + 1, r);
            merge(arr, l, mid, r);
        }
    }

    private static void merge(int[] arr, int l, int mid, int r) {
        int n1 = mid - l + 1, n2 = r - mid;
        int[] left = new int[n1], right = new int[n2];
        for (int i = 0; i < n1; i++) left[i] = arr[l + i];
        for (int i = 0; i < n2; i++) right[i] = arr[mid + 1 + i];
        int i = 0, j = 0, k = l;
        while (i < n1 && j < n2) arr[k++] = (left[i] <= right[j]) ? left[i++] : right[j++];
        while (i < n1) arr[k++] = left[i++];
        while (j < n2) arr[k++] = right[j++];
    }

    public static void main(String[] args) {
        int[] arr = {38, 27, 43, 3, 9, 82, 10};
        mergeSort(arr, 0, arr.length - 1);
        System.out.println(Arrays.toString(arr));
    }
}
```

## Quick Sort

**Python**
```python
def quick_sort(arr, lo=0, hi=None):
    if hi is None:
        hi = len(arr) - 1
    if lo < hi:
        p = partition(arr, lo, hi)
        quick_sort(arr, lo, p - 1)
        quick_sort(arr, p + 1, hi)

def partition(arr, lo, hi):
    pivot = arr[hi]
    i = lo - 1
    for j in range(lo, hi):
        if arr[j] <= pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    arr[i + 1], arr[hi] = arr[hi], arr[i + 1]
    return i + 1
```

**Java**
```java
import java.util.Arrays;

public class QuickSortDnC {
    public static void quickSort(int[] arr, int lo, int hi) {
        if (lo < hi) {
            int p = partition(arr, lo, hi);
            quickSort(arr, lo, p - 1);
            quickSort(arr, p + 1, hi);
        }
    }

    private static int partition(int[] arr, int lo, int hi) {
        int pivot = arr[hi], i = lo - 1;
        for (int j = lo; j < hi; j++) {
            if (arr[j] <= pivot) {
                i++;
                int temp = arr[i]; arr[i] = arr[j]; arr[j] = temp;
            }
        }
        int temp = arr[i + 1]; arr[i + 1] = arr[hi]; arr[hi] = temp;
        return i + 1;
    }

    public static void main(String[] args) {
        int[] arr = {10, 7, 8, 9, 1, 5};
        quickSort(arr, 0, arr.length - 1);
        System.out.println(Arrays.toString(arr));
    }
}
```

## Maximum Subarray (Divide & Conquer)

**Python**
```python
def max_subarray(arr):
    def divide_conquer(lo, hi):
        if lo == hi:
            return arr[lo]
        mid = (lo + hi) // 2
        left_max = divide_conquer(lo, mid)
        right_max = divide_conquer(mid + 1, hi)
        cross_max = max_crossing(lo, mid, hi)
        return max(left_max, right_max, cross_max)

    def max_crossing(lo, mid, hi):
        left_sum = float('-inf')
        s = 0
        for i in range(mid, lo - 1, -1):
            s += arr[i]
            left_sum = max(left_sum, s)
        right_sum = float('-inf')
        s = 0
        for i in range(mid + 1, hi + 1):
            s += arr[i]
            right_sum = max(right_sum, s)
        return left_sum + right_sum

    return divide_conquer(0, len(arr) - 1)
```

**Java**
```java
public class MaxSubarrayDnC {
    public static int maxSubarray(int[] arr) {
        return divideConquer(arr, 0, arr.length - 1);
    }

    private static int divideConquer(int[] arr, int lo, int hi) {
        if (lo == hi) return arr[lo];
        int mid = lo + (hi - lo) / 2;
        int leftMax = divideConquer(arr, lo, mid);
        int rightMax = divideConquer(arr, mid + 1, hi);
        int crossMax = maxCrossing(arr, lo, mid, hi);
        return Math.max(leftMax, Math.max(rightMax, crossMax));
    }

    private static int maxCrossing(int[] arr, int lo, int mid, int hi) {
        int leftSum = Integer.MIN_VALUE, sum = 0;
        for (int i = mid; i >= lo; i--) {
            sum += arr[i];
            leftSum = Math.max(leftSum, sum);
        }
        int rightSum = Integer.MIN_VALUE;
        sum = 0;
        for (int i = mid + 1; i <= hi; i++) {
            sum += arr[i];
            rightSum = Math.max(rightSum, sum);
        }
        return leftSum + rightSum;
    }

    public static void main(String[] args) {
        int[] arr = {-2, 1, -3, 4, -1, 2, 1, -5, 4};
        System.out.println(maxSubarray(arr)); // 6
    }
}
```

## Count Inversions

**Python**
```python
def count_inversions(arr):
    def merge_sort(arr):
        if len(arr) <= 1:
            return arr, 0
        mid = len(arr) // 2
        left, inv_left = merge_sort(arr[:mid])
        right, inv_right = merge_sort(arr[mid:])
        merged, inv_cross = merge_count(left, right)
        return merged, inv_left + inv_right + inv_cross

    def merge_count(left, right):
        i = j = inv = 0
        res = []
        while i < len(left) and j < len(right):
            if left[i] <= right[j]:
                res.append(left[i])
                i += 1
            else:
                res.append(right[j])
                inv += len(left) - i
                j += 1
        return res + left[i:] + right[j:], inv

    _, count = merge_sort(arr)
    return count
```

**Java**
```java
import java.util.Arrays;

public class CountInversions {
    public static long countInversions(int[] arr) {
        int[] temp = new int[arr.length];
        return mergeSort(arr, temp, 0, arr.length - 1);
    }

    private static long mergeSort(int[] arr, int[] temp, int l, int r) {
        long inv = 0;
        if (l < r) {
            int mid = l + (r - l) / 2;
            inv += mergeSort(arr, temp, l, mid);
            inv += mergeSort(arr, temp, mid + 1, r);
            inv += merge(arr, temp, l, mid, r);
        }
        return inv;
    }

    private static long merge(int[] arr, int[] temp, int l, int mid, int r) {
        for (int i = l; i <= r; i++) temp[i] = arr[i];
        int i = l, j = mid + 1, k = l;
        long inv = 0;
        while (i <= mid && j <= r) {
            if (temp[i] <= temp[j]) arr[k++] = temp[i++];
            else {
                arr[k++] = temp[j++];
                inv += (mid - i + 1);
            }
        }
        while (i <= mid) arr[k++] = temp[i++];
        while (j <= r) arr[k++] = temp[j++];
        return inv;
    }

    public static void main(String[] args) {
        int[] arr = {2, 4, 1, 3, 5};
        System.out.println(countInversions(arr)); // 3
    }
}
```
