# Clean & Hexagonal Architecture

Clean Architecture (Robert Martin) and Hexagonal/Ports & Adapters (Alistair Cockburn) are the two most influential modern architectures. Both express one idea: **the business core is king; everything else is a swappable detail**. This note is the practical how-to: the dependency rule, use cases, entities, and the composition root.

**The Intuition:** Imagine the business rules are the heart of a body, and frameworks, databases, and UIs are the organs and limbs. The heart shouldn't know how the lungs work — it just needs blood (ports). When you upgrade the lungs (database), the heart is untouched. Clean architecture makes the heart (domain) the center and pushes every detail to the rim.

## The layered rings

```text
        OUTERMOST: Frameworks & Drivers (Spring, Django, Rails, DB drivers, UI)
              ↓ depends on (implement)
        Interface Adapters (controllers, presenters, repository implementations)
              ↓ depends on (implement)
        Use Cases (application services — orchestrate entities)
              ↓ depends on
        ENTITIES (business rules — the pure core)
```

**The Dependency Rule:** source code dependencies point *inward only*. Nothing in an inner ring knows anything about an outer ring. The domain has no imports of frameworks, no SQL, no HTTP — just pure business logic.

## The four layers in practice

### Entities — the business rules
```java
// Pure Java. No Spring, no JPA annotations, no HTTP.
public class Order {
    private final List<OrderLine> lines;
    private Status status = Status.PENDING;

    public void addLine(Product p, int qty) {
        if (status != Status.PENDING) throw new IllegalStateException();
        lines.add(new OrderLine(p, qty));
    }
    public Money total() { ... }              // pure calculation
}
```

### Use cases — the application logic
```java
// Orchestrates entities + ports. One class per use case.
public class PlaceOrder {
    private final OrderRepository orders;     // a PORT (interface)
    private final PaymentGateway payments;    // a PORT

    public PlaceOrder(OrderRepository r, PaymentGateway p) { ... }

    public OrderId execute(Cart cart, PaymentInfo pi) {
        Order order = new Order();
        for (CartLine l : cart.lines()) order.addLine(l.product(), l.qty());
        Receipt rc = payments.charge(order.total(), pi);   // calls the port
        orders.save(order);
        return order.id();
    }
}
```

### Interface adapters — the implementations
```java
// JPA repository implementing the domain port:
public class JpaOrderRepository implements OrderRepository {
    private final JpaRepository<OrderEntity, Long> repo;
    public void save(Order o) { repo.save(toEntity(o)); }   // maps domain ↔ persistence
}
// REST controller calling the use case:
@RestController
class OrderController {
    private final PlaceOrder placeOrder;
    @PostMapping("/orders")
    OrderDto place(@RequestBody CartDto cart) { ... }
}
```

### Frameworks — the rim
```java
// Composition root (main / Application.java):
public static void main(String[] args) {
    OrderRepository repo = new JpaOrderRepository(entityManager);
    PaymentGateway pgw = new StripeGateway(config);
    PlaceOrder useCase = new PlaceOrder(repo, pgw);   // WIRE the pieces
    // hand useCase to the web framework...
}
```

## Use case = one behavior per class

```text
PlaceOrder, CancelOrder, GetOrderHistory — one class each.
Each constructor takes the PORTS it needs (dependency injection).
Each execute() is a short, readable script of domain + port calls.

The use case layer is where business *processes* live:
  - validation that spans entities
  - calling external services through ports
  - transaction boundaries (often declarative)
```

## The composition root

```text
The ONLY place that knows concrete classes.
Everything else depends on interfaces.

main() = new everything, wire it up, hand it to the framework.

This is why "dependency injection" is really "invert the dependencies
at the composition root."
```

## Database-driven vs use-case-driven design

```text
Database-driven (common but backwards):
  Design tables first → entities become table mappings → use cases
  are CRUD scripts. The domain is a mirror of the schema.

Use-case-driven (Clean):
  Design the BEHAVIORS first (use cases) → entities enforce rules →
  the persistence layer maps to tables however it likes.
  The domain owns its rules; the DB is a detail.
```

---

**Setup:** A validation rule "an order can't exceed $10,000" — where does it live?

**Solution:** In the *entity* (or a domain service):
```java
public void addLine(Product p, int qty) {
    if (total().plus(p.price().times(qty)).exceeds(MAX)) {
        throw new OrderLimitExceeded();
    }
    lines.add(new OrderLine(p, qty));
}
```
Not in the controller (HTTP layer), not in the SQL (persistence layer). The rule is business logic — it belongs in the core.

**Key insight:** The "where does this rule live?" question is the architecture test. Rules about *money, status transitions, invariants* → domain. Rules about *routing, formatting, protocol* → adapters.

---

**Setup:** Swap MySQL → PostgreSQL. What should change?

**Solution:** The adapter only: `JpaOrderRepository` (or its config). The domain, use cases, and controllers are untouched — they depend on the `OrderRepository` *port*. If a repository change is forced by DB-specific behavior (e.g., different locking), the *use case* may declare its need (e.g., a `lockForUpdate` port method) — but never SQL in the core.

**Key insight:** The port is the contract. As long as `save/get` semantics hold, the implementation is free. This is why "the database is a detail" isn't a slogan — it's the test of whether your boundaries actually point inward.

---

**Setup:** Testing strategy for Clean Architecture.

**Solution:**
```text
Entities:    pure unit tests (no mocks — just instantiate)
Use cases:   unit tests with in-memory fakes for ports
Adapters:    integration tests against the real DB/API
Controllers: tests against the framework (thin)
```
The domain/use-case tests run in milliseconds with zero infrastructure.

**Key insight:** The architecture *is* the testability — depending on ports means every inner layer is testable without the outer layers. If a layer is hard to test, the dependencies point the wrong way.

---

**Setup:** When is Clean Architecture overkill?

**Solution:** For a CRUD app with no business rules (a thin data-entry form), the layers add ceremony without payoff — a service + repository is enough. Clean Architecture pays when *rules change* (business logic evolves), when *details swap* (DB/vendor changes are plausible), and when the domain is valuable enough to protect.

**Key insight:** Architecture is an investment in change-friendliness. YAGNI applies: add boundaries where change is *likely and costly*. But as soon as a rule matters ("this amount can't exceed X"), it deserves its place in the core.

---

## Practice (try before peeking)

1. What's the only layer that knows concrete classes?
2. Where does a "5% discount on orders over $500" rule live?
3. Entities may depend on what?

<details><summary>Answers</summary>

1. The composition root (main / wiring) — everywhere else depends on interfaces.
2. The domain — it's a business rule. The entity or a domain service computes the discount; the use case orchestrates it.
3. Nothing external — entities are pure business logic: other domain objects, value types, exceptions. No frameworks, DBs, or HTTP.

</details>

---

**Common traps:**
- Annotations (JPA, Spring) leaking into entities — the domain is no longer pure
- Use cases with framework types (`HttpRequest` in the signature) — keep them plain
- The repository returning framework types — map to domain types at the boundary
- Composition root scattered everywhere (`new` calls in adapters) — wire once
- Perfect-layering paralysis — pragmatic exceptions are fine if the *direction* holds

---
