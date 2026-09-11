# Storage Engines & NoSQL

Under the SQL sits a **storage engine** deciding how rows and indexes live on disk. Beyond relational databases, the **NoSQL** family trades relational guarantees for scale or flexibility. Understanding the engine (InnoDB, Postgres heap, LSM) and the NoSQL spectrum (key-value, document, wide-column, graph) lets you *choose* rather than *default*.

**The Intuition:** The storage engine is the database's file system — it decides row layout, index format, and crash recovery. Two databases with the same SQL surface can perform wildly differently because of the engine. NoSQL engines answer a different question: "I don't need joins/transactions — I need horizontal scale or a flexible schema." Choose the tool for the access pattern.

## Engine architectures

### Row-oriented (OLTP default: InnoDB, Postgres heap)

```text
Rows stored contiguously — reading a whole row = one page fetch.
Optimized for: point lookups, updates, transactions (row reads/writes).

InnoDB: clustered by PK; secondary indexes store PK as row pointer.
Postgres: heap file + separate index files pointing to (page, slot);
  UPDATE writes a NEW row version (MVCC), old one vacuumed.
```

### Column-oriented (OLAP: ClickHouse, Redshift)

```text
Columns stored contiguously — a column read touches only those pages.
Compression shines (same-type values). Optimized for:
aggregations over few columns of huge tables.
Bad at: point updates, single-row operations.

"Row stores = you touch a row, get it whole.
 Column stores = you touch a column, get it whole."
```

### LSM-tree (write-optimized: RocksDB, Cassandra, LevelDB, Bigtable)

```text
Writes go to an in-memory memtable, flushed to immutable sorted
SSTable files; background compaction merges them.

Write: O(1)-ish append — no random writes, no page splits.
Read:  must check memtable + multiple SSTables (bloom filters help).

Great when: writes dominate (logs, metrics, event ingestion).
Weak when: point reads dominate (must merge-search many files).
```

| Engine | Writes | Reads | Used by |
|--------|--------|-------|---------|
| B-tree (InnoDB) | good | excellent | MySQL default |
| Heap (Postgres) | good (MVCC) | excellent | Postgres |
| Column store | poor | great for analytics | ClickHouse, Redshift |
| LSM | excellent | good | Cassandra, RocksDB |

## The NoSQL spectrum

### Key-value stores (Redis, Memcached)
```text
Model: GET/SET by key — the simplest possible access
Use: caches, sessions, counters, leaderboards
Trade-off: no queries beyond keys; everything is the app's job
```

### Document stores (MongoDB, CouchDB)
```text
Model: JSON-like documents, flexible schemas
Use: content, catalogs, user profiles, "the schema keeps changing"
Trade-off: no joins (embed or denormalize), transactions limited (per-doc in old versions)
```

### Wide-column (Cassandra, HBase, Bigtable)
```text
Model: rows with many columns, ordered by partition key + sort key
Use: event streams, time-series, IoT, massive write throughput
Trade-off: query pattern must match the key design — inflexible ad-hoc queries
```

### Graph databases (Neo4j)
```text
Model: nodes + edges as first-class
Use: social graphs, recommendation, fraud detection, dependency analysis
Trade-off: powerful for traversals; weaker for aggregate analytics
```

## When to go NoSQL — the honest checklist

```text
GO NoSQL when:
  - You must scale writes horizontally (beyond one server's B-tree)
  - The schema genuinely changes fast (documents)
  - The access pattern is simple and known in advance (key-value)
  - You need graph traversals as the core operation

STAY SQL when:
  - You need joins, transactions, or ad-hoc reports
  - Consistency/durability guarantees are non-negotiable (money!)
  - The workload fits one node (it usually does!)
  - Your team already knows SQL

The default should be SQL. Most "NoSQL needed" cases are really
"I need caching" or "I need to shard later."
```

## CAP theorem — the constraint

```text
In a distributed system under PARTITION, you choose:
  Consistency   — every read sees the last write
  Availability  — every request gets a response
  Partition tolerance — the system keeps working with network splits

CAP: you can pick at most two. Since partitions happen, you effectively
choose CP (consistent — MongoDB, HBase) or AP (available — Cassandra, Dynamo).

BASE (NoSQL) vs ACID (SQL):
  Basically Available, Soft state, Eventually consistent
  — the price of availability under partitions.
```

---

**Setup:** Pick an engine for a time-series metrics system writing 100k points/sec.

**Solution:** LSM or column store — Cassandra (wide-column, LSM, write-optimized) or a column store for analytics. B-tree InnoDB would choke on 100k random-ish writes/sec, and you don't need row-level transactions for metrics.

**Key insight:** The workload (write-heavy, append-mostly, query-by-time-range) *determines* the engine. Write-heavy + point-query-light → LSM. Analytics over big history → column store. This is the choice process in miniature.

---

**Setup:** When is a document store better than SQL for a catalog app?

**Solution:** When products have wildly varying attributes ("an e-book has pages, a laptop has RAM, a T-shirt has sizes") — documents store each item with *its own* fields, no `product_attributes` EAV table or dozens of nullable columns. Reads fetch the whole document in one go.

**Key insight:** The schema flexibility is real, but you lose joins and ad-hoc reporting. A common compromise: SQL for the transactional core + a document store (or JSON column) for the flexible part.

---

**Setup:** A caching layer for hot reads — which store?

**Solution:** Redis (or Memcached) — key-value, in-memory, microsecond reads. The relational DB stays the source of truth; the cache holds hot keys with TTLs. Cache invalidation (write-through, TTL, or event-driven) is the hard part — not the store choice.

**Key insight:** "NoSQL needed" usually means "cache needed." A key-value cache in front of your SQL database solves the read hot-spot without abandoning relational guarantees. Cache the *result*, not the whole table.

---

**Setup:** You need graph traversals ("friends of friends who bought X") — options?

**Solution:** A graph DB (Neo4j) if traversals are the core workload; otherwise, consider whether the traversal depth is bounded (2-3 levels → SQL with joins/recursive CTEs is fine). Graph DBs make depth-unbounded traversals natural; SQL makes them painful.

**Key insight:** "Graph" is a *workload* property, not just a data property. If your query pattern is always "neighbors within a few hops," SQL can handle it. Only deep/unbounded traversal-heavy workloads justify the graph database.

---

## Practice (try before peeking)

1. Column store: good for what read pattern?
2. CAP: MongoDB is typically CP or AP?
3. Cassandra's write advantage comes from what structure?

<details><summary>Answers</summary>

1. Aggregations over few columns of huge tables (analytics) — column reads touch only those columns, compress well.
2. CP by default (configurable) — consistency over availability during partitions (single-primary).
3. LSM-tree: writes hit an in-memory memtable and append immutable SSTables — no random writes, no page splits, no read-modify-write.

</details>

---

**Common traps:**
- Choosing NoSQL "because it's modern" — the relational default is right for most workloads
- Ignoring the write path — an engine that reads fast can still write slow (B-tree random writes)
- "Eventually consistent" surprises — stale reads are a *design choice*, know when your app can't tolerate them
- Using a document store where you need joins/transactions — you'll reimplement them badly
- Benchmarking with a toy dataset — NoSQL scaling advantages appear at size; choose by workload, not hype

---
