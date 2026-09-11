# Strings & Arrays

Strings and arrays are Java's workhorse containers. Strings are immutable with a rich API; arrays are fixed-size contiguous blocks with runtime bounds checks. Between them they handle most "collection of things" work before you reach the Collections Framework.

**The Intuition:** A String is an immutable char[] with methods. Every "change" returns a new String — which makes string manipulation in loops expensive unless you use StringBuilder. An array is a fixed-size container; accessing `arr[i]` is checked at runtime (throws `ArrayIndexOutOfBoundsException` instead of corrupting memory like C).

## Strings — the immutable workhorse

```java
String s = "Hello";
s.length()                  // 5 — method, NOT a field (unlike arrays)
s.charAt(0)                 // 'H'
s.substring(1, 4)           // "ell" — [start, end), end exclusive
s.substring(2)              // "llo" — to the end
s.indexOf('l')              // 2 — first index, or -1
s.lastIndexOf('l')          // 3
s.indexOf("ll")             // 2 — substring search
s.contains("ell")           // true
s.startsWith("He")          // true
s.endsWith("lo")            // true
s.toUpperCase()             // "HELLO"
s.toLowerCase()
s.trim()                    // strips leading/trailing whitespace
s.replace('l', 'L')         // "HeLLo"
s.replace("ll", "rr")       // "Herro"
s.split(" ")                // String[] — splits on regex!
s.isEmpty()                 // false — length == 0
s.isBlank()                 // true if only whitespace (Java 11+)
"abc".repeat(3)             // "abcabcabc" (Java 11+)
```

## Comparing strings

```java
s1.equals(s2)               // content equality — THE way
s1.equalsIgnoreCase(s2)
s1.compareTo(s2)            // lexicographic: <0, 0, >0
s1.compareToIgnoreCase(s2)

// == compares references — almost always wrong for strings
```

## Building strings — the O(n²) trap

```java
// BAD — String is immutable, so + in a loop creates a NEW string each time:
String result = "";
for (int i = 0; i < 100000; i++) {
    result += i;        // O(n²) — copies the whole string every iteration!
}

// GOOD — StringBuilder mutates in place:
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 100000; i++) {
    sb.append(i);
}
String result = sb.toString();     // O(n)

// StringBuilder API:
sb.append(x)            // anything — converts to string
sb.insert(0, "pre")
sb.reverse()
sb.delete(2, 5)
sb.length(), sb.charAt(i)
```

**Rule:** 3+ concatenations in a loop → StringBuilder. The compiler even uses it for `+` chains of literals, but not inside loops.

## Arrays

```java
// Declaration & initialization
int[] arr = new int[5];           // all zeros
int[] nums = {1, 2, 3};           // literal — size inferred
String[] names = new String[3];   // all null!
int[][] matrix = new int[3][4];   // 2D: 3 rows × 4 cols

// Properties — THE array gotchas
nums.length               // FIELD (no parens) — 3
nums[0]                   // 1 — zero-based
nums[nums.length - 1]     // last element
nums[5]                   // ArrayIndexOutOfBoundsException — CHECKED at runtime!

// Iteration
for (int i = 0; i < nums.length; i++) { ... }
for (int x : nums) { ... }              // enhanced for — read-only view

// Copying — arrays are references!
int[] a = {1, 2, 3};
int[] b = a;               // SAME array — b[0] = 99 changes a!
int[] c = a.clone();       // a real copy
int[] d = Arrays.copyOf(a, 5);   // grow/shrink: {1,2,3,0,0}

// java.util.Arrays — the toolkit
Arrays.sort(nums)                 // in place
Arrays.binarySearch(sorted, key)  // index or negative
Arrays.fill(arr, 0)
Arrays.toString(nums)             // "[1, 2, 3]" — printing arrays correctly
Arrays.equals(a, b)               // content equality (== compares refs!)
Arrays.asList(1, 2, 3)            // fixed-size List view
```

## The `Arrays.toString` trap

```java
System.out.println(nums);
// [I@1b6d3586  — the default toString of an array is "type@hashcode"!

System.out.println(Arrays.toString(nums));
// [1, 2, 3] — the right way
```

## Char arrays & String conversion

```java
char[] chars = "hello".toCharArray();    // String → char[]
String s = new String(chars);            // char[] → String
String s = String.valueOf(chars);

// String ↔ int
int n = Integer.parseInt("42");          // throws NumberFormatException on bad input
String s = Integer.toString(42);
// Long.parseLong, Double.parseDouble, etc.
```

## 2D arrays — arrays of arrays

```java
int[][] grid = new int[3][4];     // rectangular
grid.length          // 3 — number of rows
grid[0].length       // 4 — length of first row

// Ragged arrays — rows can differ in length:
int[][] ragged = new int[3][];
ragged[0] = new int[2];
ragged[1] = new int[5];
ragged[2] = new int[1];

// Iterate
for (int[] row : grid) {
    for (int x : row) { ... }
}
```

---

**Setup:** Count word frequencies in a sentence, using a HashMap.

**Solution:**
```java
String text = "the cat and the dog and the bird";
Map<String, Integer> counts = new HashMap<>();
for (String word : text.split(" ")) {
    counts.put(word, counts.getOrDefault(word, 0) + 1);
}
// {the=3, cat=1, and=2, dog=1, bird=1}
```

**Key insight:** `getOrDefault(word, 0)` avoids the explicit null check — the idiomatic Java counter. `split(" ")` (regex) divides on spaces.

---

**Setup:** Reverse a string — three ways.

**Solution:**
```java
String reverse(String s) {
    return new StringBuilder(s).reverse().toString();
}
// Or two-pointer on char[], or:
String reversed = new String(new StringBuilder(s).reverse());
```

**Key insight:** StringBuilder's `reverse()` is O(n) and in-place. Doing it by hand (char[] + two pointers) is the DSA exercise; using the API is the production answer.

---

**Setup:** Check if a string is an anagram of another.

**Solution:**
```java
boolean isAnagram(String a, String b) {
    char[] ca = a.toCharArray();
    char[] cb = b.toCharArray();
    Arrays.sort(ca);
    Arrays.sort(cb);
    return Arrays.equals(ca, cb);
}
```

**Key insight:** Sorting both and comparing — O(n log n). The hash-map counting version is O(n). `Arrays.equals` compares *contents* (crucial — `==` would compare array references).

---

**Setup:** Find the most frequent element in an array.

**Solution:**
```java
int mostFrequent(int[] arr) {
    Map<Integer, Integer> counts = new HashMap<>();
    for (int x : arr) counts.put(x, counts.getOrDefault(x, 0) + 1);
    int best = arr[0], bestCount = 0;
    for (Map.Entry<Integer, Integer> e : counts.entrySet()) {
        if (e.getValue() > bestCount) { best = e.getKey(); bestCount = e.getValue(); }
    }
    return best;
}
```

**Key insight:** The HashMap count + single scan for the max — O(n) time. `entrySet()` iterates key/value pairs efficiently.

---

## Practice (try before peeking)

1. `String s = "Hello";` then `s.toUpperCase();` — what's s now?
2. `int[] a = {1,2}; int[] b = a; b[0] = 9;` — what is `a[0]`?
3. `"abc".substring(1, 2)` — what does it return?

<details><summary>Answers</summary>

1. Still `"Hello"` — Strings are immutable; `toUpperCase()` *returned* a new string that you ignored. Capture it: `s = s.toUpperCase();`.
2. `9` — `b = a` made b an alias of the *same* array; the element changed through b.
3. `"b"` — substring is [start, end): chars at index 1 only.

</details>

---

**Common traps:**
- `s.length()` (method) vs `arr.length` (field) — mixing them is a compile error that confuses beginners
- `Arrays.toString(arr)` for printing; `arr.toString()` gives `[I@...`
- `==` vs `.equals()` on strings and arrays
- String concatenation in loops — use StringBuilder
- `split` takes a **regex** — `"."` splits on *any* character, not a literal dot (use `"\\."`)

---
