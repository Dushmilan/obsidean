# Lambdas & Streams

Lambdas (Java 8) bring functional programming to Java: functions as values. Streams are the lazy, declarative pipeline over collections — `filter`, `map`, `reduce` replace loops with expressions. Together they're the modern Java style and the practical home of the Strategy/Functional patterns.

**The Intuition:** A lambda is an anonymous function you pass around: `x -> x * 2`. A stream is a *lazy* pipeline: describe the transformation ("filter evens, double them, sum") and the stream does it efficiently, possibly in parallel. You describe *what*, not *how* — no index variables, no accumulators.

## Lambdas

```java
// The shape:  (parameters) -> body
//            x -> x * 2            single param, expression
//            (x, y) -> x + y       multiple params
//            (x) -> { ... }        block body with statements

// Where they fit — functional interfaces (SAM: Single Abstract Method):
Runnable r = () -> System.out.println("run");
Comparator<String> c = (a, b) -> a.length() - b.length();
Callable<Integer> c2 = () -> 42;

// Built-in functional interfaces (java.util.function):
Function<String, Integer> f = s -> s.length();    // T -> R
Predicate<Integer> p = n -> n > 0;                // T -> boolean
Consumer<String> c3 = s -> System.out.println(s); // T -> void
Supplier<String> s2 = () -> "hello";              // () -> T
BiFunction<Integer, Integer, Integer> add = (a, b) -> a + b;
```

## Method references — the compact form

```java
// Where a lambda just calls a method:
list.forEach(s -> System.out.println(s));
list.forEach(System.out::println);       // instance method

list.sort((a, b) -> a.compareTo(b));
list.sort(String::compareTo);            // method reference

List<Integer> lens = list.stream().map(s -> s.length()).toList();
List<Integer> lens2 = list.stream().map(String::length).toList();

// Class::staticMethod, Class::instanceMethod, object::instanceMethod
```

## Streams — the pipeline

```java
List<String> words = List.of("cat", "elephant", "dog", "bird");

// filter → map → collect
List<String> result = words.stream()
    .filter(w -> w.length() > 3)        // intermediate: lazy
    .map(String::toUpperCase)           // intermediate: lazy
    .toList();                          // terminal: runs everything
// ["ELEPHANT", "BIRD"]

// A stream has THREE parts:
// 1. Source:   collection.stream(), Arrays.stream(), Stream.of(...)
// 2. Intermediates (lazy, chainable): filter, map, distinct, sorted, limit, peek
// 3. Terminal (eager — triggers execution): toList, count, sum, forEach, reduce, collect

// Numbers
int sum = IntStream.rangeClosed(1, 100).sum();         // 5050
double avg = list.stream().mapToInt(s -> s.length()).average().orElse(0);

// Reduction
int total = numbers.stream().reduce(0, (a, b) -> a + b);   // sum via reduce
String joined = words.stream().reduce("", (a, b) -> a + ", " + b);

// Grouping
Map<Integer, List<String>> byLength = words.stream()
    .collect(Collectors.groupingBy(String::length));
// {3=[cat, dog], 4=[bird], 8=[elephant]}

// Min/max/sorted/first
words.stream().max(Comparator.comparingInt(String::length));
words.stream().sorted().limit(2).toList();
words.stream().findFirst();
```

## Intermediate vs terminal — lazy matters

```java
// NOTHING runs until a terminal operation is called:
words.stream()
    .filter(w -> { System.out.println("filter: " + w); return w.length() > 3; })
    .map(w -> { System.out.println("map: " + w); return w.toUpperCase(); });
// No output yet! The pipeline is lazy.

// The terminal triggers it — and elements flow one at a time:
List<String> r = words.stream()
    .filter(...)
    .map(...)
    .limit(1)          // stops after ONE element flows through
    .toList();
// filter/map run on at most the first elements — short-circuit!
```

## Parallel streams — easy parallelism

```java
long total = bigList.stream()
    .parallel()              // split the work across threads
    .filter(p -> p.active)
    .count();
// "easy" — but only for stateless, order-independent pipelines.
// Shared mutable state + parallel = race conditions. Use with care.
```

## Stream vs loop — when to use which

| Task | Prefer |
|------|--------|
| Filter + transform + collect | Stream |
| Grouping, aggregation | Stream |
| Short, readable chain | Stream |
| Index needed (i, j two-pointer) | Loop |
| `break`/`continue`/complex state | Loop |
| Debugging with breakpoints per step | Loop |

```java
// Loop version:
int sum = 0;
for (int x : nums) if (x > 0) sum += x * x;

// Stream version:
int sum = nums.stream().filter(x -> x > 0).mapToInt(x -> x * x).sum();
// Same result. The stream version reads as the SPECIFICATION.
```

---

**Setup:** Find the 3 longest words in a sentence, in descending length.

**Solution:**
```java
String sentence = "the quick brown fox jumps over the lazy dog";
List<String> top3 = Arrays.stream(sentence.split(" "))
    .distinct()                                   // unique words
    .sorted(Comparator.comparingInt(String::length).reversed())
    .limit(3)
    .toList();
// [quick, jumps, brown] (or similar — ties by original order)
```

**Key insight:** The whole algorithm is a declarative chain — no counters, no sorting by hand. `comparingInt(...).reversed()` sorts by length descending; `distinct()` dedupes "the".

---

**Setup:** Count word frequencies with streams.

**Solution:**
```java
Map<String, Long> counts = Arrays.stream(text.split(" "))
    .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
// {the=3, and=2, cat=1, ...}
```

**Key insight:** `groupingBy(key, counting())` is the one-liner frequency table — group each word by itself and count per group. The manual `Map.merge` loop is the imperative equivalent.

---

**Setup:** Partition a list of numbers into evens and odds.

**Solution:**
```java
Map<Boolean, List<Integer>> parts = numbers.stream()
    .collect(Collectors.partitioningBy(n -> n % 2 == 0));
// parts.get(true)  = evens
// parts.get(false) = odds
```

**Key insight:** `partitioningBy` gives a two-bucket map keyed by a boolean — the idiomatic "split into two groups" operation. `groupingBy` generalizes to N groups.

---

**Setup:** Sum the squares of even numbers from a large range without materializing a list.

**Solution:**
```java
long total = IntStream.rangeClosed(1, 10_000_000)
    .filter(n -> n % 2 == 0)
    .mapToLong(n -> (long) n * n)
    .sum();
```

**Key insight:** `IntStream.rangeClosed` is lazy — no billion-element list exists. The stream processes values one at a time, like Python's generators. `mapToLong` avoids int overflow on the squares.

---

## Practice (try before peeking)

1. `list.stream().filter(...)` — does anything run?
2. Which is terminal: `filter`, `map`, `toList`, `sorted`?
3. `words.stream().map(w -> w.length()).toList()` — what's the type of the result?

<details><summary>Answers</summary>

1. No — intermediates are lazy; nothing executes until a terminal operation (`toList`, `sum`, `count`, ...) is called.
2. `toList` is terminal (returns the result); `filter`, `map`, `sorted` are intermediate (return streams, do nothing yet).
3. `List<Integer>` — the map's output type (lengths).

</details>

---

**Common traps:**
- Forgetting the terminal operation — the pipeline silently does nothing
- Reusing a stream — streams are single-use; `list.stream()` each time
- Modifying the source collection while streaming → `ConcurrentModificationException`
- Lambdas capturing mutable loop variables (use effectively-final variables)
- Parallel streams on shared mutable state — data races; use only for stateless pipelines

---
