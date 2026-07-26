---
date: 2026-07-20
type: code-patterns
tags:
  - dsa
  - recursion
  - backtracking
  - python
  - java
---

# Recursion & Backtracking — Patterns

## Factorial

**Python**
```python
def factorial(n: int) -> int:
    if n <= 1:
        return 1
    return n * factorial(n - 1)
```

**Java**
```java
public class Factorial {
    public static int factorial(int n) {
        if (n <= 1) return 1;
        return n * factorial(n - 1);
    }

    public static void main(String[] args) {
        System.out.println(factorial(5)); // 120
    }
}
```

## Fibonacci

**Python**
```python
def fib(n: int) -> int:
    if n <= 1:
        return n
    return fib(n - 1) + fib(n - 2)
```

**Java**
```java
public class Fibonacci {
    public static int fib(int n) {
        if (n <= 1) return n;
        return fib(n - 1) + fib(n - 2);
    }

    public static void main(String[] args) {
        System.out.println(fib(6)); // 8
    }
}
```

## Subsets (Power Set)

**Python**
```python
def subsets(nums: list[int]) -> list[list[int]]:
    res = []

    def backtrack(start, path):
        res.append(path[:])
        for i in range(start, len(nums)):
            path.append(nums[i])
            backtrack(i + 1, path)
            path.pop()

    backtrack(0, [])
    return res
```

**Java**
```java
import java.util.*;

public class Subsets {
    public static List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        backtrack(nums, 0, new ArrayList<>(), res);
        return res;
    }

    private static void backtrack(int[] nums, int start, List<Integer> path, List<List<Integer>> res) {
        res.add(new ArrayList<>(path));
        for (int i = start; i < nums.length; i++) {
            path.add(nums[i]);
            backtrack(nums, i + 1, path, res);
            path.remove(path.size() - 1);
        }
    }

    public static void main(String[] args) {
        System.out.println(subsets(new int[]{1, 2})); // [[], [1], [2], [1, 2]]
    }
}
```

## Permutations

**Python**
```python
def permute(nums: list[int]) -> list[list[int]]:
    res = []

    def backtrack(path, used):
        if len(path) == len(nums):
            res.append(path[:])
            return
        for i in range(len(nums)):
            if used[i]:
                continue
            used[i] = True
            path.append(nums[i])
            backtrack(path, used)
            path.pop()
            used[i] = False

    backtrack([], [False] * len(nums))
    return res
```

**Java**
```java
import java.util.*;

public class Permutations {
    public static List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        boolean[] used = new boolean[nums.length];
        backtrack(nums, used, new ArrayList<>(), res);
        return res;
    }

    private static void backtrack(int[] nums, boolean[] used, List<Integer> path, List<List<Integer>> res) {
        if (path.size() == nums.length) {
            res.add(new ArrayList<>(path));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            if (used[i]) continue;
            used[i] = true;
            path.add(nums[i]);
            backtrack(nums, used, path, res);
            path.remove(path.size() - 1);
            used[i] = false;
        }
    }

    public static void main(String[] args) {
        System.out.println(permute(new int[]{1, 2, 3}));
    }
}
```

## Combinations

**Python**
```python
def combine(n: int, k: int) -> list[list[int]]:
    res = []

    def backtrack(start, path):
        if len(path) == k:
            res.append(path[:])
            return
        for i in range(start, n + 1):
            path.append(i)
            backtrack(i + 1, path)
            path.pop()

    backtrack(1, [])
    return res
```

**Java**
```java
import java.util.*;

public class Combinations {
    public static List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> res = new ArrayList<>();
        backtrack(n, k, 1, new ArrayList<>(), res);
        return res;
    }

    private static void backtrack(int n, int k, int start, List<Integer> path, List<List<Integer>> res) {
        if (path.size() == k) {
            res.add(new ArrayList<>(path));
            return;
        }
        for (int i = start; i <= n; i++) {
            path.add(i);
            backtrack(n, k, i + 1, path, res);
            path.remove(path.size() - 1);
        }
    }

    public static void main(String[] args) {
        System.out.println(combine(4, 2));
        // [[1,2], [1,3], [1,4], [2,3], [2,4], [3,4]]
    }
}
```

## N-Queens

**Python**
```python
def solve_n_queens(n: int) -> list[list[str]]:
    cols = set()
    diag1 = set()
    diag2 = set()
    board = [["."] * n for _ in range(n)]
    res = []

    def backtrack(row):
        if row == n:
            res.append(["".join(r) for r in board])
            return
        for col in range(n):
            if col in cols or (row - col) in diag1 or (row + col) in diag2:
                continue
            cols.add(col)
            diag1.add(row - col)
            diag2.add(row + col)
            board[row][col] = "Q"
            backtrack(row + 1)
            board[row][col] = "."
            cols.remove(col)
            diag1.remove(row - col)
            diag2.remove(row + col)

    backtrack(0)
    return res
```

**Java**
```java
import java.util.*;

public class NQueens {
    public static List<List<String>> solveNQueens(int n) {
        Set<Integer> cols = new HashSet<>();
        Set<Integer> diag1 = new HashSet<>();
        Set<Integer> diag2 = new HashSet<>();
        char[][] board = new char[n][n];
        for (char[] row : board) Arrays.fill(row, '.');
        List<List<String>> res = new ArrayList<>();
        backtrack(n, 0, board, cols, diag1, diag2, res);
        return res;
    }

    private static void backtrack(int n, int row, char[][] board,
            Set<Integer> cols, Set<Integer> diag1, Set<Integer> diag2,
            List<List<String>> res) {
        if (row == n) {
            List<String> copy = new ArrayList<>();
            for (char[] r : board) copy.add(new String(r));
            res.add(copy);
            return;
        }
        for (int col = 0; col < n; col++) {
            if (cols.contains(col) || diag1.contains(row - col)
                    || diag2.contains(row + col)) continue;
            cols.add(col); diag1.add(row - col); diag2.add(row + col);
            board[row][col] = 'Q';
            backtrack(n, row + 1, board, cols, diag1, diag2, res);
            board[row][col] = '.';
            cols.remove(col); diag1.remove(row - col); diag2.remove(row + col);
        }
    }

    public static void main(String[] args) {
        System.out.println(solveNQueens(4));
    }
}
```

## Generate Parentheses

**Python**
```python
def generate_parentheses(n: int) -> list[str]:
    res = []

    def backtrack(open_n, close_n, path):
        if open_n == close_n == n:
            res.append(path)
            return
        if open_n < n:
            backtrack(open_n + 1, close_n, path + "(")
        if close_n < open_n:
            backtrack(open_n, close_n + 1, path + ")")

    backtrack(0, 0, "")
    return res
```

**Java**
```java
import java.util.*;

public class GenerateParentheses {
    public static List<String> generateParentheses(int n) {
        List<String> res = new ArrayList<>();
        backtrack(n, 0, 0, "", res);
        return res;
    }

    private static void backtrack(int n, int open, int close, String path, List<String> res) {
        if (open == n && close == n) {
            res.add(path);
            return;
        }
        if (open < n) backtrack(n, open + 1, close, path + "(", res);
        if (close < open) backtrack(n, open, close + 1, path + ")", res);
    }

    public static void main(String[] args) {
        System.out.println(generateParentheses(3));
        // [((())), (()()), (())(), ()(()), ()()()]
    }
}
```
