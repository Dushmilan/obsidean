# Joins & Aggregation

Joins are how the relational model answers questions spanning multiple tables; aggregation is how it summarizes groups. Together they're most real analytical SQL. The deep skill is choosing the right join (inner/outer/cross/self) and knowing when to aggregate versus when to window.

**The Intuition:** A join is "combine these tables based on a relationship" — like looking up a student's name by their id in another table. An aggregation is "collapse a group of rows into one summary" — like turning 1000 order rows into "average order value." The art is *grouping correctly* (which rows belong together?) and *join-correctly* (which rows match?).

## The join family — full detail

```sql
-- INNER JOIN: only matching rows
SELECT s.name, e.cid
FROM Student s
INNER JOIN Enrolled e ON s.sid = e.sid;
-- Students with no enrollments: ABSENT from the result

-- LEFT OUTER JOIN: all left rows, matched or not
SELECT s.name, e.cid
FROM Student s
LEFT JOIN Enrolled e ON s.sid = e.sid;
-- Students with no enrollments: present, e.cid = NULL

-- RIGHT / FULL OUTER: mirror / both sides preserved
-- CROSS JOIN: every pair (careful — n×m rows)
SELECT * FROM Colors CROSS JOIN Sizes;   -- 4 colors × 3 sizes = 12

-- SELF JOIN: table joined to itself (aliases required)
SELECT e.name, m.name AS manager
FROM Employee e
JOIN Employee m ON e.manager_id = m.emp_id;
```

## Join + aggregation — the classic combination

```sql
-- Number of enrollments per student, ALL students:
SELECT s.name, COUNT(e.sid) AS enrollments
FROM Student s
LEFT JOIN Enrolled e ON s.sid = e.sid
GROUP BY s.name;
```

**The trap:** with an INNER JOIN, students with zero enrollments disappear *before* grouping — so they'd be missing from the report. LEFT JOIN keeps them, and `COUNT(e.sid)` counts only non-NULL enrollments (0 for the NULL-filled).

## COUNT gotchas

```sql
COUNT(*)            -- counts ROWS (including all-NULL rows)
COUNT(col)          -- counts NON-NULL values of col
COUNT(DISTINCT col) -- counts distinct non-NULL values

-- In a LEFT JOIN:
COUNT(e.sid)  -- counts enrollments; unmatched left rows contribute 0
COUNT(*)      -- counts left rows TOO — unmatched rows count 1!
```

## Filtering before vs after grouping

```sql
-- WHERE: filter rows BEFORE grouping (cheaper, and different meaning)
SELECT dept, COUNT(*)
FROM Employee
WHERE salary > 50000          -- only high earners grouped
GROUP BY dept;

-- HAVING: filter groups AFTER grouping
SELECT dept, COUNT(*)
FROM Employee
GROUP BY dept
HAVING COUNT(*) > 10;         -- groups with > 10 members
```

**Both can appear:**
```sql
SELECT dept, AVG(salary)
FROM Employee
WHERE salary > 0                     -- row filter first
GROUP BY dept
HAVING COUNT(*) >= 5;                -- then group filter
```

## Aggregation functions

```sql
COUNT, SUM, AVG, MIN, MAX
-- AVG ignores NULLs; SUM ignores NULLs
-- MIN/MAX on strings = lexicographic
-- No MEDIAN in standard SQL (PostgreSQL: percentile_cont)
```

## Window functions — aggregation without collapsing

```sql
-- GROUP BY collapses rows; window functions DON'T:
SELECT name, dept, salary,
       AVG(salary) OVER (PARTITION BY dept) AS dept_avg,   -- per dept
       RANK() OVER (ORDER BY salary DESC) AS rank          -- overall rank
FROM Employee;

-- Running total:
SELECT date, amount,
       SUM(amount) OVER (ORDER BY date) AS running_total
FROM Transactions;

-- Moving average (last 3 rows):
AVG(amount) OVER (ORDER BY date ROWS BETWEEN 2 PRECEDING AND CURRENT ROW)
```

**Window grammar:** `OVER (PARTITION BY <group> ORDER BY <order> [ROWS ...])`. PARTITION splits into groups (like GROUP BY, but no collapsing); ORDER BY sequences within the partition; the frame (`ROWS BETWEEN...`) defines the moving window.

## Grouping sets / ROLLUP / CUBE

```sql
-- Subtotals and grand totals in one query:
SELECT dept, role, COUNT(*)
FROM Employee
GROUP BY ROLLUP (dept, role);
-- Produces: per (dept,role), per dept, and overall total rows.

-- GROUPING SETS: explicit combinations
GROUP BY GROUPING SETS ((dept, role), (dept), ());
```

---

**Setup:** Show each course with its enrollment count and average grade — every course, including empty ones.

**Solution:**
```sql
SELECT c.cid, COUNT(e.sid) AS n, AVG(e.grade) AS avg_grade
FROM Course c
LEFT JOIN Enrolled e ON c.cid = e.cid
GROUP BY c.cid;
```

**Key insight:** LEFT JOIN preserves courses with no students; `COUNT(e.sid)` gives 0 and `AVG(e.grade)` gives NULL for them. Without LEFT, empty courses vanish. This "all X, even those without Y" is THE outer-join use case.

---

**Setup:** Find the highest-paid employee per department.

**Solution:**
```sql
SELECT name, dept, salary
FROM (
    SELECT name, dept, salary,
           RANK() OVER (PARTITION BY dept ORDER BY salary DESC) AS rnk
    FROM Employee
) ranked
WHERE rnk = 1;
```

**Key insight:** Window + outer filter — the subquery ranks within each department; the WHERE keeps the top. `RANK()` gives ties the same rank (1,1,3...); `ROW_NUMBER()` breaks ties arbitrarily; `DENSE_RANK()` gives 1,1,2. Choose per the business rule.

---

**Setup:** Find departments where the average salary exceeds the company average.

**Solution:**
```sql
SELECT dept, AVG(salary) AS avg_sal
FROM Employee
GROUP BY dept
HAVING AVG(salary) > (SELECT AVG(salary) FROM Employee);
```

**Key insight:** The scalar subquery computes the company average once; HAVING compares each group. Comparing *group aggregates* to *table aggregates* is the classic HAVING-with-subquery pattern.

---

**Setup:** Compute the difference between each employee's salary and their department's average.

**Solution:**
```sql
SELECT name, dept, salary,
       salary - AVG(salary) OVER (PARTITION BY dept) AS vs_dept_avg
FROM Employee;
```

**Key insight:** Window functions let you compare a row against its *group's* aggregate *in the same row* — impossible with plain GROUP BY (which collapses). This single capability replaces many self-join gymnastics.

---

## Practice (try before peeking)

1. INNER JOIN on two tables with no matches — how many result rows?
2. `COUNT(e.id)` with a LEFT JOIN — what's the count for an unmatched left row?
3. RANK vs DENSE_RANK with salaries 100, 90, 90, 80?

<details><summary>Answers</summary>

1. Zero (or the cross-product-less empty result) — inner join drops all unmatched rows.
2. 0 — COUNT(col) counts non-NULLs; the NULL-filled unmatched row contributes nothing.
3. RANK: 1,2,2,4 (gaps). DENSE_RANK: 1,2,2,3 (no gaps). ROW_NUMBER: 1,2,3,4 (arbitrary tie-break).

</details>

---

**Common traps:**
- INNER JOIN silently dropping rows you wanted (use LEFT JOIN for "all X")
- `COUNT(*)` vs `COUNT(col)` in LEFT JOINs — off-by-enrollments
- Filtering in WHERE vs HAVING — wrong stage, wrong meaning
- Joining without an ON condition → accidental cross join (n×m rows)
- Forgetting DISTINCT after a join fans out rows (1 student × N enrollments)

---
