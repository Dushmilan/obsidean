# Generics

Generics let you write one class/method that works for any type, with **compile-time type safety**. `List<String>` can hold only strings — the compiler checks inserts and knows the type of gets, eliminating a whole class of cast-and-crash bugs.

**The Intuition:** Without generics, a `List` holds `Object` — you can put a String in and get a String out only if you *remember* the type and cast. One wrong insert → `ClassCastException` at runtime, far from where the bug was made. Generics make the collection *typed*: `List<String>` is checked at compile time and `get()` returns `String` directly.

## The problem generics solve

```java
// WITHOUT generics (raw type):
List list = new ArrayList();
list.add("hello");
list.add(42);                    // compiles! mixing types
String s = (String) list.get(1); // ClassCastException at RUNTIME — 42 isn't a String

// WITH generics:
List<String> list = new ArrayList<>();
list.add("hello");
// list.add(42);                 // COMPILE ERROR — caught immediately
String s = list.get(0);          // no cast — compiler knows it's String
```

## Generic classes

```java
// The type parameter T is a placeholder, filled in at use:
public class Box<T> {
    private T value;

    public void set(T value) { this.value = value; }
    public T get() { return value; }
}

Box<Integer> intBox = new Box<>();
intBox.set(42);
int n = intBox.get();        // int — no cast, no boxing worry

// Multiple parameters:
public class Pair<K, V> {
    private K key;
    private V value;
    public Pair(K key, V value) { this.key = key; this.value = value; }
    public K getKey() { return key; }
    public V getValue() { return value; }
}
Pair<String, Integer> p = new Pair<>("Ada", 92);
```

## Generic methods

```java
public static <T> T first(List<T> list) {
    return list.get(0);
}

// The <T> before the return type declares the method's own type parameter
String s = first(List.of("a", "b"));      // T = String — inferred
Integer i = first(List.of(1, 2));         // T = Integer — inferred
```

## Bounded type parameters

```java
// <T extends Number> — T must be a Number (or subclass)
public static <T extends Number> double sum(List<T> nums) {
    double total = 0;
    for (T n : nums) total += n.doubleValue();   // Number's method available
    return total;
}

// T can extend a class AND implement interfaces:
// <T extends Comparable<T> & Serializable>
```

## Wildcards — `?`

```java
// ? extends — read-only ("producer"): accept any subtype
void printAll(List<? extends Animal> animals) {
    for (Animal a : animals) a.speak();     // OK — read as Animal
    // animals.add(new Dog());              // ERROR — could be a Cat list
}

// ? super — write-only ("consumer"): accept any supertype
void addDogs(List<? super Dog> dogs) {
    dogs.add(new Dog());                    // OK — Dog fits any supertype
    // Dog d = dogs.get(0);                 // ERROR — could be an Animal list
}

// The mnemonic: PECS — Producer Extends, Consumer Super
// (from the famous "Effective Java" rule)
```

## Type erasure — what actually happens

```java
// Generics are a COMPILE-TIME feature. At runtime:
// List<String> and List<Integer> are BOTH just List (the raw type).
// The compiler:
//   1. checks everything at compile time
//   2. erases type parameters to their bound (or Object)
//   3. inserts casts where needed

// Consequences:
// - list instanceof List<String>  → illegal! can't test type params
// - new T()                       → illegal! T is erased, can't construct
// - static fields of type T       → illegal (T unknown at runtime)
// - Overloading by type param     → clash (erasure makes them identical)
```

## Naming conventions

| Parameter | Meaning |
|-----------|---------|
| `T` | Type |
| `E` | Element (collections) |
| `K`, `V` | Key, Value (maps) |
| `N` | Number |
| `R` | Return type |

## Generics + collections — the everyday combo

```java
List<String> names = new ArrayList<>();
Set<Integer> ids = new HashSet<>();
Map<String, List<Integer>> scores = new HashMap<>();   // nested generics

Map<String, List<Integer>> map = new HashMap<>();
map.computeIfAbsent("Ada", k -> new ArrayList<>()).add(92);

// var + generics (Java 10+):
var list = new ArrayList<String>();       // still List<String>
```

---

**Setup:** Write a generic `max` for any Comparable type.

**Solution:**
```java
public static <T extends Comparable<T>> T max(List<T> list) {
    T best = list.get(0);
    for (int i = 1; i < list.size(); i++) {
        if (list.get(i).compareTo(best) > 0) best = list.get(i);
    }
    return best;
}

Integer m = max(List.of(3, 9, 4));        // 9
String s = max(List.of("cat", "dog"));    // "dog"
```

**Key insight:** `<T extends Comparable<T>>` — the bound guarantees `compareTo` exists, and self-referencing `Comparable<T>` is the standard pattern. Same algorithm works for any comparable type.

---

**Setup:** A method that accepts a list of any number type and returns the sum.

**Solution:**
```java
public static double total(List<? extends Number> nums) {
    double sum = 0;
    for (Number n : nums) sum += n.doubleValue();
    return sum;
}

total(List.of(1, 2, 3));            // works
total(List.of(1.5, 2.5));           // works — wildcard accepts any Number subtype
```

**Key insight:** `List<? extends Number>` reads any number type; `doubleValue()` is available via the Number bound. Without the wildcard, `List<Integer>` would NOT be a `List<Number>` (generics are invariant!) — the wildcard is the bridge.

---

**Setup:** Why can't you write `List<Object> objs = new ArrayList<String>();`?

**Solution:** Generics are **invariant** — `ArrayList<String>` is *not* a subtype of `List<Object>`, even though String is a subtype of Object. If it were allowed, this would be legal:
```java
List<Object> objs = new ArrayList<String>();  // if allowed
objs.add(42);                                  // corrupts the String list!
```
The invariance protects type safety.

**Key insight:** Arrays are *covariant* (`Object[] a = new String[10]` compiles) — and it's a known flaw (runtime `ArrayStoreException`). Generics fixed this by being invariant. Wildcards (`? extends`/`? super`) restore flexibility safely.

---

## Practice (try before peeking)

1. Why can't you do `if (list instanceof List<String>)`?
2. `List<String>` and `List<Integer>` — same class at runtime?
3. `static T field;` in a generic class — legal?

<details><summary>Answers</summary>

1. Type parameters are erased at runtime — there's no `List<String>` type to test against. You'd have to cast and risk ClassCastException.
2. Yes — both erase to the raw `List`. Generics are a compile-time illusion (erasure).
3. No — a static field is shared by the whole class; it can't have an instance's T. Also `new T()` and `T[] arr = new T[10]` are illegal.

</details>

---

**Common traps:**
- Raw types (`List` without `<...>`) — compiles with warnings, loses all safety
- `List<Number>` won't accept a `List<Integer>` — use `? extends Number`
- Creating arrays of parameterized types: `new T[10]` and `new List<String>[5]` are illegal — use `ArrayList<List<String>>` instead
- Forgetting the bound: `<T>` methods can't call `compareTo`/`doubleValue` without `<T extends ...>`
- Diamond `<>` is inferred — `new ArrayList<>()` not `new ArrayList<String>()` (Java 7+)

---
