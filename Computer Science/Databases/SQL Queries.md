# SQL Queries

SQL is declarative: you describe the result, the engine computes it. The deep skill is understanding the **logical execution order** — SQL's clauses run in a fixed sequence regardless of how you write them. Almost every SQL bug traces to a misunderstanding of that order, or of NULL semantics.

**The Intuition:** Writing SQL is describing the answer to a literal-minded clerk. "Take the students table, keep only CS majors, group by year, count each group, show me the count where it's above 10." The clerk (optimizer) figures out the fastest physical way, but the *logical* pipeline is fixed: FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT.

## The logical execution order

```sql
SELECT ...                 -- 6: compute/alias expressions
FROM ...                   -- 1: build the working set (joins)
WHERE ...                  -- 2: filter ROWS
GROUP BY ...               -- 3: form groups
HAVING ...                 -- 4: filter GROUPS
ORDER BY ...               -- 7: sort the result
LIMIT ...                  -- 8: truncate
-- (DISTINCT sneaks between 6 and 7)
```

**Consequences:**
- `WHERE` can't reference aggregates (`COUNT(*)`) — they don't exist yet → `HAVING` is for that
- Column aliases from `SELECT` can't be used in `WHERE` (alias created at step 6) but CAN in `ORDER BY` (step 7)
- `ORDER BY` can reference anything from the final select list

## The basic shapes

```sql
-- Filtering
SELECT name, age FROM Student WHERE age >= 18 AND major = 'CS';
SELECT name FROM Student WHERE major IN ('CS', 'Math');
SELECT name FROM Student WHERE age BETWEEN 18 AND 25;
SELECT name FROM Student WHERE name LIKE 'A%';      -- % wildcard, _ single
SELECT name FROM Student WHERE major IS NULL;       -- NEVER use = NULL

-- Ordering & limit
SELECT * FROM Student ORDER BY age DESC, name ASC LIMIT 10;

-- Distinct
SELECT DISTINCT major FROM Student;

-- Aggregation
SELECT major, COUNT(*), AVG(age), MIN(age), MAX(age)
FROM Student GROUP BY major;
```

## NULL — the third truth value

```sql
-- NULL means "unknown". Comparisons with NULL yield NULL (not true/false):
WHERE age = NULL        -- NEVER true — matches nothing!
WHERE age IS NULL       -- correct
WHERE age IS NOT NULL

-- NULL propagates through arithmetic:
SELECT NULL + 1;        -- NULL
SELECT NULL = NULL;     -- NULL (not true!)

-- WHERE filters out NULL results:
-- "col != 'x'" excludes rows where col IS NULL — silently!
-- COALESCE fixes it:
WHERE COALESCE(col, '') != 'x'
```

## GROUP BY & HAVING

```sql
-- Average grade per course, only courses with > 5 students:
SELECT cid, AVG(grade) AS avg_grade
FROM Enrolled
GROUP BY cid
HAVING COUNT(*) > 5
ORDER BY avg_grade DESC;

-- Every column in SELECT must be aggregated OR in GROUP BY:
SELECT cid, grade, AVG(...)   -- ERROR: grade not in GROUP BY
```

## Joins

```sql
SELECT s.name, e.cid, e.grade
FROM Student s
JOIN Enrolled e ON s.sid = e.sid;

-- Inner vs outer:
-- INNER: only matching rows
-- LEFT:  all left rows + matches (NULL-fill unmatched right)
-- RIGHT / FULL: the other sides / both

SELECT s.name, COUNT(e.cid)     -- every student, even with no enrollments:
FROM Student s
LEFT JOIN Enrolled e ON s.sid = e.sid
GROUP BY s.name;
-- Without LEFT, students with no enrollments vanish entirely!
```

## Subqueries

```sql
-- In WHERE:
SELECT name FROM Student
WHERE sid IN (SELECT sid FROM Enrolled WHERE cid = 'CS101');

-- Correlated (runs per outer row):
SELECT s.name FROM Student s
WHERE EXISTS (
    SELECT 1 FROM Enrolled e
    WHERE e.sid = s.sid AND e.grade = 'A'
);

-- In FROM (derived table):
SELECT dept, cnt FROM (
    SELECT dept, COUNT(*) AS cnt FROM Employee GROUP BY dept
) AS t WHERE cnt > 10;

-- In SELECT (scalar subquery):
SELECT name,
       (SELECT MAX(grade) FROM Enrolled e WHERE e.sid = s.sid) AS top_grade
FROM Student s;
```

## Set operations

```sql
SELECT cid FROM Course WHERE dept = 'CS'
UNION                   -- dedupe
SELECT cid FROM Course WHERE dept = 'Math';

-- UNION ALL (keeps duplicates, faster)
-- INTERSECT, EXCEPT (MINUS in some DBs)
```

---

**Setup:** Find departments with an average salary above 60k, sorted.

**Solution:**
```sql
SELECT dept, AVG(salary) AS avg_sal
FROM Employee
GROUP BY dept
HAVING AVG(salary) > 60000
ORDER BY avg_sal DESC;
```

**Key insight:** The `HAVING` filters *groups* (post-aggregation); `WHERE` would filter *rows* pre-aggregation. `AVG(salary)` in HAVING can't go in WHERE. The alias `avg_sal` is usable in ORDER BY (last step).

---

**Setup:** List each employee with their department name (not just the dept id).

**Solution:**
```sql
SELECT e.name, d.dept_name
FROM Employee e
JOIN Department d ON e.dept_id = d.dept_id;
```

**Key insight:** This is the "decomposed schema reassembled" query — normalization split the data; JOIN is how you reunite it. The ON condition is the FK→PK match.

---

**Setup:** Find students who never enrolled — three ways.

**Solution:**
```sql
-- 1. NOT IN — TRAP: fails if the subquery returns any NULL!
SELECT name FROM Student WHERE sid NOT IN (SELECT sid FROM Enrolled);

-- 2. NOT EXISTS — safe
SELECT s.name FROM Student s
WHERE NOT EXISTS (SELECT 1 FROM Enrolled e WHERE e.sid = s.sid);

-- 3. LEFT JOIN IS NULL
SELECT s.name FROM Student s
LEFT JOIN Enrolled e ON s.sid = e.sid
WHERE e.sid IS NULL;
```

**Key insight:** `NOT IN` with NULLs matches nothing — the classic silent bug. `NOT EXISTS` and LEFT JOIN are the robust spellings. This is why "anti-join" is a known pattern: "rows in A without a match in B."

---

**Setup:** Running total — cumulative sum per student ordered by date.

**Solution:**
```sql
SELECT date, amount,
       SUM(amount) OVER (ORDER BY date) AS running_total
FROM Transactions
WHERE sid = 42;
```

**Key insight:** Window functions (`OVER`) compute across a *window* of rows *without collapsing* them — unlike GROUP BY. `ROW_NUMBER()`, `RANK()`, `LAG()`, `SUM(...) OVER (...)` are the analytics workhorses: rankings, running totals, moving averages, previous-row comparisons.

---

## Practice (try before peeking)

1. `WHERE major = 'CS' OR 'Math'` — why is this wrong?
2. Can `WHERE` use `COUNT(*)`? Can `HAVING`?
3. `LIMIT` with no `ORDER BY` — what does it return?

<details><summary>Answers</summary>

1. `'Math'` is a truthy string constant, not a predicate — the condition is always true for every row. Must be `major = 'CS' OR major = 'Math'` (or `IN`).
2. WHERE can't (rows filtered before grouping); HAVING can (groups exist by then).
3. An *arbitrary* subset — no order is defined without ORDER BY. The rows chosen are engine-dependent.

</details>

---

**Common traps:**
- `= NULL` instead of `IS NULL` — matches nothing, silently
- NOT IN with NULLs in the subquery — empty result
- Forgetting GROUP BY columns must be in SELECT or aggregated
- Column alias used in WHERE — doesn't exist yet
- ORDER BY on a non-selected column (allowed in most DBs, but confusing)
- `SELECT *` in production — fragile, wasteful; name columns explicitly

---
