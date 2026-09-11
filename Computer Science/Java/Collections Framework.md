# Collections Framework

Java's `java.util` collections are production-tested data structures — the exact list/set/map implementations from your DSA notes, ready to use. The framework is built on interfaces (`List`, `Set`, `Map`) with multiple implementations, so you pick by *behavior* and *complexity*.

**The Intuition:** `List` is the interface (ordered, indexed, allows duplicates); `ArrayList` and `LinkedList` are implementations with different trade-offs. `Set` is uniqueness; `Map` is key→value. Program against the *interface* (`List<String>`) so you can swap implementations without touching the rest of your code.

## The interface hierarchy

```
Collection (Iterable)
├── List          — ordered, indexed, duplicates OK
│   ├── ArrayList     — dynamic array
│   └── LinkedList    — doubly linked
├── Set           — unique elements
│   ├── HashSet       — hash table, O(1)
│   ├── LinkedHashSet — insertion order + hash
│   └── TreeSet       — sorted, O(log n)
└── Queue/Deque   — FIFO / double-ended
    ├── ArrayDeque    — fast, preferred
    └── LinkedList    — also a queue

Map (NOT a Collection)
├── HashMap        — hash table, O(1)
├── LinkedHashMap  — insertion order
└── TreeMap        — sorted keys, O(log n)
```

## Choosing by complexity

| Operation | ArrayList | LinkedList | HashSet | TreeSet | HashMap | TreeMap |
|-----------|-----------|-----------|---------|---------|---------|---------|
| get/contains | O(1) | O(n) | O(1) | O(log n) | O(1) | O(log n) |
| add | O(1) amortized | O(1) at ends | O(1) | O(log n) | O(1) | O(log n) |
| remove | O(n) | O(1) at ends | O(1) | O(log n) | O(1) | O(log n) |
| ordered | index | index | no | sorted | no | sorted |

**Golden rule:** Need fast lookup by key → `HashMap`. Need uniqueness + fast contains → `HashSet`. Need order → `ArrayList`. Need sorted iteration → `TreeSet`/`TreeMap`.

## List — the workhorse

```java
List<String> names = new ArrayList<>();

// Add / get / remove
names.add("Ada");               // add at end
names.add(0, "Zed");            // insert at index (shifts — O(n))
names.get(0);                   // "Zed"
names.set(0, "Alice");          // replace
names.remove(0);                // by index
names.remove("Ada");            // by value (first occurrence)
names.size();                   // count (method, not field!)
names.contains("Ada");          // O(n) for ArrayList
names.indexOf("Ada");           // index or -1
names.isEmpty();

// Iteration
for (String s : names) { ... }
for (int i = 0; i < names.size(); i++) { ... }
names.forEach(s -> System.out.println(s));
names.stream().filter(s -> s.length() > 3).toList();

// Init helpers
List<String> fixed = List.of("a", "b", "c");     // IMMUTABLE — add() throws!
List<String> mutable = new ArrayList<>(List.of("a", "b", "c"));
List<Integer> nums = new ArrayList<>(Arrays.asList(1, 2, 3));
```

## Set — uniqueness

```java
Set<Integer> nums = new HashSet<>();
nums.add(5);
nums.add(5);             // ignored — already present
nums.size();             // 1
nums.contains(5);        // true — O(1)
nums.remove(5);

// Dedupe a list:
List<Integer> dupes = List.of(1, 2, 2, 3, 3);
List<Integer> unique = new ArrayList<>(new HashSet<>(dupes));   // [1,2,3]

// Set algebra:
Set<Integer> a = new HashSet<>(List.of(1, 2, 3));
Set<Integer> b = new HashSet<>(List.of(2, 3, 4));
a.retainAll(b);    // a = intersection {2,3}
a.addAll(b);       // union
a.removeAll(b);    // difference

// LinkedHashSet preserves insertion order; TreeSet is sorted.
```

## Map — key → value

```java
Map<String, Integer> scores = new HashMap<>();
scores.put("Ada", 92);
scores.put("Bob", 85);
scores.get("Ada");                  // 92
scores.get("Cy");                   // null — missing key returns null
scores.getOrDefault("Cy", 0);       // 0 — the safe way
scores.containsKey("Bob");          // true
scores.containsValue(92);           // true — O(n), rare
scores.remove("Bob");
scores.size(), scores.isEmpty();
scores.keySet();                    // Set<String> — all keys
scores.values();                    // Collection<Integer>
scores.entrySet();                  // Set<Map.Entry<...>> — the iteration way

// Iterate
for (Map.Entry<String, Integer> e : scores.entrySet()) {
    System.out.println(e.getKey() + " = " + e.getValue());
}
// Or with lambdas:
scores.forEach((k, v) -> System.out.println(k + " = " + v));

// merge / compute (Java 8+) — atomic update patterns
scores.merge("Cy", 1, Integer::sum);   // count-up idiom
```

## Queue & Deque

```java
Queue<String> q = new ArrayDeque<>();
q.offer("a");          // enqueue — use offer, not add (no exception)
q.poll();              // dequeue — null if empty (not remove, which throws)
q.peek();              // look, don't remove

Deque<String> stack = new ArrayDeque<>();
stack.push("a");       // stack.push/pop — LIFO
stack.pop();
// ArrayDeque is the modern choice for both stack and queue
// (LinkedList also works but with more overhead)
```

## Sorting & searching

```java
List<Integer> nums = new ArrayList<>(List.of(3, 1, 2));
Collections.sort(nums);                     // ascending
nums.sort(Comparator.naturalOrder());
nums.sort(Comparator.reverseOrder());
nums.sort(Comparator.comparingInt(...));    // by extracted key
nums.sort(Comparator.comparing(Person::age).thenComparing(Person::name));

Collections.max(nums);  Collections.min(nums);
Collections.frequency(nums, 3);             // count occurrences
Collections.reverse(nums);
Collections.shuffle(nums);
```

---

**Setup:** Count word frequencies and print the top 3.

**Solution:**
```java
String text = "the cat and the dog and the bird";
Map<String, Integer> counts = new HashMap<>();
for (String w : text.split(" ")) {
    counts.merge(w, 1, Integer::sum);
}

counts.entrySet().stream()
    .sorted(Map.Entry.<String, Integer>comparingByValue().reversed())
    .limit(3)
    .forEach(e -> System.out.println(e.getKey() + ": " + e.getValue()));
// the: 3, and: 2, cat: 1 (or dog/bird — tie order unspecified)
```

**Key insight:** `merge(w, 1, Integer::sum)` is the clean counter — insert 1 if absent, else add. The stream pipeline sorts, truncates, prints — all declarative.

---

**Setup:** Remove duplicates from a list, preserving order.

**Solution:**
```java
List<Integer> input = List.of(3, 1, 3, 2, 1);
Set<Integer> seen = new LinkedHashSet<>(input);   // insertion-ordered dedupe
List<Integer> result = new ArrayList<>(seen);     // [3, 1, 2]
```

**Key insight:** `LinkedHashSet` keeps first-occurrence order — order-preserving dedupe in one line. Plain `HashSet` would lose order; `TreeSet` would sort.

---

**Setup:** The most frequent element in an array.

**Solution:**
```java
int mostFrequent(int[] arr) {
    Map<Integer, Integer> counts = new HashMap<>();
    for (int x : arr) counts.merge(x, 1, Integer::sum);
    int best = arr[0], bestCount = 0;
    for (var e : counts.entrySet()) {
        if (e.getValue() > bestCount) { best = e.getKey(); bestCount = e.getValue(); }
    }
    return best;
}
```

**Key insight:** One pass to count (O(n)), one pass over entries to find the max — same algorithm as the DSA HashMap pattern. `var` infers `Map.Entry<Integer,Integer>`.

---

**Setup:** Why does `List.of("a","b")` throw `UnsupportedOperationException` on `.add()`?

**Solution:** `List.of` returns an **immutable** list — a fixed-size, unmodifiable view. It's optimized (no growth logic) and safe (can't be mutated). To change it, copy first: `new ArrayList<>(List.of("a","b"))`.

**Key insight:** "Immutable" collections are a *different contract* — they document intent (a constant list) and prevent accidental mutation bugs. Know which factory you're using.

---

## Practice (try before peeking)

1. Which collection gives O(1) `contains()`?
2. `HashMap` vs `TreeMap` — when does TreeMap win?
3. What's the difference between `remove()` and `poll()` on a Queue?

<details><summary>Answers</summary>

1. `HashSet` (and `HashMap` keys) — O(1) via hashing. `ArrayList.contains` is O(n), `TreeSet` is O(log n).
2. When you need iteration in sorted key order, or range queries (`subMap`, `headMap`, `tailMap`). HashMap is faster for plain get/put.
3. `remove()` throws `NoSuchElementException` on empty; `poll()` returns `null`. `poll`/`offer`/`peek` are the "no-throw" family.

</details>

---

**Common traps:**
- `List.of`/`Arrays.asList` are immutable/fixed-size — calling `add` throws
- `Map.get` returns `null` for missing keys — NPE when you use it without checking (use `getOrDefault`)
- Iterating while removing → `ConcurrentModificationException` — use `iterator.remove()` or `removeIf`
- `contains` on `ArrayList` is O(n) — for hot membership checks use a `HashSet`
- Wrapper classes in collections: `Map<Integer, ...>` — autoboxing/unboxing nulls NPE

---
