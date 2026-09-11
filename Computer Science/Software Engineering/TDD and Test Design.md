# TDD & Test Design

**Test-Driven Development** flips the order: write the test *first*, watch it fail, write the minimal code to pass, then refactor. Beyond the red-green-refactor ritual, the real skills are **test doubles** (mocks, stubs, fakes), **test naming**, and knowing what *not* to test.

**The Intuition:** TDD is design-in-small: the first test forces you to decide the *interface* — the name, the inputs, the contract — before the implementation exists. It also guarantees every line of behavior has a test. Red (test fails) → Green (minimal code passes) → Refactor (clean up, tests still green). The discipline produces code designed for testability.

## Red-Green-Refactor

```text
RED    — write ONE failing test. Run it. Watch it fail.
         (The failure is the specification.)
GREEN  — write the MINIMAL code to make it pass.
         (No extras. No speculative features.)
REFACTOR — improve the code. Tests verify you didn't break anything.

Repeat. Each cycle adds one behavior, proven.
```

**The rules:**
- A test that never fails (never went red) is suspect — it may be testing nothing
- Write only enough code to pass the current test (YAGNI)
- Refactor only under the protection of green tests

## Test doubles

| Double | Behavior | Use |
|--------|----------|-----|
| **Stub** | Returns canned answers; no logic | Replace a dependency for a fixed scenario |
| **Mock** | Stub + records calls; can assert on them | Verify interactions ("was save() called?") |
| **Fake** | Working lightweight implementation | In-memory DB instead of the real one |
| **Spy** | Wraps the real object, records calls | Observe calls on the real dependency |

```python
# Mock — verify the email was actually sent:
emailer = Mock()
service = OrderService(emailer)
service.place_order(...)
emailer.send.assert_called_once_with("Ada", "order confirmed")

# Fake — a real in-memory repository:
class InMemoryRepo:
    def __init__(self): self.data = {}
    def save(self, r): self.data[r.id] = r
    def get(self, i): return self.data.get(i)
```

## The mocking danger

```text
Over-mocking: every dependency mocked → the test verifies
  the MOCK's behavior, not the code's.
  The classic failure: code calls db.save() but the real DB has a
  different API → the mock "works", production crashes.

Rule: mock at the BOUNDARY (the external thing: API, DB, clock).
Don't mock your own code's internals — test them.
```

## What to test — and what not to

```text
TEST:
  - Public behavior: inputs → outputs, side effects
  - Edge cases and failure paths
  - Invariants ("balance is never negative")
  - The contract between modules (integration)

DON'T test:
  - Private methods (they're implementation — test via public behavior)
  - Third-party libraries (they have their own tests)
  - Trivial getters/setters (nothing can go wrong)
  - The framework (Spring, Django — trusted)
  - Implementation details (refactoring would break them)
```

## Test naming & structure

```python
# Behavior-named, sentence-shaped:
def test_withdraw_rejects_overdraft(): ...
def test_withdraw_returns_false_on_overdraft(): ...

# Structure: Arrange-Act-Assert (AAA)
# Or Given-When-Then (BDD flavor)
```

## The transformation priority premise

Write the test for the *simplest* behavior first, then grow:
```text
test returns 0 for empty input      → code: return 0
test sums [1,2]                     → code: sum them
test rejects negative numbers       → code: raise
```
Each step is one behavior, proven green, before the next.

---

**Setup:** TDD a `fizzbuzz(n)` from scratch.

**Solution:**
```text
1. RED:  test_fizzbuzz_returns_number_for_plain → assert fizzbuzz(1) == "1" → FAIL
   GREEN: return str(n)
2. RED:  test_fizzbuzz_multiple_of_3 → assert fizzbuzz(3) == "Fizz" → FAIL
   GREEN: if n % 3 == 0: return "Fizz"
3. RED:  test_multiple_of_5 → "Buzz";  GREEN: handle 5
4. RED:  test_multiple_of_both → "FizzBuzz";  GREEN: check 15 first
5. REFACTOR: tidy the conditions — tests still green
```

**Key insight:** The tests drove the interface (function name, string output) before implementation existed. Each step added exactly one behavior. The final refactor is safe *because* the suite exists — this is TDD's whole value proposition.

---

**Setup:** When should you use a stub vs a fake vs a mock?

**Solution:**
- **Stub**: "the API returns 500" — I need the branch that handles 500. Stub returns it, nothing else.
- **Fake**: "I need to test the repository's save/get logic" — an in-memory implementation behaves like the real one, no network.
- **Mock**: "I must verify the email was sent" — assert on the call. If you only need the return value, a stub suffices; mocks are for *verifying interactions*.

**Key insight:** Choose the *simplest* double that serves the test. Reach for mocks only when you assert on *how* something was called — otherwise you're over-verifying implementation detail.

---

**Setup:** A test suite breaks every time you refactor — why?

**Solution:** The tests assert implementation details (private methods, internal state, exact call sequences) instead of behavior. Fix: test the *observable contract* — inputs, outputs, side effects — through the public API.

**Key insight:** Tests that survive refactoring are the point. If changing `_compute_xyz`'s name breaks 30 tests, the tests are coupled to the wrong thing. Behavior-based tests make refactoring *safe* — the safety net is what allows the improvement.

---

## Practice (try before peeking)

1. Why must a TDD test be seen to fail first?
2. Over-mocking causes what failure mode?
3. What's the minimal code to pass `test_area_rectangle(2,3) == 6`?

<details><summary>Answers</summary>

1. A test that passes immediately might be testing nothing (tautology, wrong assertion, or the feature already existed). Seeing red proves the test can fail — so its green later means something.
2. Tests pass against mocks while the real integration is broken — false confidence. Mock at boundaries only.
3. `def area(w, h): return w * h` — the literal minimal implementation. The next test forces generalization.

</details>

---

**Common traps:**
- Writing all tests *after* the code — you've lost TDD's design pressure and often write tautological tests
- Mocks for everything — the suite becomes a mockery
- "Refactor" without green tests — you're just changing code unguided
- One test asserting five things — failures become ambiguous
- Testing the framework/library — noise, not safety

---
