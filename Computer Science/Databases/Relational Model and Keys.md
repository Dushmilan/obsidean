# Relational Model & Keys

The relational model represents data as **relations** (tables): sets of **tuples** (rows) with a fixed **schema** (columns). Its power is that queries are expressed *declaratively* — you describe the result, not the procedure. Keys give rows identity and connect tables. This is the mathematical foundation of SQL.

**The Intuition:** A table is a set of facts. Each row is one fact ("student 42 is named Ada, age 36"); each column is one attribute. Because a relation is a *set*, rows are distinct and unordered — no duplicates, no implicit order. Keys are how you point at a specific row — like a primary key is the row's identity card, and a foreign key is "this row refers to that row."

## Schema vs instance

```text
SCHEMA (the blueprint — static):
  Student(sid: int, name: varchar, age: int)
  ^table   ^attributes with domains

INSTANCE (the data — changes over time):
  sid | name | age
  1   | Ada  | 36
  2   | Bob  | 25
```

A relation $R \subseteq \text{dom}(A_1) \times \cdots \times \text{dom}(A_n)$ — a subset of the Cartesian product of its domains.

## Keys

| Key type | Definition | Notes |
|----------|-----------|-------|
| **Superkey** | a set of attributes that uniquely identifies a row | minimality not required |
| **Candidate key** | a *minimal* superkey | no proper subset is a superkey |
| **Primary key (PK)** | the chosen candidate key | one per table; NOT NULL + UNIQUE |
| **Foreign key (FK)** | attributes referencing another table's PK | enforces referential integrity |
| **Composite key** | a key with multiple attributes | e.g., (course_id, term) |
| **Surrogate key** | an artificial key (auto-increment id) | when no natural key exists |

```sql
CREATE TABLE Student (
    sid  INT PRIMARY KEY,          -- candidate key, chosen as PK
    name VARCHAR(50) NOT NULL,
    age  INT
);

CREATE TABLE Enrolled (
    sid  INT REFERENCES Student(sid),   -- FOREIGN KEY
    cid  INT REFERENCES Course(cid),
    grade CHAR(2),
    PRIMARY KEY (sid, cid)              -- composite candidate key
);
```

## Referential integrity

A foreign key value must either be NULL or match an existing primary key in the referenced table. The DBMS enforces it — you can't enroll a student that doesn't exist. On delete/update of the referenced row, you choose a policy:

```sql
ON DELETE CASCADE      -- delete the referencing rows too
ON DELETE SET NULL     -- null out the FK
ON DELETE RESTRICT     -- refuse the delete if references exist
```

## The integrity constraints

| Constraint | Guarantees |
|-----------|-----------|
| Domain | attribute values are in their type's domain |
| NOT NULL | column always has a value |
| UNIQUE | no duplicate values in the column |
| CHECK | arbitrary predicate on the row |
| PRIMARY KEY | NOT NULL + UNIQUE (a candidate key) |
| FOREIGN KEY | value exists in the referenced table |
| Entity integrity | PK is never NULL |
| Referential integrity | FK points at a real row |

## Why "set" semantics matter

Because a relation is a set:
- **No duplicate rows** (mathematically) — though SQL defaults to *multisets* (bags) unless you say `SELECT DISTINCT`
- **No ordering** — tables have no inherent order; `ORDER BY` is the only way to get a specific sequence
- **A row is a fact** — the same fact can't appear twice

---

**Setup:** A `Grade` table has (student_name, course_name, grade). What's wrong with using student_name as part of the key?

**Solution:** Names aren't unique — two "Ada" students collide, breaking uniqueness, and a name change forces updates everywhere (update anomalies). Use surrogate keys:
```sql
Student(sid PK, name)
Course(cid PK, title)
Grade(sid FK, cid FK, grade, PK(sid, cid))
```

**Key insight:** Natural attributes (names) make poor keys. The "student number" is a *natural* key that happens to work; when none exists, generate a surrogate. The schema decomposition also removes redundancy — the name lives in exactly one table.

---

**Setup:** Why can't a primary key be NULL?

**Solution:** The PK uniquely identifies each row — a NULL would be an unidentified row (entity integrity). Also, foreign keys reference PK values; "this FK references NULL" is meaningless. NULL means "unknown," and an unknown identity is a contradiction.

**Key insight:** NULL semantics are the deepest source of SQL surprises. Every constraint that "should" treat NULL specially does — NOT NULL, UNIQUE (multiple NULLs allowed in many DBs!), and comparisons (`col = NULL` is never true).

---

**Setup:** `Enrolled(sid, cid)` with `ON DELETE CASCADE` — what happens deleting a Student row?

**Solution:** The DBMS deletes the student AND every Enrolled row with that sid. CASCADE propagates the deletion to referencing rows — consistent but destructive. RESTRICT would refuse; SET NULL would orphan the enrollments with `sid = NULL`.

**Key insight:** The delete policy is a *data-integrity decision*: cascade for "ownership" relationships (order → order items), restrict for "must exist" relationships. Choosing wrong leads to either dangling references or accidentally wiping history.

---

**Setup:** Is `(first_name, last_name)` a candidate key for a Users table?

**Solution:** Only if the pair is *minimal* and *unique*. It's unique only if no two users share both names (false in general) — so it's not even a superkey, let alone a candidate. The real candidate might be `email` or an auto-increment id.

**Key insight:** A candidate key is a *claim about the domain*: "these attributes, together, are guaranteed distinct." Design the schema around genuine uniqueness constraints, and use surrogate keys when reality doesn't provide one.

---

## Practice (try before peeking)

1. Difference between superkey and candidate key?
2. Can a table have two primary keys?
3. `ON DELETE SET NULL` — when does it make sense?

<details><summary>Answers</summary>

1. A candidate key is a *minimal* superkey — no subset still uniquely identifies rows. A superkey can have redundant attributes.
2. No — one primary key per table (which may be composite: multiple columns forming it).
3. When the FK is optional and orphans are acceptable — e.g., an order's assigned salesperson is deleted: keep the order, null the assignment.

</details>

---

**Common traps:**
- Using nullable columns as keys — NULL identity is broken
- Natural keys that drift (names, emails change) — prefer stable/surrogate keys
- Composite PKs forgotten — the row can't be identified without the full pair
- Assuming tables have order — they don't; `ORDER BY` is required
- Believing duplicate rows are impossible — SQL allows them unless `DISTINCT`/constraints forbid

---
