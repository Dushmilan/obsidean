---
date: 2026-07-20
type: code-patterns
tags:
  - dsa
  - hash-tables
  - python
  - java
---

# Hash Tables — Patterns

## Frequency Counter

**Python**
```python
from collections import Counter, defaultdict

def freq_count(arr: list) -> dict:
    return dict(Counter(arr))

def freq_count_manual(arr: list) -> dict:
    freq = {}
    for x in arr:
        freq[x] = freq.get(x, 0) + 1
    return freq

# With defaultdict
def freq_count_dd(arr: list) -> dict:
    freq = defaultdict(int)
    for x in arr:
        freq[x] += 1
    return dict(freq)
```

**Java**
```java
import java.util.HashMap;
import java.util.Map;

public class FreqCounter {
    public static Map<Integer, Integer> freqCount(int[] arr) {
        Map<Integer, Integer> freq = new HashMap<>();
        for (int x : arr) {
            freq.put(x, freq.getOrDefault(x, 0) + 1);
        }
        return freq;
    }

    public static void main(String[] args) {
        int[] arr = {1, 2, 2, 3, 3, 3};
        System.out.println(freqCount(arr)); // {1=1, 2=2, 3=3}
    }
}
```

## Two Sum with Hash Map

**Python**
```python
def two_sum(arr: list[int], target: int) -> list[int]:
    seen = {}
    for i, val in enumerate(arr):
        comp = target - val
        if comp in seen:
            return [seen[comp], i]
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
            int comp = target - arr[i];
            if (seen.containsKey(comp)) {
                return new int[]{seen.get(comp), i};
            }
            seen.put(arr[i], i);
        }
        return new int[]{};
    }

    public static void main(String[] args) {
        int[] arr = {2, 7, 11, 15};
        int[] res = twoSum(arr, 9);
        System.out.println(res[0] + " " + res[1]); // 0 1
    }
}
```

## Contains Duplicate

**Python**
```python
def contains_duplicate(arr: list[int]) -> bool:
    seen = set()
    for x in arr:
        if x in seen:
            return True
        seen.add(x)
    return False
```

**Java**
```java
import java.util.HashSet;
import java.util.Set;

public class ContainsDuplicate {
    public static boolean containsDuplicate(int[] arr) {
        Set<Integer> seen = new HashSet<>();
        for (int x : arr) {
            if (seen.contains(x)) return true;
            seen.add(x);
        }
        return false;
    }

    public static void main(String[] args) {
        System.out.println(containsDuplicate(new int[]{1,2,3,1})); // true
        System.out.println(containsDuplicate(new int[]{1,2,3,4})); // false
    }
}
```

## Intersection of Two Arrays

**Python**
```python
def intersection(arr1: list[int], arr2: list[int]) -> list[int]:
    set1 = set(arr1)
    set2 = set(arr2)
    return list(set1 & set2)
```

**Java**
```java
import java.util.HashSet;
import java.util.Set;
import java.util.ArrayList;
import java.util.List;

public class Intersection {
    public static int[] intersection(int[] nums1, int[] nums2) {
        Set<Integer> set1 = new HashSet<>();
        for (int x : nums1) set1.add(x);
        Set<Integer> result = new HashSet<>();
        for (int x : nums2) {
            if (set1.contains(x)) result.add(x);
        }
        return result.stream().mapToInt(i -> i).toArray();
    }

    public static void main(String[] args) {
        int[] res = intersection(new int[]{1,2,2,1}, new int[]{2,2});
        // [2]
    }
}
```

## Subarray Sum Equals K

**Python**
```python
def subarray_sum(arr: list[int], k: int) -> int:
    prefix_sum = 0
    count = 0
    seen = {0: 1}
    for x in arr:
        prefix_sum += x
        count += seen.get(prefix_sum - k, 0)
        seen[prefix_sum] = seen.get(prefix_sum, 0) + 1
    return count
```

**Java**
```java
import java.util.HashMap;
import java.util.Map;

public class SubarraySumK {
    public static int subarraySum(int[] arr, int k) {
        Map<Integer, Integer> seen = new HashMap<>();
        seen.put(0, 1);
        int prefix = 0, count = 0;
        for (int x : arr) {
            prefix += x;
            count += seen.getOrDefault(prefix - k, 0);
            seen.put(prefix, seen.getOrDefault(prefix, 0) + 1);
        }
        return count;
    }

    public static void main(String[] args) {
        System.out.println(subarraySum(new int[]{1,1,1}, 2)); // 2
    }
}
```

## Hash Set Usage

**Python**
```python
hs = set()
hs.add(1)
hs.add(2)
exists = 1 in hs
hs.remove(1)
size = len(hs)
```

**Java**
```java
import java.util.HashSet;
import java.util.Set;

public class HashSetDemo {
    public static void main(String[] args) {
        Set<Integer> hs = new HashSet<>();
        hs.add(1);
        hs.add(2);
        boolean exists = hs.contains(1);
        hs.remove(1);
        int size = hs.size();
    }
}
```
