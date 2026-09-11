# Index Structures

An **index** is a data structure that makes lookups fast — the book's index page before the full text. The two big families: **B-trees** (sorted, range-friendly, the default) and **hash indexes** (exact-match only, O(1)). Knowing what each structure does — and what it *can't* do — is the difference between fast and full-scan queries.

**The Intuition:** Without an index, finding one row means reading every row (a full table scan, $O(n)$). A B-tree organizes keys so a lookup visits ~3-4 pages regardless of table size ($O(\log n)$). It's a balanced search tree that lives on disk, with high fan-out so each disk read brings many keys.

## B-tree — the workhorse

```text
Properties:
  - Balanced: all leaves at the same depth
  - High fan-out (hundreds of keys per node) → height ~3-4 for billions of rows
  - Internal nodes: routing keys; leaves: actual entries
  - Leaves often linked → fast range scans

Operations (with n = rows):
  Point lookup:   O(log_B n) ≈ 3-4 disk reads
  Range scan:     O(log_B n + k)  (k = result size)
  Insert/delete:  O(log_B n) — split/merge nodes
  Ordered iteration: O(n) — follow the leaf links
```

## Clustered vs secondary

```text
CLUSTERED index (e.g., InnoDB PRIMARY KEY):
  The TABLE's rows are physically stored in the index's order.
  Leaf = the row data itself. One per table. No separate row fetch.
  Sequential inserts → append-only → fast.

SECONDARY index (non-clustered):
  Leaf = the key + a pointer to the row (the PK, in InnoDB).
  Lookup = index scan to the leaf, then a row fetch via the PK.
  Many allowed per table. Adds write cost.

Index-only scan: when the secondary index contains ALL columns the
query needs → no row fetch at all ("covering index").
```

## Composite (multicolumn) indexes

```sql
CREATE INDEX idx_name ON Users(last_name, first_name);

-- Usable for:
WHERE last_name = 'X'                       ✓ leftmost prefix
WHERE last_name = 'X' AND first_name = 'Y'  ✓
WHERE first_name = 'Y'                      ✗ can't skip the leftmost column!
ORDER BY last_name, first_name              ✓
WHERE last_name = 'X' ORDER BY first_name   ✓
```

**The leftmost-prefix rule:** a composite index `(a, b, c)` helps queries touching `a`, `a,b`, or `a,b,c` — never `b`, `b,c`, or `c` alone. Put the most selective / most-used column first.

## Hash index

```text
O(1) exact-match lookups:
  WHERE id = 42       ✓ fast
  WHERE id > 42       ✗ can't do ranges
  ORDER BY id         ✗ no order
  LIKE 'ab%'          ✗ no prefix scan

B-tree handles ALL of these; hash only exact matches.
=> B-tree is the default for a reason.
```

## The index-read trade-off

| Benefit | Cost |
|---------|------|
| Faster reads (lookups, joins, sorts, ranges) | Slower writes: every INSERT/UPDATE/DELETE must maintain each index ($O(\log n)$ per index) |
| Index-only scans | Disk space (roughly index size ≈ data size for wide keys) |
| Constraint enforcement (UNIQUE, PK) | Insert hotspots (auto-increment PKs contend on the rightmost leaf) |

**Rule of thumb:** index what you *query and join on* — WHERE, JOIN ON, ORDER BY, GROUP BY — not every column.

## Partial, expression & covering indexes

```sql
-- Partial: only some rows
CREATE INDEX idx_active ON Users(status) WHERE status = 'active';

-- Expression / functional: index the TRANSFORMED value
CREATE INDEX idx_lower ON Users(LOWER(email));   -- enables WHERE LOWER(email)=...
-- (Note: WHERE LOWER(email) = ... without this index won't use a plain email index!)

-- Covering: includes extra columns to avoid row fetches
CREATE INDEX idx_cov ON Orders(customer_id) INCLUDE (total);
-- Query needing only customer_id + total → index-only scan
```

## Index usage in the query plan

```text
EXPLAIN SELECT * FROM Orders WHERE customer_id = 7;
  -- "Index Range Scan on idx_orders_customer" → the index was used
  -- vs "Seq Scan on Orders" → full scan (no index / index unusable)

Reasons an index ISN'T used:
  - Function/expression on the column: WHERE YEAR(created_at) = 2026
  - Leading wildcard:  WHERE name LIKE '%smith%'
  - Type mismatch:  WHERE numeric_col = '42'  (implicit cast defeats index)
  - Selectivity too low: optimizer prefers a scan for "most rows match"
```

---

**Setup:** A 100M-row Orders table. `WHERE customer_id = 7 AND status = 'PAID'` is slow. What index?

**Solution:**
```sql
CREATE INDEX idx_orders_cust_status ON Orders(customer_id, status);
```
The leftmost prefix matches `customer_id`, then `status` filters within the index's order — the query reads only customer 7's PAID entries.

**Key insight:** Column order in a composite index decides which predicates it serves. `(customer_id, status)` serves the pair query *and* the customer-only query; `(status, customer_id)` would serve status-only and the pair but not customer-only.

---

**Setup:** Why does `WHERE LOWER(email) = 'ada@x.com'` ignore an index on email?

**Solution:** The stored values are original-case; the function changes the comparison key, so the B-tree order no longer helps. The optimizer can't know `LOWER(email)` is ordered. Fix: index the expression (`CREATE INDEX ... ON Users(LOWER(email))`).

**Key insight:** "Use the same expression you indexed" — functional indexes exist for exactly this. This is also why leading wildcards (`%smith`) and casts defeat indexes: the search key doesn't match the stored order.

---

**Setup:** Should `status` (3 distinct values) be the *first* column of a composite index?

**Solution:** Usually not — low-cardinality leading columns make the index "fan out" poorly (each status value points to millions of rows; the optimizer often prefers a scan). Prefer the high-selectivity column first: `(customer_id, status)` rather than `(status, customer_id)`.

**Key insight:** Selectivity (distinctness) guides column order. A nearly-unique first column narrows the search fastest. There are exceptions (status-only queries), but "selective first" is the strong default.

---

**Setup:** Auto-increment primary key — is it a good clustered index?

**Solution:** Yes for writes — monotonic inserts always append at the rightmost leaf (no page splits, no shuffling). It's why surrogate integer PKs are so common in InnoDB. A random key (UUID) as clustered index is the opposite — inserts hit random leaves, causing page splits and fragmentation.

**Key insight:** The clustered key determines *where rows are written*. Sequential keys = append-only = fast; random keys = random inserts = slow. This is why UUIDs are often stored as secondary indexes instead of the clustered PK.

---

## Practice (try before peeking)

1. B-tree point lookup complexity?
2. Can a hash index serve `WHERE age BETWEEN 20 AND 30`?
3. Index on `(a, b, c)` — which of these uses it: `WHERE b=1 AND c=2`, `WHERE a=1 AND c=2`?

<details><summary>Answers</summary>

1. $O(\log_B n)$ — ~3-4 disk reads for billions of rows (fan-out B is large).
2. No — hash indexes only do exact matches. Range scans need a B-tree.
3. Only `WHERE a=1 AND c=2` (leftmost prefix `a` matches; `b` can't be skipped but the index still narrows on `a`). The `b,c`-only query can't use it.

</details>

---

**Common traps:**
- Indexing every column — write slowdown with no read benefit
- Function on the indexed column — silently disables the index
- Leading wildcard LIKE — same problem
- Forgetting the leftmost-prefix rule when ordering composite columns
- Assuming the optimizer *must* use your index — it estimates; a full scan can be cheaper

---
