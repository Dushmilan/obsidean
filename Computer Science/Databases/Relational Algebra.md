# Relational Algebra

Relational algebra is the formal language underneath SQL — a small set of operations on relations that compose into any query. It's *procedural* (you specify how to compute) whereas SQL is *declarative* (you specify what you want). The optimizer rewrites your SQL into an algebraic plan, then reorders it using equivalence rules.

**The Intuition:** The algebra is a toolkit of relation-to-relation operations: filter rows (select), pick columns (project), combine relations (join/union). Every SQL query — however complex — compiles to an algebraic expression. Understanding the algebra means you can reason about what a query *means* and *how it might be evaluated*, which is the difference between writing queries and *tuning* them.

## The six core operators

```text
1. SELECTION   σ_θ(R)      — filter ROWS by predicate θ
2. PROJECTION  π_A(R)      — keep only columns A (dedupes!)
3. RENAME      ρ_S(R)      — rename relation or attributes
4. UNION       R ∪ S       — rows in either (compatible schemas)
5. DIFFERENCE  R − S       — rows in R not in S
6. PRODUCT     R × S       — all pairs (cross join)
```

Plus the derived operators:
```text
INTERSECTION R ∩ S = R − (R − S)
JOIN         R ⨝_θ S = σ_θ(R × S)      (theta join)
NATURAL JOIN R ⨝ S   — equi-join on all common attributes, merged
DIVISION     R ÷ S   — "all" queries
```

## Selection & projection — the atoms

```text
σ_age>18(Student)      — rows where age > 18
π_name,age(Student)    — just name and age columns
π_sid(σ_major='CS'(Student))   — CS majors' ids (compose!)
```

**Projection removes duplicates** (set semantics). SQL's `SELECT` doesn't unless you add `DISTINCT` — a famous mismatch.

## The join family

```text
R ⨝ S           natural join — match on common attribute names
R ⨝_θ S         theta join — arbitrary condition (e.g., price ≤ budget)
R ⨝_{R.a=S.a} S equi-join — equality on the condition
R ⟕ S           left outer join — keep unmatched R rows, NULL-fill
R ⟖ S           right outer join
R ⟗ S           full outer join
```

**Why outer joins exist:** plain joins *drop* unmatched rows. Outer joins keep one side (or both) and NULL-fill the missing side. "All customers, even those with no orders" needs a left outer join.

## Division — the "for all" operator

```text
R ÷ S answers: "which values appear with ALL of S's values?"

Grades(sid, cid), Courses(cid)
  →  Grades ÷ Courses  =  students who took EVERY course
```

**There's no SQL keyword** — you simulate it with double negation (`NOT EXISTS ... NOT EXISTS`). Division is the only algebra operator that expresses universal quantification.

## Equivalence rules — the optimizer's toolkit

```text
σ_θ(σ_φ(R)) = σ_{θ∧φ}(R)               — merge/cascade selections
σ_θ(R ⨝ S) = σ_θ(R) ⨝ S  (θ uses only R) — push selection DOWN
π_A(π_B(R)) = π_A(R)  (A ⊆ B)           — cascade projections
σ_θ(R × S) = R ⨝_θ S                    — selection + product = join
R ⨝ S = S ⨝ R                            — join commutativity
(R ⨝ S) ⨝ T = R ⨝ (S ⨝ T)               — join associativity
```

**The golden rule: push selections down.** Filtering earlier reduces what flows into joins — often 100× faster.

## SQL ↔ algebra correspondence

```sql
SELECT name
FROM Student s, Enrolled e
WHERE s.sid = e.sid AND s.major = 'CS';
```
```text
π_name( σ_s.major='CS' ∧ s.sid=e.sid (Student × Enrolled) )
      = π_name( σ_major='CS'(Student) ⨝ Enrolled )     — after pushing selection
```

---

**Setup:** Express "names of CS majors enrolled in CS101 with grade ≥ B" in algebra.

**Solution:**
```text
π_name( σ_major='CS'(Student)
        ⨝
        σ_cid='CS101' ∧ grade≥'B'(Enrolled) )
```

**Key insight:** Push selections into each table *before* joining — the optimizer does exactly this. The join then works on filtered (smaller) inputs.

---

**Setup:** "Students who took every course" — with and without division.

**Solution:** Division:
```text
π_sid,cid(Enrolled) ÷ π_cid(Course)
```
SQL (double negation):
```sql
SELECT DISTINCT sid FROM Enrolled e1
WHERE NOT EXISTS (
    SELECT 1 FROM Course c
    WHERE NOT EXISTS (
        SELECT 1 FROM Enrolled e2
        WHERE e2.sid = e1.sid AND e2.cid = c.cid
    )
);
```

**Key insight:** "Every X" always reduces to a double `NOT EXISTS` in SQL — "no course exists that this student didn't take." Division is the compact mathematical spelling of the same idea.

---

**Setup:** Find students NOT enrolled in any course.

**Solution:** Difference: $\pi_{sid}(Student) - \pi_{sid}(Enrolled)$.
```sql
SELECT sid FROM Student
EXCEPT
SELECT sid FROM Enrolled;
```
Or: `SELECT s.sid FROM Student s LEFT JOIN Enrolled e ON s.sid=e.sid WHERE e.sid IS NULL;`

**Key insight:** Three equivalent spellings (EXCEPT, NOT IN, LEFT JOIN IS NULL). The algebra's difference operator IS the set difference — and SQL's `EXCEPT`/`MINUS` is the direct translation. NOT IN has the NULL trap (see the SQL note).

---

**Setup:** Show the optimizer's plan for joining three tables.

**Solution:** The algebra tree is associative/commutative — the optimizer picks the *join order* by estimated sizes:
```text
π (...) ( (σ(Student) ⨝ Enrolled) ⨝ Course )        — join order 1
     vs
π (...) ( σ(Student) ⨝ (Enrolled ⨝ Course) )        — join order 2
```
It estimates the size of each intermediate and picks the cheapest order (usually smallest-first with indexes).

**Key insight:** The optimizer's job is choosing among *equivalent* algebra trees. Your `FROM` order hints, but the optimizer reorders by cost estimates (statistics: row counts, value distributions). This is why `EXPLAIN` output shows the *chosen* plan — not your SQL's syntax order.

---

## Practice (try before peeking)

1. What does π_sid(σ_grade='A'(Enrolled)) return? How many rows per A-student?
2. Natural join of Student(sid,name) and Enrolled(sid,cid) — what's the join attribute?
3. Does `SELECT` return a set or a bag by default?

<details><summary>Answers</summary>

1. The sids of students with an A — one row *per A enrollment* (projection dedupes at the algebra level; SQL needs DISTINCT).
2. `sid` — the common attribute; natural join merges it into one column.
3. A bag (multiset) — duplicates allowed by default. Algebra's projection is a set (deduped). Use `DISTINCT` to match algebra semantics.

</details>

---

**Common traps:**
- Confusing selection (rows) with projection (columns)
- Natural join on no common attributes = cross product (accidental)
- Division semantics — most misunderstood operator
- Projection dedupes in algebra but not in SQL
- Writing SQL without thinking about the algebra — the optimizer's plan is the algebra

---
