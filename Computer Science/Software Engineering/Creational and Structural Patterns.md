# Creational & Structural Patterns

Two of the three GoF pattern families: **creational** patterns control *how objects are created* (so callers don't hard-code `new Concrete()`), and **structural** patterns control *how objects compose* (so the structure stays flexible). Each pattern is a known solution to a recurring problem — learn the *problem*, not the pattern name.

**The Intuition:** Patterns are chess openings for code — standard responses to recurring situations. Creational patterns answer "who decides which class to instantiate?" Structural patterns answer "how do these pieces fit together without gluing them permanently?" The payoff is the Open/Closed principle: extend by adding, not editing.

## Creational patterns

### Factory Method — subclasses decide the concrete type
```java
// Problem: callers hard-code `new WordDoc()` — adding PDFDoc means edits.
interface Document { void open(); }
class WordDoc implements Document { public void open() { ... } }

class DocumentCreator {
    public Document create(String type) {       // THE FACTORY
        return switch (type) {
            case "word" -> new WordDoc();
            case "pdf"  -> new PdfDoc();
            default -> throw new IllegalArgumentException();
        };
    }
}
// Adding a format = one case + one class. Callers unchanged.
```
**Use when:** construction logic varies by type, or you want to decouple callers from concrete classes.

### Builder — step-by-step construction of complex objects
```java
Pizza pizza = new Pizza.Builder()
    .size("Large")
    .topping("Mushroom")
    .topping("Olive")
    .cheese(true)
    .build();
// Instead of a constructor with 8 positional params:
// new Pizza("Large", null, null, null, true, ...)  — unreadable & error-prone
```
**Use when:** an object needs many optional configurations (URLs, HTTP requests, complex configs).

### Singleton — one instance (use sparingly!)
```java
public final class Config {
    private static final Config INSTANCE = new Config();
    private Config() {}                          // private constructor
    public static Config get() { return INSTANCE; }
}
```
**Anti-pattern warning:** a global `get()` hides dependencies and breaks testability. Prefer dependency injection — the "singleton *by the container*" pattern keeps one instance without global access.

## Structural patterns

### Adapter — make incompatible interfaces work together
```java
// Legacy API:
class LegacyReportGenerator {
    String generateLegacy() { return "legacy"; }
}

// The interface the new system expects:
interface ReportSource { String fetchReport(); }

// ADAPTER — wraps the legacy, exposes the new interface:
class LegacyAdapter implements ReportSource {
    private final LegacyReportGenerator legacy;
    public LegacyAdapter(LegacyReportGenerator g) { legacy = g; }
    public String fetchReport() { return legacy.generateLegacy(); }
}
// New system uses ReportSource; the legacy is adapted, not modified.
```
**Use when:** integrating with old/third-party code you can't change.

### Decorator — add behavior without subclassing
```java
interface Stream { void write(String s); }

class FileStream implements Stream { public void write(String s) { ... } }

// DECORATOR wraps + adds behavior:
class EncryptedStream implements Stream {
    private final Stream inner;
    public EncryptedStream(Stream inner) { this.inner = inner; }
    public void write(String s) { inner.write(encrypt(s)); }   // add on the way through
}

Stream s = new EncryptedStream(new FileStream());   // compose freely
Stream s2 = new CompressedStream(new EncryptedStream(new FileStream()));
```
**Use when:** you need combinations of behaviors (Java's `BufferedReader` wrapping `FileReader` is exactly this).

### Composite — treat leaf and container uniformly
```java
interface Component { void render(); }
class Leaf implements Component { public void render() { ... } }
class Group implements Component {
    private List<Component> children = new ArrayList<>();
    public void render() { for (Component c : children) c.render(); }
}
// A Group can contain Leaves OR Groups — recursion via the interface.
```
**Use when:** tree structures — UI widgets, file systems, organization charts.

### Facade — a simple door into a complex subsystem
```java
class VideoConverter {                      // THE FACADE
    void convert(String src, String fmt) {
        codecDetector.run(src);             // hide all this complexity
        decoder.decode(...);
        encoder.encode(fmt, ...);
        container.mux(...);
    }
}
// Callers get one method instead of five classes.
```

---

**Setup:** Replace `new ConcreteClass()` sprinkled everywhere with a factory.

**Solution:**
```java
// Everywhere:  Notification n = new EmailNotification(...)
// The class changes → edit every call site.

// After:
Notification n = NotificationFactory.create("email", config);
// Only the factory knows concrete classes.
// Swap Email→SMS = change the factory's case + one class.
```

**Key insight:** The factory centralizes *construction decisions*. Callers depend on the abstraction (`Notification`); the concrete choice lives in one place. This is Dependency Inversion applied to creation.

---

**Setup:** Wrap a slow legacy API with caching — without changing the API's users.

**Solution:** Decorator:
```java
class CachedUserService implements UserService {
    private final UserService inner;
    private final Map<String, User> cache = new HashMap<>();

    public User findByEmail(String email) {
        return cache.computeIfAbsent(email, inner::findByEmail);
    }
}
// Callers still use UserService — the cache is invisible.
```

**Key insight:** The decorator adds a *cross-cutting* behavior (caching) around the existing interface. No subclassing, no caller changes — and you can stack behaviors (cache + logging + retry).

---

**Setup:** A UI needs buttons, text fields, and checkboxes in Windows *and* Mac styles.

**Solution:** Abstract Factory:
```java
interface UIFactory {
    Button createButton();
    TextField createTextField();
}
class WindowsFactory implements UIFactory { ... }
class MacFactory implements UIFactory { ... }

// App builds its UI through the factory:
void buildUI(UIFactory f) {
    Button b = f.createButton();     // Windows or Mac, transparently
    ...
}
```

**Key insight:** The abstract factory produces *families* of related objects consistently — no mixing a Windows button with a Mac text field. The app depends only on `UIFactory`; swapping platforms = passing a different factory.

---

**Setup:** When is Singleton a legitimate choice?

**Solution:** When there's genuinely one instance AND global access is acceptable: a logging sink, a hardware interface, a configuration registry *in a small app*. But the same "one instance" can be achieved by constructing once and injecting it — which stays testable. The pattern isn't the problem; *global mutable state* is.

**Key insight:** Modern practice: "singleton by convention" — create one instance at the composition root and inject it. You keep the benefits (one instance) and drop the liabilities (hidden global, untestable).

---

## Practice (try before peeking)

1. Adapter vs Decorator — what's the difference?
2. Which pattern for "tree of UI elements"?
3. Builder vs Factory — when each?

<details><summary>Answers</summary>

1. Adapter changes the *interface* (legacy → expected); Decorator keeps the interface and adds *behavior* around it.
2. Composite — leaves and containers share an interface; containers recurse.
3. Factory = "which class?" (one of a family); Builder = "how is this complex object configured?" (step-by-step optional parts).

</details>

---

**Common traps:**
- Singleton-as-global-state — the most-abused pattern; prefer injection
- Decorators that change the interface (then it's an adapter)
- Factory methods that just `new` without a decision — ceremony without value
- Applying patterns "because they're in the book" — each has a *problem* it solves; no problem, no pattern
- Deep decorator chains hiding behavior — three levels is often enough

---
