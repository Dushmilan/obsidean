---
date: 2026-07-20
type: code-patterns
tags:
  - dsa
  - sliding-window
  - python
  - java
---

# Sliding Window — Patterns

## Fixed Size — Maximum Sum

**Python**
```python
def max_sum_fixed(arr, k):
    n = len(arr)
    if n < k: return -1
    window_sum = sum(arr[:k])
    max_sum = window_sum
    for i in range(n - k):
        window_sum = window_sum - arr[i] + arr[i + k]
        max_sum = max(max_sum, window_sum)
    return max_sum
```

**Java**
```java
public class MaxSumFixed {
    public static int maxSumFixed(int[] arr, int k) {
        int n = arr.length;
        if (n < k) return -1;
        int windowSum = 0;
        for (int i = 0; i < k; i++) windowSum += arr[i];
        int maxSum = windowSum;
        for (int i = k; i < n; i++) {
            windowSum = windowSum - arr[i - k] + arr[i];
            maxSum = Math.max(maxSum, windowSum);
        }
        return maxSum;
    }

    public static void main(String[] args) {
        int[] arr = {5, 2, -1, 0, 3};
        System.out.println(maxSumFixed(arr, 3)); // 6
    }
}
```

## Variable Size — Longest Substring Without Repeating

**Python**
```python
def length_of_longest_substring(s: str) -> int:
    seen = {}
    left = max_len = 0
    for right, ch in enumerate(s):
        if ch in seen and seen[ch] >= left:
            left = seen[ch] + 1
        seen[ch] = right
        max_len = max(max_len, right - left + 1)
    return max_len
```

**Java**
```java
import java.util.HashMap;
import java.util.Map;

public class LongestSubstring {
    public static int lengthOfLongestSubstring(String s) {
        Map<Character, Integer> seen = new HashMap<>();
        int left = 0, maxLen = 0;
        for (int right = 0; right < s.length(); right++) {
            char ch = s.charAt(right);
            if (seen.containsKey(ch) && seen.get(ch) >= left) {
                left = seen.get(ch) + 1;
            }
            seen.put(ch, right);
            maxLen = Math.max(maxLen, right - left + 1);
        }
        return maxLen;
    }

    public static void main(String[] args) {
        System.out.println(lengthOfLongestSubstring("abcabcbb")); // 3
    }
}
```

## Variable Size — Minimum Window Substring

**Python**
```python
from collections import Counter

def min_window(s: str, t: str) -> str:
    need = Counter(t)
    have = 0
    need_count = len(need)
    left = 0
    res, res_len = "", float('inf')

    for right, ch in enumerate(s):
        if ch in need:
            need[ch] -= 1
            if need[ch] == 0:
                have += 1

        while have == need_count:
            if right - left + 1 < res_len:
                res = s[left:right + 1]
                res_len = right - left + 1
            left_ch = s[left]
            if left_ch in need:
                if need[left_ch] == 0:
                    have -= 1
                need[left_ch] += 1
            left += 1

    return res
```

**Java**
```java
import java.util.HashMap;
import java.util.Map;

public class MinWindowSubstring {
    public static String minWindow(String s, String t) {
        Map<Character, Integer> need = new HashMap<>();
        for (char ch : t.toCharArray()) {
            need.put(ch, need.getOrDefault(ch, 0) + 1);
        }
        int have = 0, needCount = need.size();
        int left = 0, resLen = Integer.MAX_VALUE;
        String res = "";

        for (int right = 0; right < s.length(); right++) {
            char ch = s.charAt(right);
            if (need.containsKey(ch)) {
                need.put(ch, need.get(ch) - 1);
                if (need.get(ch) == 0) have++;
            }

            while (have == needCount) {
                if (right - left + 1 < resLen) {
                    res = s.substring(left, right + 1);
                    resLen = right - left + 1;
                }
                char leftCh = s.charAt(left);
                if (need.containsKey(leftCh)) {
                    if (need.get(leftCh) == 0) have--;
                    need.put(leftCh, need.get(leftCh) + 1);
                }
                left++;
            }
        }
        return res;
    }

    public static void main(String[] args) {
        System.out.println(minWindow("ADOBECODEBANC", "ABC")); // "BANC"
    }
}
```

## Fixed Size — Count Anagrams

**Python**
```python
from collections import Counter

def count_anagrams(s: str, pattern: str) -> int:
    need = Counter(pattern)
    have = 0
    need_count = len(need)
    k = len(pattern)
    count = 0

    for i, ch in enumerate(s):
        if ch in need:
            need[ch] -= 1
            if need[ch] == 0:
                have += 1
        if i >= k:
            left_ch = s[i - k]
            if left_ch in need:
                if need[left_ch] == 0:
                    have -= 1
                need[left_ch] += 1
        if have == need_count:
            count += 1

    return count
```

**Java**
```java
import java.util.HashMap;
import java.util.Map;

public class CountAnagrams {
    public static int countAnagrams(String s, String pattern) {
        Map<Character, Integer> need = new HashMap<>();
        for (char ch : pattern.toCharArray()) {
            need.put(ch, need.getOrDefault(ch, 0) + 1);
        }
        int have = 0, needCount = need.size();
        int k = pattern.length(), count = 0;

        for (int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);
            if (need.containsKey(ch)) {
                need.put(ch, need.get(ch) - 1);
                if (need.get(ch) == 0) have++;
            }
            if (i >= k) {
                char leftCh = s.charAt(i - k);
                if (need.containsKey(leftCh)) {
                    if (need.get(leftCh) == 0) have--;
                    need.put(leftCh, need.get(leftCh) + 1);
                }
            }
            if (have == needCount) count++;
        }
        return count;
    }

    public static void main(String[] args) {
        System.out.println(countAnagrams("cbaebabacd", "abc")); // 2
    }
}
```

## Variable Size — Longest Substring with K Distinct

**Python**
```python
def longest_k_distinct(s: str, k: int) -> int:
    freq = {}
    left = max_len = 0
    for right, ch in enumerate(s):
        freq[ch] = freq.get(ch, 0) + 1
        while len(freq) > k:
            left_ch = s[left]
            freq[left_ch] -= 1
            if freq[left_ch] == 0:
                del freq[left_ch]
            left += 1
        max_len = max(max_len, right - left + 1)
    return max_len
```

**Java**
```java
import java.util.HashMap;
import java.util.Map;

public class LongestKDistinct {
    public static int longestKDistinct(String s, int k) {
        Map<Character, Integer> freq = new HashMap<>();
        int left = 0, maxLen = 0;
        for (int right = 0; right < s.length(); right++) {
            char ch = s.charAt(right);
            freq.put(ch, freq.getOrDefault(ch, 0) + 1);
            while (freq.size() > k) {
                char leftCh = s.charAt(left);
                freq.put(leftCh, freq.get(leftCh) - 1);
                if (freq.get(leftCh) == 0) freq.remove(leftCh);
                left++;
            }
            maxLen = Math.max(maxLen, right - left + 1);
        }
        return maxLen;
    }

    public static void main(String[] args) {
        System.out.println(longestKDistinct("eceba", 2)); // 3 (ece)
    }
}
```
