# Architecture Fundamentals

Architecture is the set of structural decisions that shape a system's evolution: how it's divided, how parts communicate, and which choices are hard to reverse. Good architecture keeps the *cost of change* low. The core idea: **dependencies point inward, toward the domain** — and you defer irreversible decisions as long as possible.

**The Intuition:** Architecture is the load-bearing structure of a building. Walls (features) move cheaply; the foundation (the architecture) is expensive to change. The goal: isolate the stable core (business rules) from the volatile details (frameworks, databases, UIs) so that changing a detail never requires rebuilding the core.

## The cost curve — why architecture matters

```text
Cost per change:
  No architecture:  cost grows super-linearly with code size —
    every change touches unrelated code ("big ball of mud")
  Good architecture: cost per change stays roughly FLAT —
    changes are local, isolated

"Software architecture is about minimizing the human resources
 required to build and maintain a system."  — Robert Martin
```

## Layered architecture — the dependency rule

```text
        Presentation (UI / HTTP / CLI)
              ↓ depends on
        Application (use cases / services)
              ↓ depends on
          Domain (business rules)  ← THE CORE
              ↓ depends on
        Infrastructure (DB, queues, mail, framework)
```

**The dependency rule:** source-code dependencies point *inward*. The domain knows nothing about the database or the HTTP framework. Every layer inward is *dumber about the outside world*; every layer outward is *replaceable*.

## The key patterns

### Ports & Adapters (Hexagonal)
```text
              ┌── HTTP adapter ──┐
   PORTS ◄────┤  CLI adapter    ├──► DOMAIN ◄── ports ──► adapters
   (interfaces)  GUI adapter   │       │       (interfaces)   │
              └────────────────┘       │       (DB adapter,   │
                                       │        queue adapter)┘
                                       └────────►  infrastructure

The domain defines PORTS (interfaces it needs).
Adapters implement them (REST, SQL, CLI).
Swap any adapter without touching the domain.
```

### Clean Architecture (Uncle Bob)
```text
Entities (business rules) ← Use Cases ← Interface Adapters ← Frameworks & Drivers
Dependencies point inward only. Everything volatile is on the rim.
```

## Coupling & cohesion — the quality dials

| | Cohesion | Coupling |
|--|----------|----------|
| What | How related a module's parts are | How much modules depend on each other |
| High | One responsibility, clear purpose ✓ | Change ripples everywhere ✗ |
| Low | A grab-bag of unrelated code ✗ | Change is local ✓ |

**Goal: high cohesion, low coupling.** `Module` that parses + validates + stores + emails has low cohesion; split it. Modules that reach into each other's internals have high coupling; give them interfaces.

## Modularity in practice

```text
Module = a unit with a clear BOUNDARY:
  - public API (what others use)
  - private internals (free to change)

The module boundary is the encapsulation of architecture:
  "I can rewrite the internals of this module without touching
   anything outside it" — that's the test of a good boundary.
```

## Monolith vs microservices — a cost decision

| | Modular monolith | Microservices |
|--|------------------|---------------|
| Deployment | one artifact | many services |
| Scaling | whole app | per-service |
| Complexity | development-time | operational-time |
| Boundaries | enforced by modules | enforced by network |
| When | most teams, most sizes | large orgs, clear service boundaries, independent scaling/teams |

**The trap:** microservices add distributed-system costs (network failures, tracing, eventual consistency, N deployments). *Modularity* comes from boundaries, not from splitting processes — a well-built monolith is modular; a badly-split microservice system is a distributed monolith.

---

**Setup:** A payment system must support Stripe now, PayPal later.

**Solution:**
```java
// DOMAIN (never changes when a provider changes):
interface PaymentGateway {
    Receipt charge(Amount amount);
}

// ADAPTERS (one per provider, swappable):
class StripeAdapter implements PaymentGateway { ... }
class PaypalAdapter implements PaymentGateway { ... }

// COMPOSITION ROOT (where the choice is made):
PaymentGateway gateway = new StripeAdapter(config.stripe());  // one line to swap
```

**Key insight:** The domain depends on the *port* (`PaymentGateway`), never on Stripe. Adding PayPal = a new adapter + one line at the composition root. The dependency points inward — the direction is the architecture.

---

**Setup:** Why does a layered architecture forbid the domain from calling the database directly?

**Solution:** If the domain calls `db.findUser(...)`, every domain test needs a database, and swapping storage touches domain code. The domain should call its own *port* (`UserRepository`), implemented by the infrastructure layer. Tests inject an in-memory fake.

**Key insight:** The dependency rule is what makes the domain *testable and portable*. "Depend on abstractions, not concretions" (Dependency Inversion) is the mechanism; the inward-pointing arrows are the architecture.

---

**Setup:** Should a startup build microservices?

**Solution:** Almost always no. Start with a modular monolith: clear boundaries, fast iteration, one deployable. Split into services *when* a boundary is proven by pain (independent scaling, independent teams, release cadence mismatch). Premature microservices are the #1 architecture anti-pattern.

**Key insight:** Microservices are an *operational* strategy, not a *modularity* strategy. Modularity is a code concern; you can have a perfectly modular monolith. Split when operations demand it — not for fashion.

---

**Setup:** How do you know your architecture is good?

**Solution:** Measure *change cost*:
- Can you swap the database without touching domain code? (adapter test)
- Can you add a feature without editing existing modules? (Open/Closed test)
- Can you test the core without infrastructure? (port test)
- Is a change local, or does it ripple? (coupling test)

**Key insight:** Architecture is unprovable at build time — it shows in *evolution*. The "architecture fitness functions" (automated checks that dependencies point inward, no infrastructure imports in domain) make the rules enforceable in CI.

---

## Practice (try before peeking)

1. Which direction do dependencies point in layered architecture?
2. Hexagonal "ports" are what kind of language construct?
3. Monolith or microservices for a 5-person team?

<details><summary>Answers</summary>

1. Inward — toward the domain. Outer layers depend on inner; the domain depends on nothing external.
2. Interfaces — the ports are abstractions the domain defines and adapters implement.
3. Modular monolith — microservices' operational costs aren't justified until scale/team boundaries demand them.

</details>

---

**Common traps:**
- Layered *folders* ≠ layered *dependencies* — structure follows the dependency rule, not the directory tree
- God modules / god classes — low cohesion
- Premature microservices — distributed monolith with extra ops pain
- Domain depending on frameworks — the framework becomes unswappable
- Architecture as a big up-front design — it emerges with each boundary you add under pressure

---
