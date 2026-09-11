# Functional Dependencies

A **functional dependency** $X \to Y$ is the statement "rows that agree on $X$ also agree on $Y$" — the attribute set $X$ *determines* $Y$. Functional dependencies (FDs) are the raw material of schema design: they reveal redundancy, define keys, and drive normalization.

**The Intuition:** In a table of employees, `emp_id → name` says "one employee id, one name." That's a fact about the *domain*, not about any particular row. When two rows share an `emp_id`, they must also share the name — or the data is inconsistent. FDs are the "rules the world obeys" that your schema must respect; violating them creates redundancy and update anomalies.

## Definitions

```text
X → Y  means:  if two rows have the same X values, they MUST have the same Y values.

Examples (Employee table):
  emp_id → name, dept_id         — each employee has one name, one dept
  dept_id → dept_name            — each dept has one name
  emp_id → dept_name             — by transitivity (emp_id → dept_id → dept_name)

Not an FD:  dept_id → salary     — two employees in the same dept can differ in salary
```

## Trivial & non-trivial

```text
X → Y is TRIVIAL if Y ⊆ X.     e.g., {a, b} → {a}
X → Y is NON-TRIVIAL otherwise. e.g., {a} → {b}
```

Trivial FDs hold in every table by definition — they carry no information.

## Closure of a set of attributes — the engine

The **closure** $X^+$ = all attributes determined by $X$, found by iterating FDs:

```text
FDs:  AB → C,  C → D,  D → E
Compute {A,B}+:
  Start: {A,B}
  AB → C: add C → {A,B,C}
  C → D:  add D → {A,B,C,D}
  D → E:  add E → {A,B,C,D,E}
Closure = {A,B,C,D,E} — AB determines everything → AB is a KEY.
```

**Key test:** $X$ is a **superkey** iff $X^+ = \text{all attributes}$. It's a **candidate key** iff also minimal.

## Closure of a set of FDs — Armstrong's axioms

From given FDs, which others *must* hold? Armstrong's axioms generate them all:

```text
Reflexivity:     Y ⊆ X  ⇒  X → Y
Augmentation:    X → Y  ⇒  XZ → YZ
Transitivity:    X → Y, Y → Z  ⇒  X → Z
Derived:         X → Y, X → Z  ⇒  X → YZ     (union)
                X → YZ  ⇒  X → Y             (decomposition)
                X → Y, WY → Z  ⇒  WX → Z     (pseudo-transitivity)
```

## The closure algorithm

```text
To find the closure of FD set F (all FDs implied by F):
  For each FD's left side, compute its attribute closure; if it
  covers the right side, the FD is implied.
  
Example: F = {A→B, B→C}. Is A→C implied? {A}+ = {A,B,C} ∋ C → YES (transitivity).
         Is B→A implied?  {B}+ = {B,C} ∌ A → NO.
```

## Canonical cover — minimal equivalent FD set

A **canonical cover** $F_c$ is an equivalent FD set that is:
1. No FD with extraneous attributes (left or right)
2. Each left side unique (all FDs with the same left merged)

```text
F = {A→BC, B→C, A→B, AB→C}
Canonical cover: {A→B, B→C}
  - A→C is implied (transitive), AB→C redundant, A→BC merges.
```

The canonical cover is what you actually use for decomposition.

## Keys from FDs

```text
To find ALL candidate keys: try every subset, compute closure.
(Exponential in general — but with small schemas, pattern-spotting works.)

Attribute classes:
  - Never on any right side → must be in EVERY candidate key
  - Never on any left side  → in no candidate key (dependent only)
```

---

**Setup:** R(A, B, C, D) with FDs AB→C, C→D, D→A. Find all candidate keys.

**Solution:**
```text
Compute closures:
  {A,B}+: AB→C → {A,B,C}; C→D → {A,B,C,D} = ALL → AB is a key
  {B,C}+: C→D → {B,C,D}; D→A → {A,B,C,D} → BC is a key
  {B,D}+: D→A → {A,B,D}; AB→C → {A,B,C,D} → BD is a key
  {B}+: {B} — not a key
Candidate keys: AB, BC, BD.
```

**Key insight:** Finding keys = closure hunting. Note how *three* different minimal keys can exist — each is a valid identity for rows. Prime attributes: those in any key (here A, B, C, D all prime!).

---

**Setup:** Given R(A,B,C,D,E) with FDs AB→C, C→D, D→E, is AB→E implied?

**Solution:** Compute {A,B}+: AB→C → {A,B,C}; C→D → {A,B,C,D}; D→E → {A,B,C,D,E}. Contains E → YES, AB→E is implied.

**Key insight:** The attribute-closure algorithm decides implication mechanically — no cleverness needed. This is the same closure used to test keys and to reason about normalization.

---

**Setup:** Find the canonical cover of F = {A→BC, B→C, A→B, AB→C}.

**Solution:**
```text
1. Split right sides: {A→B, A→C, B→C, A→B, AB→C} → {A→B, A→C, B→C, AB→C}
2. Remove extraneous from AB→C: is A extraneous? {B}+ = {B,C} ∋ C → YES.
   So AB→C reduces to B→C (already have). Drop AB→C.
3. A→C is implied by A→B, B→C — remove.
Canonical cover: {A→B, B→C}.
```

**Key insight:** The canonical cover is the *minimal* set that implies the same FDs — it's what you decompose with. Checking extraneous attributes uses the same closure test.

---

**Setup:** Prove or refute: if A→B and A→C hold, then A→BC holds.

**Solution:** Holds — the *union rule* (derived from augmentation + decomposition): A→B ⇒ A→AB; A→C ⇒ AB→BC... more directly, from the definition: rows agreeing on A agree on B and agree on C, hence agree on (B,C).

**Key insight:** Armstrong's axioms (and their derived rules) let you rewrite FD sets safely — but *you must only add implied FDs*, never ones that merely "seem reasonable" for the data at hand.

---

## Practice (try before peeking)

1. Is `name → age` a sensible FD to *assume*? Why?
2. Compute {A}+ given A→B, B→C, C→A.
3. What makes an FD trivial?

<details><summary>Answers</summary>

1. No — you can't *assume* FDs from a few rows; two people can share a name with different ages. FDs are domain rules, verified against the data's meaning, not guessed from samples.
2. {A,B,C} — A determines everything (A→B→C→A closes the loop); A is a candidate key.
3. The right side is a subset of the left side (Y ⊆ X) — true by definition in every table.

</details>

---

**Common traps:**
- Guessing FDs from a few sample rows — FDs are claims about the *entire* domain
- Forgetting that an FD violation only shows with two *specific* rows — absence of counterexamples in small data proves nothing
- Confusing "FD holds in this table instance" with "FD is a domain rule"
- Computing closure of the *FD set* vs closure of an *attribute set* — different algorithms
- Assuming every table's given FDs are complete — hidden FDs (via transitivity) must be derived

---
