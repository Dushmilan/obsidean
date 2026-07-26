---
date: 2026-07-20
type: code-patterns
tags:
  - dsa
  - greedy
  - python
  - java
---

# Greedy — Patterns

## Activity Selection

**Python**
```python
def activity_selection(start, end):
    n = len(start)
    activities = sorted(zip(start, end), key=lambda x: x[1])
    count = 1
    last_end = activities[0][1]
    for s, e in activities[1:]:
        if s >= last_end:
            count += 1
            last_end = e
    return count
```

**Java**
```java
import java.util.*;

public class ActivitySelection {
    public static int activitySelection(int[] start, int[] end) {
        int n = start.length;
        int[][] activities = new int[n][2];
        for (int i = 0; i < n; i++) {
            activities[i][0] = start[i];
            activities[i][1] = end[i];
        }
        Arrays.sort(activities, (a, b) -> a[1] - b[1]);
        int count = 1, lastEnd = activities[0][1];
        for (int i = 1; i < n; i++) {
            if (activities[i][0] >= lastEnd) {
                count++;
                lastEnd = activities[i][1];
            }
        }
        return count;
    }

    public static void main(String[] args) {
        int[] start = {1, 3, 0, 5, 8, 5};
        int[] end   = {2, 4, 6, 7, 9, 9};
        System.out.println(activitySelection(start, end)); // 4
    }
}
```

## Coin Change (Greedy)

**Python**
```python
def coin_change_greedy(coins, amount):
    coins.sort(reverse=True)
    count = 0
    for coin in coins:
        if amount == 0:
            break
        count += amount // coin
        amount %= coin
    return count if amount == 0 else -1
```

**Java**
```java
import java.util.Arrays;

public class CoinChangeGreedy {
    public static int coinChangeGreedy(int[] coins, int amount) {
        Arrays.sort(coins);
        int count = 0;
        for (int i = coins.length - 1; i >= 0; i--) {
            if (amount == 0) break;
            count += amount / coins[i];
            amount %= coins[i];
        }
        return amount == 0 ? count : -1;
    }

    public static void main(String[] args) {
        System.out.println(coinChangeGreedy(new int[]{1, 5, 10, 25}, 63)); // 6
    }
}
```

## Fractional KnapSack

**Python**
```python
def fractional_knapsack(values, weights, capacity):
    items = [(v / w, v, w) for v, w in zip(values, weights)]
    items.sort(reverse=True)
    total_value = 0.0
    for ratio, v, w in items:
        if capacity >= w:
            total_value += v
            capacity -= w
        else:
            total_value += ratio * capacity
            break
    return total_value
```

**Java**
```java
import java.util.Arrays;

public class FractionalKnapsack {
    public static double fractionalKnapsack(int[] values, int[] weights, int capacity) {
        int n = values.length;
        double[][] items = new double[n][3];
        for (int i = 0; i < n; i++) {
            items[i][0] = (double) values[i] / weights[i];
            items[i][1] = values[i];
            items[i][2] = weights[i];
        }
        Arrays.sort(items, (a, b) -> Double.compare(b[0], a[0]));
        double total = 0;
        for (int i = 0; i < n; i++) {
            if (capacity >= items[i][2]) {
                total += items[i][1];
                capacity -= items[i][2];
            } else {
                total += items[i][0] * capacity;
                break;
            }
        }
        return total;
    }

    public static void main(String[] args) {
        int[] values = {60, 100, 120};
        int[] weights = {10, 20, 30};
        System.out.println(fractionalKnapsack(values, weights, 50)); // 240
    }
}
```

## Jump Game

**Python**
```python
def can_jump(nums):
    max_reach = 0
    for i, n in enumerate(nums):
        if i > max_reach:
            return False
        max_reach = max(max_reach, i + n)
    return True
```

**Java**
```java
public class JumpGame {
    public static boolean canJump(int[] nums) {
        int maxReach = 0;
        for (int i = 0; i < nums.length; i++) {
            if (i > maxReach) return false;
            maxReach = Math.max(maxReach, i + nums[i]);
        }
        return true;
    }

    public static void main(String[] args) {
        System.out.println(canJump(new int[]{2, 3, 1, 1, 4})); // true
        System.out.println(canJump(new int[]{3, 2, 1, 0, 4})); // false
    }
}
```

## Minimum Platforms

**Python**
```python
def min_platforms(arrival, departure):
    arrival.sort()
    departure.sort()
    i = j = 0
    platforms = max_platforms = 0
    while i < len(arrival):
        if arrival[i] <= departure[j]:
            platforms += 1
            max_platforms = max(max_platforms, platforms)
            i += 1
        else:
            platforms -= 1
            j += 1
    return max_platforms
```

**Java**
```java
import java.util.Arrays;

public class MinPlatforms {
    public static int minPlatforms(int[] arrival, int[] departure) {
        Arrays.sort(arrival);
        Arrays.sort(departure);
        int i = 0, j = 0, platforms = 0, maxPlatforms = 0;
        while (i < arrival.length) {
            if (arrival[i] <= departure[j]) {
                platforms++;
                maxPlatforms = Math.max(maxPlatforms, platforms);
                i++;
            } else {
                platforms--;
                j++;
            }
        }
        return maxPlatforms;
    }

    public static void main(String[] args) {
        int[] arrival = {900, 940, 950, 1100, 1500, 1800};
        int[] departure = {910, 1200, 1120, 1130, 1900, 2000};
        System.out.println(minPlatforms(arrival, departure)); // 3
    }
}
```
