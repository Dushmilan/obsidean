# Behavioral Patterns

The third GoF family — **behavioral** patterns — manage *how objects communicate*: who calls whom, when, and with what. The stars are **Strategy** (swap algorithms), **Observer** (event notification), **Command** (actions as objects), and **State** (behavior that changes with state). These show up constantly in real frameworks.

**The Intuition:** Creational patterns answer "who creates?", structural "how do they fit?", behavioral "how do they talk?" The theme is decoupling: the sender shouldn't know the receiver (Observer), the caller shouldn't know the algorithm (Strategy), the invoker shouldn't know the action (Command).

## Strategy — swap algorithms at runtime

```java
// Problem: shipping cost varies by method. if/else chains explode.
interface ShippingStrategy {
    double cost(double weight);
}
class Standard implements ShippingStrategy {
    public double cost(double w) { return w * 2; }
}
class Express implements ShippingStrategy {
    public double cost(double w) { return w * 5; }
}

class Checkout {
    private ShippingStrategy strategy;
    public void setStrategy(ShippingStrategy s) { strategy = s; }
    public double shipping(double w) { return strategy.cost(w); }
}

Checkout c = new Checkout();
c.setStrategy(new Express());       // swap at runtime
c.shipping(3.0);                    // 15.0
```

**Why it matters:** Adding a shipping method = a new class + `setStrategy`. The `Checkout` never changes. This is the Open/Closed principle made concrete — and it's exactly how `Comparator`, `sorted(key=...)`, and dependency injection work.

## Observer — notify without knowing who

```java
interface Observer { void update(String event); }

class Button {                        // the SUBJECT
    private final List<Observer> observers = new ArrayList<>();
    public void subscribe(Observer o) { observers.add(o); }
    public void click() {
        for (Observer o : observers) o.update("clicked");
    }
}

Button btn = new Button();
btn.subscribe(logger);                // three decoupled reactions
btn.subscribe(analytics);
btn.subscribe(view);
btn.click();                          // all three notified, Button knows none
```

**Why it matters:** The button doesn't know what reacts to it — listeners attach and detach freely. This is the backbone of UIs, pub/sub, event-driven systems, and reactive programming (RxJS, signals).

## Command — actions as objects

```java
interface Command { void execute(); }

class TurnOnLight implements Command {
    public void execute() { light.on(); }
}
class TurnOffLight implements Command {
    public void execute() { light.off(); }
}

// The invoker knows only the Command interface:
class RemoteControl {
    private final List<Command> history = new ArrayList<>();
    public void press(Command c) { c.execute(); history.add(c); }   // undo possible!
}
```

**Why it matters:** Encapsulating an action as an object enables: queues (delay execution), undo/redo (store history), logging, transactional behavior, and macros (compose commands).

## State — behavior changes with state

```java
interface State {
    void handle(TrafficLight light);
}
class Red implements State {
    public void handle(TrafficLight light) { System.out.println("STOP"); light.setState(new Green()); }
}
class Green implements State {
    public void handle(TrafficLight light) { System.out.println("GO"); light.setState(new Red()); }
}

class TrafficLight {
    private State state = new Red();
    public void setState(State s) { state = s; }
    public void change() { state.handle(this); }     // delegates to current state
}
// The light's behavior depends on ITS state — no if(state==RED) chains.
```

**Why it matters:** Replaces `if (state == A) ... else if (state == B)` with polymorphism — each state object knows its own transitions. TCP connections, vending machines, order lifecycles.

## Template Method — fixed skeleton, pluggable steps

```java
abstract class DataParser {
    public final void parse() {          // the fixed algorithm
        open();
        while (hasMore()) processLine(); // processLine varies
        close();
    }
    protected abstract void processLine();
    protected void open() { ... }        // shared defaults
}
// Subclasses fill in processLine only — the skeleton never changes.
```

**Why it matters:** The parent defines the algorithm once; subclasses vary the steps. This is the "framework callback" pattern — exactly how JUnit's lifecycle, Spring's templates, and UI framework hooks work.

## Template vs Strategy

```text
Template Method: INHERITANCE — subclass overrides steps of a fixed algorithm
Strategy:        COMPOSITION — inject the whole algorithm

Prefer Strategy when the algorithm varies whole-cloth;
Template when only steps vary within a fixed sequence.
```

---

**Setup:** A `DiscountCalculator` with percent, fixed, and BOGO rules.

**Solution:** Strategy:
```java
interface Discount { double apply(double price); }
class PercentDiscount implements Discount { ... }
class FixedAmountDiscount implements Discount { ... }

// The calculator delegates:
double finalPrice = price - discount.apply(price);
// Adding "Buy-One-Get-One" = one new class.
```

**Key insight:** The if/else discount logic vanishes — each rule is a strategy object, composed at runtime. This is the pattern behind Spring's `@Qualifier` and Python's key functions.

---

**Setup:** Log to console AND file AND remote — three destinations.

**Solution:** Observer:
```java
class Logger {
    private final List<LogSink> sinks = new ArrayList<>();
    public void addSink(LogSink s) { sinks.add(s); }
    public void log(String msg) { for (LogSink s : sinks) s.write(msg); }
}
// Add a new sink (database) without touching Logger.
```

**Key insight:** The logger publishes; sinks subscribe. Adding a destination is a runtime change, not a code edit — the classic publish/subscribe decoupling.

---

**Setup:** Implement undo/redo for an editor.

**Solution:** Command:
```java
interface EditCommand { void execute(); void undo(); }
class InsertText implements EditCommand {
    private final Document doc; private final String text;
    InsertText(Document d, String t) { doc = d; text = t; }
    public void execute() { doc.insert(text); }
    public void undo() { doc.deleteLast(text.length()); }
}

// History stacks:
Deque<EditCommand> undoStack = new ArrayDeque<>();
undoStack.push(new InsertText(doc, "hello"));
undoStack.push(...);
EditCommand last = undoStack.pop(); last.undo();   // undo in reverse
```

**Key insight:** Making each edit a *command object* gives you the history stack for free — undo pops and calls `undo()`, redo re-executes. This is why Command is the standard answer for editor/transaction systems.

---

**Setup:** A vending machine with states: Idle, HasCoin, Dispensing.

**Solution:** State pattern — each state is a class implementing `handle` transitions. The machine has no `if (state == ...)` chains; each state object knows what's legal (e.g., "HasCoin" accepts product selection, "Idle" doesn't).

**Key insight:** The state pattern is polymorphism applied to state machines — the "current state" object owns the transition logic. Adding a state (e.g., "Maintenance") = one class + wiring, no touching existing states.

---

## Practice (try before peeking)

1. Strategy vs State — how do they differ?
2. Which pattern lets you queue actions for later execution?
3. Template Method relies on what OOP mechanism?

<details><summary>Answers</summary>

1. Strategy: caller *swaps* the algorithm; the object doesn't change. State: the object *itself* changes behavior as its state changes — the swap happens internally on transitions.
2. Command — actions become objects you can queue, log, undo, or execute later.
3. Inheritance + overriding (subclasses fill the abstract steps) — the fixed skeleton calls the overridable methods.

</details>

---

**Common traps:**
- Strategy with one implementation — a pattern with no variation is ceremony
- Observer leaks — forgetting to unsubscribe keeps objects alive (memory leaks in long-running apps)
- State objects with shared mutable state — each transition must be clean
- Command classes with logic leaking out — keep execute/undo self-contained
- Applying patterns to trivial code — "a pattern is justified by a *recurring* problem"

---
