---
date: 2026-07-20
type: code-patterns
tags:
  - dsa
  - bit-manipulation
  - python
  - java
---

# Bit Manipulation — Patterns

## Check if Bit is Set

**Python**
```python
def is_bit_set(n: int, pos: int) -> bool:
    return (n >> pos) & 1 == 1
```

**Java**
```java
public class CheckBit {
    public static boolean isBitSet(int n, int pos) {
        return ((n >> pos) & 1) == 1;
    }

    public static void main(String[] args) {
        System.out.println(isBitSet(5, 0)); // true (101)
        System.out.println(isBitSet(5, 1)); // false
    }
}
```

## Set / Clear / Toggle Bit

**Python**
```python
def set_bit(n: int, pos: int) -> int:
    return n | (1 << pos)

def clear_bit(n: int, pos: int) -> int:
    return n & ~(1 << pos)

def toggle_bit(n: int, pos: int) -> int:
    return n ^ (1 << pos)
```

**Java**
```java
public class BitOps {
    public static int setBit(int n, int pos) {
        return n | (1 << pos);
    }

    public static int clearBit(int n, int pos) {
        return n & ~(1 << pos);
    }

    public static int toggleBit(int n, int pos) {
        return n ^ (1 << pos);
    }

    public static void main(String[] args) {
        int n = 5; // 101
        System.out.println(setBit(n, 1));    // 7 (111)
        System.out.println(clearBit(n, 2));  // 1 (001)
        System.out.println(toggleBit(n, 2)); // 1 (001)
    }
}
```

## Count Set Bits (Brian Kernighan's)

**Python**
```python
def count_set_bits(n: int) -> int:
    count = 0
    while n:
        n &= n - 1  # clears the lowest set bit
        count += 1
    return count
```

**Java**
```java
public class CountSetBits {
    public static int countSetBits(int n) {
        int count = 0;
        while (n != 0) {
            n &= (n - 1);
            count++;
        }
        return count;
    }

    public static void main(String[] args) {
        System.out.println(countSetBits(13)); // 3 (1101)
    }
}
```

## Power of Two Check

**Python**
```python
def is_power_of_two(n: int) -> bool:
    return n > 0 and (n & (n - 1)) == 0
```

**Java**
```java
public class PowerOfTwo {
    public static boolean isPowerOfTwo(int n) {
        return n > 0 && (n & (n - 1)) == 0;
    }

    public static void main(String[] args) {
        System.out.println(isPowerOfTwo(16)); // true
        System.out.println(isPowerOfTwo(18)); // false
    }
}
```

## Find the Single Non-Repeating Element

**Python**
```python
def single_number(nums: list[int]) -> int:
    xor = 0
    for x in nums:
        xor ^= x
    return xor
```

**Java**
```java
public class SingleNumber {
    public static int singleNumber(int[] nums) {
        int xor = 0;
        for (int x : nums) xor ^= x;
        return xor;
    }

    public static void main(String[] args) {
        int[] nums = {4, 1, 2, 1, 2};
        System.out.println(singleNumber(nums)); // 4
    }
}
```

## Two Non-Repeating Elements

**Python**
```python
def two_single_numbers(nums: list[int]) -> list[int]:
    xor = 0
    for x in nums:
        xor ^= x

    # Find rightmost set bit
    diff = xor & -xor
    a = b = 0
    for x in nums:
        if x & diff:
            a ^= x
        else:
            b ^= x
    return [a, b]
```

**Java**
```java
import java.util.Arrays;

public class TwoSingleNumbers {
    public static int[] twoSingleNumbers(int[] nums) {
        int xor = 0;
        for (int x : nums) xor ^= x;
        int diff = xor & -xor;
        int a = 0, b = 0;
        for (int x : nums) {
            if ((x & diff) != 0) a ^= x;
            else b ^= x;
        }
        return new int[]{a, b};
    }

    public static void main(String[] args) {
        int[] nums = {1, 2, 1, 3, 2, 5};
        System.out.println(Arrays.toString(twoSingleNumbers(nums))); // [3, 5]
    }
}
```

## Subsets via Bitmask

**Python**
```python
def subsets_bitmask(nums: list[int]) -> list[list[int]]:
    n = len(nums)
    res = []
    for mask in range(1 << n):
        subset = []
        for i in range(n):
            if mask & (1 << i):
                subset.append(nums[i])
        res.append(subset)
    return res
```

**Java**
```java
import java.util.*;

public class SubsetsBitmask {
    public static List<List<Integer>> subsets(int[] nums) {
        int n = nums.length;
        List<List<Integer>> res = new ArrayList<>();
        for (int mask = 0; mask < (1 << n); mask++) {
            List<Integer> subset = new ArrayList<>();
            for (int i = 0; i < n; i++) {
                if ((mask & (1 << i)) != 0) {
                    subset.add(nums[i]);
                }
            }
            res.add(subset);
        }
        return res;
    }

    public static void main(String[] args) {
        System.out.println(subsets(new int[]{1, 2}));
        // [[], [1], [2], [1, 2]]
    }
}
```
