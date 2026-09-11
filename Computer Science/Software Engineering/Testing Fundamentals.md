# Testing Fundamentals

Tests are executable specifications: they say what code *should* do, and they verify it on every run. The layers — unit, integration, end-to-end — form the **testing pyramid**: many fast unit tests, fewer slower integration tests, fewest expensive E2E tests. The whole system exists to enable safe refactoring.

**The Intuition:** A test is a tiny program that asserts an expected outcome: "given input X, the function returns Y." The suite is a safety net — when you change code, the net catches what broke. The pyramid is about *return on investment*: the cheap, fast tests catch most bugs; the expensive ones catch the cross-system issues the cheap ones miss.

## The testing pyramid

```text
        / E2E \        few, slow, expensive — catch system-level breaks
       /  E2E  \       (UI flows, API journeys)
      /         \
     / integration \    some, seconds — catch wiring bugs
    /  integration   \  (DB, API, module boundaries)
   /                  \
  /____unit tests______\   many, milliseconds — catch logic bugs
                          most bugs live here; run on every save
```

| Layer | Count | Speed | Cost | Catches |
|-------|-------|-------|------|---------|
| Unit | many | ms | cheap | isolated logic bugs |
| Integration | some | s | medium | wiring/contract bugs |
| E2E | few | min | expensive | whole-system breakage |

## The anatomy of a test

```python
def test_withdraw_reduces_balance():
    # ARRANGE — set up the world
    account = BankAccount(initial=100)

    # ACT — exercise the behavior
    ok = account.withdraw(30)

    # ASSERT — verify the outcome
    assert ok is True
    assert account.balance() == 70
```

**Arrange–Act–Assert** is the universal structure. A good test reads like a sentence: "given X, when Y, then Z."

## Good test properties

| Property | Meaning |
|----------|---------|
| **Isolated** | Tests don't depend on each other or shared state |
| **Deterministic** | Same result every run (no sleeps, dates, randomness) |
| **Fast** | So fast you run them constantly |
| **Named for behavior** | `test_withdraw_rejects_overdraft` not `test_1` |
| **One concept each** | A failure pinpoints the broken behavior |
| **Readable** | The assertion states the contract |

## Coverage — necessary but not sufficient

```text
Coverage = % of lines executed by tests.
  70% line coverage is common; 100% is possible but not meaningful.
High coverage does NOT mean correct tests:
  - Tests that assert nothing
  - Tests that only run happy paths
  - Tests that mirror the implementation (never catch its bugs)

The metric that matters: do failures surface REAL regressions?
Coverage is a floor, not a goal.
```

## Edge cases & boundaries

Bugs cluster at boundaries:
```text
empty input, null, 0, negative, max int, off-by-one indices,
empty collections, strings with whitespace/unicode, huge values,
timeouts, network failures, concurrent access
```

**Property-based testing:** instead of hand-picked examples, state properties and let the framework generate inputs:
```python
from hypothesis import given, strategies as st

@given(st.lists(st.integers()))
def test_sort_is_idempotent(xs):
    once = sorted(xs)
    assert sorted(once) == once          # sorting twice = sorting once
```

---

**Setup:** The test for a function is "mirroring" the implementation. What's the risk?

**Solution:** `add(a, b)` implemented as `a + b`, tested with the *same formula* (`assert add(2,3) == 2+3`) — the test can't fail unless the code doesn't compile. Tests must encode the *expected behavior* (e.g., table of known results), not re-derive the implementation.

**Key insight:** A test is only as good as its independence from the code. "Kill the mutant" thinking helps: if you broke the implementation, would this test catch it? If not, it's a tautology.

---

**Setup:** A test passes locally but fails in CI. Common causes?

**Solution:**
1. **Environment differences** — different Python/Node versions, OS, locale, timezone
2. **Order dependence** — test B passes only because test A ran first (shared state)
3. **Timing/races** — sleeps, async, random data
4. **Missing fixtures/data** — CI starts clean; tests relied on local files

**Key insight:** The fixes: pin versions (lockfiles, containers), isolate tests (fresh state per test), inject clocks/randomness (deterministic tests), commit fixtures. A flaky test is a *bug in the test*, not a mystery.

---

**Setup:** When is 100% coverage justified?

**Solution:** Rarely. 100% coverage is worth it for code where *any* bug is catastrophic (parsers, security-critical validation, financial calculations). For most business logic, 70-90% with *good* assertions beats 100% with shallow ones. The cost of maintaining tests for trivial getters/setters isn't repaid.

**Key insight:** Coverage is a heuristic for *untested risk*. The question is "what would a bug here cost?" — expensive-to-fail code gets more coverage. This is the same cost-benefit lens as deciding where to add indexes.

---

## Practice (try before peeking)

1. Which layer catches "the API returns the wrong field name"?
2. Why are tests that share a database table risky?
3. What's the first thing to check when a test is flaky?

<details><summary>Answers</summary>

1. Integration tests — the contract between the client and the API. Unit tests mock the API, so they can't see the mismatch.
2. Order dependence — test A's leftovers can break/pass test B. Tests must isolate state.
3. Whether it fails deterministically in isolation — then it's environmental or order-dependent, not the code.

</details>

---

**Common traps:**
- Testing implementation details (private methods) — brittle tests, zero behavior coverage
- Over-mocking — the test passes while the real system fails
- Skipping the failure path — the happy path is the easy 50%
- Flaky tests ignored — they eventually hide real regressions
- Tests as an afterthought — writing tests last means writing them for code designed to be untestable

---
