# Normal Forms & Decomposition

Normalization removes redundancy by decomposing tables — and every redundancy traces to an **FD violation**. The normal forms (1NF → BCNF) are the ladder of cleanliness; the decomposition must be **lossless** (rejoin gives back the original data) and preferably **dependency-preserving**.

**The Intuition:** A badly designed table stores the same fact in many places. Update the fact in one row and the others drift — that's an *update anomaly*. Normalization splits the table so each fact lives in exactly one place. The rules for splitting come straight from the functional dependencies.

## The anomalies

| Anomaly | Symptom | Example (Employee with dept_name) |
|---------|---------|-----------------------------------|
| Insert | Can't record a fact without a key | New dept with no employee yet |
| Update | Must change same fact in many rows | Renaming a dept updates 50 rows |
| Delete | Deleting rows deletes facts you wanted | Firing the last engineer deletes the dept |

## The normal forms — ladder of quality

**1NF — atomic values**
Every cell holds one value (no lists, no repeating groups).
```text
BAD: Student( name, courses="CS101, CS102" )
GOOD: separate rows or a separate table
```

**2NF — 1NF + no partial dependency**
Every non-prime attribute depends on the *whole* candidate key, not part of it.
```text
R(StudentID, CourseID, Instructor, InstructorEmail)
Keys: (StudentID, CourseID)
InstructorEmail depends on Instructor, which depends only on CourseID
  — a PARTIAL dependency (on part of the composite key) → 2NF violation
```

**3NF — 2NF + no transitive dependency**
No non-prime attribute depends on another non-prime attribute.
```text
Employee(emp_id, dept_id, dept_name)
dept_id → dept_name: non-prime depends on non-prime → 3NF violation
```

**BCNF — every determinant is a superkey**
Stronger than 3NF. For *every* FD X→Y, X must be a superkey.
```text
Fails BCNF: Professor(p_id, p_name, dept, office)
FD dept → office (each dept has one office) — dept is NOT a superkey
```

## Which to target?

| Normal form | Fixes | Cost |
|-------------|-------|------|
| 1NF | non-atomic cells | — |
| 2NF | partial-key dependencies | — |
| 3NF | transitive dependencies | always dependency-preserving |
| BCNF | remaining FDs with non-superkey determinants | may LOSE dependencies |

**Practical guidance:** BCNF when possible; 3NF when a BCNF decomposition would lose an FD you need to enforce.

## The decomposition algorithm (BCNF)

```text
Given R with FDs F, and a violating FD X → Y (X not a superkey):

  Decompose R into:
    R1 = X ∪ Y          (the violating FD becomes a key)
    R2 = R − Y          (everything except the determined attributes)

  Repeat on each piece until every table is in BCNF.
```

**Lossless guarantee:** the shared attribute set of R1, R2 (which contains X) must be a key of R1 — and it is, because X→Y makes X a key of R1. This is the *lossless-join* test: $R_1 \cap R_2 \to R_1$ (or $R_2$).

## Lossless join — why it matters

**Lossy decomposition creates FAKE rows** when rejoining:
```text
R(A,B,C) with FD B→C
Split into R1(A,B) and R2(B,C):
  rejoin R1 ⋈ R2 on B → exactly the original rows (B determines C, so
  no spurious combinations appear). LOSSLESS ✓

Counter-example: split on a non-key, non-FD attribute → rejoining
produces rows that never existed (e.g., a student's name matching
another student's course).
```

**Test:** $R_1 \cap R_2 \to R_1$ or $R_1 \cap R_2 \to R_2$ — the shared attributes must be a superkey of one part.

## Dependency preservation

A decomposition preserves dependencies if every FD can be checked within a single table:
```text
R(A,B,C) FDs: A→B, B→C
Decompose: R1(A,B), R2(A,C)
  A→B preserved in R1 ✓
  B→C LOST — B and C are never together in one table; enforcing it
  requires a join per update. NOT dependency-preserving.

Better: R1(A,B), R2(B,C) — preserves both.
```

---

**Setup:** R(A,B,C,D) with FDs AB→C, C→D. Normalize to BCNF.

**Solution:**
```text
Candidate keys: AB (AB→C→D, so AB→{A,B,C,D}).
Violation: C→D — C is not a superkey.
Decompose:
  R1 = {C,D}  (the violating FD) — key C
  R2 = {A,B,C} (R − D) — FDs: AB→C
R1: C→D, key C → BCNF ✓
R2: AB→C, key AB → BCNF ✓
Lossless: R1∩R2 = {C}, C→D so C is key of R1 ✓
Dependency-preserving: AB→C in R2, C→D in R1 ✓
```

**Key insight:** The violation tells you the split — group the dependent attributes with their determinant. This decomposition is both lossless and dependency-preserving, so it's the right one.

---

**Setup:** R(A,B,C) with FDs A→B, B→C. Decompose to BCNF; is it dependency-preserving?

**Solution:**
```text
Key: A. Violation: B→C (B not a superkey).
Split: R1={B,C}, R2={A,B}.
R1: B→C, key B ✓ BCNF. R2: A→B, key A ✓ BCNF.
Lossless ✓ (B shared, key of R1). Preserves BOTH A→B and B→C ✓.
```

**Key insight:** Sometimes the BCNF split *happens* to preserve everything. The danger case is when a violating FD involves attributes already split apart — then BCNF forces a choice.

---

**Setup:** R(A,B,C,D,E) FDs AB→C, AB→D, C→E. Decompose to 3NF, dependency-preserving.

**Solution:** Key AB. C→E violates 3NF (transitive through C). Split:
```text
R1 = {C, E}  (violating FD C→E) — key C
R2 = {A, B, C, D} — key AB; FDs AB→C, AB→D
R2 in BCNF? AB is the only determinant and is a superkey ✓.
Final: R1(C,E), R2(A,B,C,D) — lossless, dependency-preserving.
```

**Key insight:** 3NF normalization follows the same "split on the violation" pattern. 3NF's promise (unlike BCNF) is that a dependency-preserving decomposition *always exists* — BCNF can fail that.

---

**Setup:** Give a real-world 3NF violation and its fix.

**Solution:**
```text
Order(order_id, customer_id, customer_name, product)
  customer_id → customer_name   (transitive via customer_id — non-prime → non-prime)
Fix: split Customer(customer_id, customer_name) and Order(order_id, customer_id, product).
Now customer_name lives in exactly one table — rename once, no drift.
```

**Key insight:** This is the canonical real-world fix: "repeated customer data in every order" disappears once the transitive dependency is broken. The normalization *is* the good design.

---

## Practice (try before peeking)

1. Which normal form forbids "dept_id → dept_name" where dept_id isn't a key?
2. Why is a lossy decomposition dangerous?
3. When would you deliberately stop at 3NF instead of BCNF?

<details><summary>Answers</summary>

1. BCNF (and 3NF also rejects it if dept_name is non-prime). 2NF doesn't catch it — it's transitive, not partial.
2. Rejoining a lossy split produces rows that never existed (spurious tuples) — wrong answers from a schema that looks fine.
3. When the BCNF decomposition loses an FD that must be enforced — 3NF guarantees a dependency-preserving decomposition exists.

</details>

---

**Common traps:**
- Stopping at 1NF/2NF thinking it's enough — the transitive dependencies are the common real-world ones
- Splitting on attributes that don't share a key with the rest → lossy
- Believing "every split is lossless" — always test the shared-attributes condition
- Forgetting candidate keys before normalizing — you can't judge BCNF without them
- Over-normalizing for read-heavy analytical workloads — denormalization is a legitimate design choice (star schemas)

---
