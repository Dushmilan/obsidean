# Concurrency Control

Isolation is *promised*; **concurrency control** is *how*. The two families are **locking** (pessimistic — block conflicts) and **MVCC** (optimistic-ish — let versions coexist). The theory: schedules, serializability, two-phase locking, and deadlock — the foundation of every modern engine's internals.

**The Intuition:** With multiple transactions running at once, the database must produce results *as if* they ran one at a time (serializable). Locking achieves this by having transactions claim rows before touching them. The rules that make locking safe — and the deadlocks it creates — are the whole subject.

## Schedules & serializability

```text
A SCHEDULE is the interleaving of operations from multiple transactions.

T1: r(A) w(A) r(B) w(B)
T2:      r(B)      w(B) r(A) w(A)

A schedule is SERIAL if no interleaving (T1 fully, then T2).
A schedule is SERIALIZABLE if it is equivalent to SOME serial schedule —
  i.e., produces the same final state. That's the correctness bar.

Conflict: two operations on the SAME object where at least one is a WRITE.
Serializability ⟺ the conflict graph (nodes = txns, edge = conflict)
  is acyclic.
```

## Two-Phase Locking (2PL) — the serializability engine

```text
PHASE 1 (growing): acquire locks, no releases
PHASE 2 (shrinking): release locks, no acquisitions

STRICT 2PL: hold ALL locks until commit/abort.
  - Guarantees serializability
  - Avoids cascading aborts (nobody reads uncommitted writes)
  - What real databases implement

Lock modes:
  SHARED (S): multiple readers allowed — conflicts with X only
  EXCLUSIVE (X): single writer — conflicts with both
```

| Requested | Held S | Held X |
|-----------|--------|--------|
| S | ✓ grant | block |
| X | block | block |

## The lost-update story through 2PL

```text
T1: LOCK-X(acct) → read(100) → write(150) → UNLOCK → COMMIT
T2: LOCK-X(acct) → BLOCKS until T1 commits → read(150) → write(130)
Final: 130 — correct. The lock serialized the two transactions.
```

## Deadlock — the price of locking

```text
T1 holds A, wants B
T2 holds B, wants A
→ deadlock. Neither can proceed.

Detection: wait-for graph; a cycle = deadlock.
Resolution: pick a victim, ROLLBACK it, let the other proceed.
Prevention: acquire all locks up front, or impose an ordering on resources.

In practice: databases detect (timeouts + cycle detection) and abort one victim.
Your code should RETRY aborted transactions.
```

## MVCC — locking's fast cousin

```text
Instead of blocking, MVCC versionizes:
  Every write creates a new row version tagged with the writer's txn id.
  Every transaction sees the newest version committed BEFORE it started.

Readers: see a consistent snapshot — NEVER block, never read dirty data.
Writers: conflict only on the same row (last-writer or abort-on-conflict).

Isolation from MVCC:
  Read Committed:  new snapshot per STATEMENT
  Repeatable Read: one snapshot per TRANSACTION
  Serializable:   snapshot + conflict detection (SSI in Postgres)

Cost: version churn → bloat → VACUUM. Long transactions pin old versions.
```

## Optimistic concurrency — the app-level version

When the DB doesn't lock what you need (or you're outside a DB), use a version counter:
```sql
UPDATE item SET qty = qty - 1, version = version + 1
WHERE id = 5 AND version = 3;
-- 0 rows affected → someone else changed it first → retry or fail
```

## Lock granularity & escalation

```text
Row locks: fine-grained, high concurrency, more overhead
Table locks: coarse, low concurrency, cheap
PAGE locks: between (SQL Server default granularity)

Lock escalation: engine upgrades many row locks to a table lock when
thresholds are hit — usually good, occasionally surprising (long scans).
```

---

**Setup:** Two transactions conflict — which operations conflict?

**Solution:**
```text
T1: write(A), read(B)
T2: read(A), write(B)
Conflicts: T1.write(A) vs T2.read(A) → conflict (write-read)
           T1.read(B) vs T2.write(B) → conflict (read-write)
Conflict graph: T1 → T2 (via A), T2 → T1 (via B) → CYCLE → NOT serializable.
```
To fix: serialized by locking (2PL) so one waits.

**Key insight:** Read-read never conflicts; write-write, read-write, write-read do. The conflict graph's acyclicity IS the serializability test — the same idea as topological ordering in graphs.

---

**Setup:** Why does strict 2PL prevent cascading aborts?

**Solution:** With strict 2PL, a transaction's writes are *locked until commit* — no other transaction can read uncommitted data. If T1 aborts, no one ever saw its partial writes, so no one else needs to abort. Without strictness (release early), T2 might read T1's uncommitted write, then T1 aborts → T2 must also abort (cascade).

**Key insight:** "Strict" = hold locks to the end. It costs a little concurrency and buys enormous robustness — which is why production engines use strict 2PL.

---

**Setup:** A transaction acquires locks in a different order than another → deadlock. Fix?

**Solution:**
```text
T1: lock A → lock B
T2: lock B → lock A     → deadlock
Fix (resource ordering): always lock in the SAME order (A then B).
Both transactions lock A first → one blocks, then proceeds — no cycle.
```
Alternatively, use `SELECT ... FOR UPDATE` in a consistent order, or rely on the DB's victim rollback + retry.

**Key insight:** Deadlock prevention by ordering is the classic technique — impose a total order on resources and always acquire in that order. It's the same trick used for avoiding lock-order inversions in multithreaded code.

---

**Setup:** What actually happens when a deadlock is detected?

**Solution:** The engine detects a cycle in the wait-for graph (or hits a lock timeout), chooses a **victim** (often the cheaper / older / lower-priority txn), rolls it back, and releases its locks. The application receives an error (e.g., MySQL error 1213) and should retry the transaction.

**Key insight:** Deadlock rollback is *expected* behavior in high-concurrency systems — applications must be written to retry. This is why idempotent, retry-safe business operations matter.

---

## Practice (try before peeking)

1. Under MVCC, can a reader block a writer?
2. What does a cycle in the wait-for graph mean?
3. Which is stricter: reading with S lock or under a snapshot?

<details><summary>Answers</summary>

1. No — readers see snapshots and never take conflicting locks; writers only conflict with other writers on the same row.
2. Deadlock — transactions waiting on each other in a cycle.
3. Snapshot (MVCC) — a consistent read that never blocks; S locks block against concurrent X locks.

</details>

---

**Common traps:**
- Assuming isolation comes free — every guarantee has a concurrency cost
- Locking everything (table locks) destroys throughput — lock at the right granularity
- Ignoring deadlock retries — "just try again" is part of the design, not an afterthought
- MVCC bloat — long transactions pin old versions indefinitely
- Serializable ≠ literally serial — it's "equivalent to some serial order," not "one at a time"

---
