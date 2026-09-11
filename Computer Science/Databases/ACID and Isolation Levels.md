# ACID & Isolation Levels

Transactions are the unit of reliability. **ACID** — Atomicity, Consistency, Isolation, Durability — is the contract databases keep so concurrent access and crashes don't corrupt data. **Isolation levels** are the dial that trades correctness for concurrency. This is the theory behind `@Transactional`, `BEGIN/COMMIT`, and every "it worked in dev but lost updates in prod" story.

**The Intuition:** A bank transfer is two writes: debit Alice, credit Bob. If the system crashes after the first, money vanishes. A transaction wraps both — all-or-nothing. Meanwhile, two cashiers updating the same balance concurrently must not clobber each other. ACID names the guarantees; isolation levels say *how much* concurrency safety you're willing to pay for.

## The four properties

| Property | Promise | How it's kept |
|----------|---------|---------------|
| **Atomicity** | All-or-nothing | Write-ahead log (undo records) — abort rolls back |
| **Consistency** | Constraints hold before and after | DB constraints + app logic |
| **Isolation** | Concurrent txns appear serial | Locking / MVCC |
| **Durability** | Committed survives crashes | Write-ahead log (redo) + fsync |

## The transaction lifecycle

```sql
BEGIN;                              -- start
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;                             -- make permanent (or ROLLBACK to undo)
-- On crash mid-way: the log undoes the partial effects (atomicity)
```

## The four anomalies — what isolation prevents

| Anomaly | Description | Level that prevents it |
|---------|-------------|----------------------|
| **Dirty read** | Read uncommitted data that may roll back | Read Committed |
| **Non-repeatable read** | Same query, different result (row changed) | Repeatable Read |
| **Phantom** | Same query, different row *count* (inserted) | Serializable |
| **Lost update** | Two txns read-modify-write; one overwrites the other | (all — needs locking/versioning) |

## The isolation levels (weak → strong)

| Level | Dirty read | Non-repeatable | Phantom | Lost update | Concurrency cost |
|-------|:---:|:---:|:---:|:---:|:---:|
| Read Uncommitted | ✗ risk | ✗ risk | ✗ risk | ✗ risk | highest |
| Read Committed (default in most) | ✓ prevented | ✗ risk | ✗ risk | ✗ risk | high |
| Repeatable Read | ✓ | ✓ prevented | ✗ risk (MySQL: prevented) | ✓ | medium |
| Serializable | ✓ | ✓ | ✓ | ✓ | lowest |

**The rule:** stronger isolation = more correctness, less concurrency. Most engines default to Read Committed; you raise the level only where correctness demands it.

## MVCC — the reason it's fast

**Multi-Version Concurrency Control:** writers create new *versions* of rows; readers see a consistent *snapshot* — readers never block writers, writers never block readers.

```text
t0: BEGIN READ txn                    — takes a snapshot
t1: Writer updates row → new version
t2: Reader SELECTs row → sees the OLD version (its snapshot)
t3: Reader commits
```
Postgres, MySQL InnoDB, Oracle all use MVCC. The cost: old versions linger (bloat) until `VACUUM`/`PURGE` reclaims them.

## Optimistic vs pessimistic concurrency

```text
PESSIMISTIC — lock first, then operate:
  SELECT ... FOR UPDATE;        -- locks the row(s)
  ... compute ...
  UPDATE ...; COMMIT;
  A concurrent writer BLOCKS until the lock releases.
  Good: high contention, correctness-critical (banking)

OPTIMISTIC — operate, then verify:
  UPDATE accounts SET balance = new_balance, version = version+1
  WHERE id = 1 AND version = old_version;
  If 0 rows affected → someone changed it → retry.
  Good: low contention, read-heavy workloads (shopping carts)
```

## Serializability & isolation definitions

```text
SERIALIZABLE means: the concurrent execution is equivalent to SOME
serial order of the transactions. The strongest guarantee.

An execution is SERIAL if transactions run one at a time — trivially safe.
Serializability ≠ serial: it allows interleaving as long as the outcome
matches a serial run.
```

---

**Setup:** Two transactions both read balance = 100 and write new values. What goes wrong?

**Solution:** Lost update:
```text
T1: read(100) → balance = 150 (wants to add 50)
T2: read(100) → balance = 80  (wants to subtract 20)
Both write → last writer wins: final = 80 or 150, one update LOST.
```
Fix with locking: `SELECT balance FROM accounts WHERE id=1 FOR UPDATE;` — T2 blocks until T1 commits.

**Key insight:** The read-modify-write cycle must be atomic — lock the row (pessimistic) or CAS via version (optimistic). This is *the* most common real-world concurrency bug, and isolation levels alone (even Repeatable Read) don't always fix it — you need explicit locking or version checks.

---

**Setup:** At Read Committed, T1 reads a row, then T2 updates and commits it, then T1 reads again. What does T1 see?

**Solution:** The second read sees T2's committed value — a **non-repeatable read** (same query, different result within one transaction). To keep T1's reads stable, use Repeatable Read or lock the row.

**Key insight:** "Repeatable Read" means *my transaction's repeated reads don't change* — even though other transactions commit in between. This matters for multi-step business logic that reads the same data twice and expects consistency (e.g., "read price, then apply discount").

---

**Setup:** Why doesn't Repeatable Read always prevent phantoms in Postgres... and does it in MySQL?

**Solution:** Postgres's Repeatable Read allows phantoms (a query returning a *different row count* after a concurrent insert commits). MySQL InnoDB's Repeatable Read uses gap locks that prevent insertions into scanned ranges — so it *does* prevent phantoms at that level. Different engines, different semantics.

**Key insight:** Isolation levels are *nominally* standardized but engine-specific in detail. Never port correctness assumptions between databases without reading the docs — the MySQL vs Postgres repeatable-read difference is a famous trap.

---

**Setup:** A web app's checkout reads the stock, decrements, and saves. How should it be done correctly?

**Solution:** One atomic UPDATE with a guard:
```sql
UPDATE products SET stock = stock - 1 WHERE id = 5 AND stock > 0;
-- Returns affected-rows: 1 = reserved, 0 = out of stock.
```
No separate read-then-write — the single statement is atomic under Read Committed.

**Key insight:** The best concurrency fix is often *not needing one* — express the invariant in one atomic statement. Row-count-based "did it succeed?" replaces locks entirely. This is why `UPDATE ... WHERE ...` with a guard is the standard stock/seat reservation pattern.

---

## Practice (try before peeking)

1. Which anomaly does Read Committed still allow?
2. What's the durability mechanism on a crash?
3. `SELECT ... FOR UPDATE` — optimistic or pessimistic?

<details><summary>Answers</summary>

1. Non-repeatable reads (and phantoms) — only dirty reads are prevented at Read Committed.
2. The write-ahead log (WAL) — committed transactions' redo records are flushed to disk and replayed on recovery.
3. Pessimistic — it takes a lock up front, blocking concurrent writers.

</details>

---

**Common traps:**
- Assuming your DB's default (Read Committed) is Serializable — it isn't
- Relying on isolation level to prevent lost updates — you still need locking or version guards
- Reading uncommitted data in analytics ("it's just a report") — dirty reads corrupt decisions
- Forgetting MVCC bloat — long transactions hold snapshots, growing the table
- Portable-code fallacy: repeatable-read semantics differ between Postgres and MySQL

---
