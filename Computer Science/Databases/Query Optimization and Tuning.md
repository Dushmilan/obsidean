# Query Optimization & Tuning

The optimizer turns your SQL into a physical execution plan — choosing join algorithms, index usage, and join order from *statistics* about your data. The tuning skill is: read the plan (`EXPLAIN`), find the expensive node, and fix the cause — usually a missing index, a bad predicate, or a query shape the optimizer can't optimize.

**The Intuition:** Your SQL says *what*; the optimizer decides *how*. It estimates each operation's cost from table statistics (row counts, value distributions) and picks the cheapest plan. When a query is slow, the plan tells you where the time went — a full scan where an index scan should be, a hash join where a nested loop would do, or an estimate wildly off from reality.

## The three join algorithms

| Algorithm | When | Cost |
|-----------|------|------|
| **Nested loop** | Small outer × indexed inner | $O(n \cdot \log m)$ with index; great for small outer |
| **Hash join** | Large unsorted relations, equality condition | $O(n + m)$ — build hash on one side, probe with the other |
| **Merge join** | Both sides sorted on the join key | $O(n + m)$ — merge like merge sort |

## Reading EXPLAIN

```sql
EXPLAIN (ANALYZE, BUFFERS) SELECT ...;
-- PostgreSQL; MySQL: EXPLAIN ANALYZE
```

```text
Seq Scan on Orders  (cost=0.00..19372.00 rows=1000000 width=...)
  Filter: (customer_id = 7)
  ->  Index Scan using idx_orders_customer on Orders
      (cost=0.29..8.30 rows=42 width=...)
```

**What to look for:**
1. **Seq Scan** on a big table with a selective filter → missing index
2. **Rows estimate vs actual** — wildly off means stale statistics (`ANALYZE`)
3. **Nested loop on two huge tables** — wants a hash join
4. **Sort** of a huge set — could an index provide the order?

## The common killers

```text
1. MISSING INDEX on the WHERE/JOIN predicate  → Seq Scan
2. FUNCTION on the column: WHERE UPPER(name) = 'X'  → index unusable
3. LEADING WILDCARD: LIKE '%foo%'  → index unusable
4. SELECT * pulling unneeded columns  → no covering index, more I/O
5. OR / IN with widely varying selectivities  → optimizer may scan
6. Correlated subqueries running per-row  → rewrite as JOIN
7. OFFSET pagination on huge tables  → scans skipped rows; keyset pagination
8. Stale statistics  → bad estimates → bad plans
```

## The fixing playbook

**1. Add the index** (the #1 fix):
```sql
CREATE INDEX idx_orders_customer ON Orders(customer_id);
-- Consider composite: (customer_id, status) for multi-predicate filters
```

**2. Make predicates sargable** ("search-argument-able"):
```sql
-- BAD:  WHERE DATE(created_at) = '2026-01-01'
-- GOOD: WHERE created_at >= '2026-01-01' AND created_at < '2026-01-02'
```

**3. Rewrite correlated subqueries as joins:**
```sql
-- BAD (runs the subquery per row):
SELECT s.name FROM Student s
WHERE EXISTS (SELECT 1 FROM Enrolled e WHERE e.sid = s.sid AND e.grade='A');
-- The optimizer often rewrites this itself; but with poor stats,
-- explicit JOIN can force the better plan.

-- GOOD:
SELECT DISTINCT s.name
FROM Student s JOIN Enrolled e ON s.sid = e.sid
WHERE e.grade = 'A';
```

**4. Keyset pagination instead of OFFSET:**
```sql
-- BAD for page 10,000:
SELECT * FROM Orders ORDER BY id LIMIT 20 OFFSET 199980;
-- GOOD (remembers where you were):
SELECT * FROM Orders WHERE id > 199980 ORDER BY id LIMIT 20;
```

## Statistics — the optimizer's eyes

```sql
ANALYZE Orders;          -- update statistics (autovacuum does this periodically)
-- Per-table: row counts, distinct values per column, histograms.
-- Wrong estimates → wrong plan (e.g., a "small" table that's actually huge).
```

## Index bloat & maintenance

```text
REINDEX / REORGANIZE / OPTIMIZE — rebuild indexes that fragmented
VACUUM (Postgres) — reclaim dead row versions (MVCC bloat)
Over time, heavy writes fragment indexes → scans get slower →
periodic maintenance keeps plans fast.
```

---

**Setup:** A query joins two 10M-row tables and takes minutes. What's the first move?

**Solution:** `EXPLAIN` it. If the plan shows a Nested Loop over both (without index), the fix is:
```sql
CREATE INDEX idx_child_fk ON child(parent_id);   -- index the FK
```
The nested loop then probes the index instead of scanning the inner table per outer row.

**Key insight:** The FK column is the *first* index to add — every parent→child join benefits. Without it, each of the outer's 10M rows triggers a full inner scan: $O(n \cdot m)$ — catastrophic.

---

**Setup:** `WHERE status = 'PAID'` on a table where 99% of rows are PAID.

**Solution:** The optimizer *should* choose a Seq Scan — scanning 10M rows is cheaper than 9.9M index lookups. An index on `status` is nearly useless here (low selectivity). Don't "fix" what the plan already got right.

**Key insight:** Indexes help selective predicates. The optimizer knows — forcing an index (`SET enable_seqscan=off`) usually makes it *worse*. The right move is trusting the plan and reading why it chose the scan.

---

**Setup:** A report query joins 5 tables and sorts a huge result. Tune it.

**Solution:**
1. Index the join keys and the WHERE columns
2. Push `WHERE` filters into each table *early* (the optimizer does this, but views/CTEs can block it — check the plan)
3. Use a covering index for the hot path (index-only scan, no table fetch)
4. If sorting dominates, add an index on the sort column(s) so the index provides order
5. Materialize heavy CTEs or use a reporting table for *frequent* versions

**Key insight:** The plan is a tree — find the widest nodes (full scans of big tables) and make them narrow. Joins scale with the *smallest* side's filtered size, so shrink inputs first.

---

**Setup:** Same query is fast in dev, slow in production.

**Solution:** Statistics differ — production data distribution (and volume) is different. Likely causes:
1. Stale stats → bad join order (run `ANALYZE`)
2. Missing production indexes (dev applied migrations, prod didn't)
3. Parameter sniffing / plan caching on a different-shape first run
4. Hardware/IOPS differences

**Key insight:** "Works in dev" is about *data shape*, not just volume. Verify the production plan with `EXPLAIN` on production-shaped data. This is why staging that mirrors production data exists.

---

## Practice (try before peeking)

1. When is a hash join better than a nested loop?
2. `WHERE YEAR(date_col) = 2026` — why might it scan?
3. What's the first index to add in a new schema?

<details><summary>Answers</summary>

1. When both inputs are large and unsorted, joined on equality — the hash build+probe avoids the inner index probes of nested loop.
2. `YEAR()` on the column defeats the index — rewrite as a range `date_col >= '2026-01-01' AND date_col < '2027-01-01'`.
3. The primary key (auto) plus indexes on every foreign key column — FK indexes serve the most common joins and cascade deletes.

</details>

---

**Common traps:**
- Tuning by guessing instead of reading `EXPLAIN` — the plan is the map
- Forcing index usage — the optimizer's choice is usually right; fight it only with evidence
- Neglecting statistics (`ANALYZE`) — stale stats cause terrible plans
- SELECT * — pulls every column, blocking covering-index scans
- Optimizing a query that's run once by an admin instead of the hot path — tune the 90%

---
