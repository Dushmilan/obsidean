# Proof Techniques

A proof is a convincing argument from premises to conclusion using accepted rules. The choice of technique matters: some statements yield to direct proof, some to contradiction, and some *require* construction. Knowing which tool fits which statement is the skill.

**The Intuition:** Proving is like debugging, but for math: you have a claim, and you must convince a skeptical reader (or a theorem prover) that it *always* holds — not just for the cases you checked. Each technique is a different strategy: walk forward (direct), walk backward (contrapositive), or assume you're wrong and derive nonsense (contradiction).

## The toolbox

### 1. Direct proof — assume P, deduce Q
```text
To prove P → Q:
  Assume P is true.
  Chain of valid steps: P ⟹ ... ⟹ Q.
  Conclude P → Q.
```

**Setup:** Prove: if $n$ is even, then $n^2$ is even.
**Solution:** $n = 2k$. Then $n^2 = 4k^2 = 2(2k^2)$, which is even. ∎

### 2. Proof by contrapositive — prove ¬Q → ¬P instead
Use when $\lnot Q$ is easier to work with than $P$.

**Setup:** Prove: if $n^2$ is even, then $n$ is even.
**Solution:** Contrapositive: if $n$ is odd, $n = 2k+1$, then $n^2 = 4k^2 + 4k + 1 = 2(2k^2+2k) + 1$, odd. So $n^2$ odd → $n$ odd, contrapositive of the claim. ∎

### 3. Proof by contradiction — assume the claim is false, derive absurdity
```text
To prove S:
  Assume ¬S.
  Derive a contradiction (e.g., 1 = 0, or p ∧ ¬p).
  Conclude S must hold.
```

**Setup:** Prove $\sqrt{2}$ is irrational.
**Solution:** Assume $\sqrt{2} = a/b$ in lowest terms. Square: $2 = a^2/b^2$, so $a^2 = 2b^2$ → $a$ even → $a = 2k$ → $4k^2 = 2b^2$ → $b^2 = 2k^2$ → $b$ even. Both even contradicts "lowest terms." So $\sqrt{2}$ irrational. ∎

### 4. Proof by construction — exhibit an object
Use for existence claims $\exists x P(x)$: just *produce* one.

**Setup:** Prove there exist irrational numbers $a, b$ with $a^b$ rational.
**Solution:** If $\sqrt{2}^{\sqrt{2}}$ is rational, done ($a=b=\sqrt2$). If not, let $a = \sqrt2^{\sqrt2}$, $b = \sqrt2$; then $a^b = (\sqrt2^{\sqrt2})^{\sqrt2} = \sqrt2^2 = 2$, rational. Either way, such a pair exists. ∎

### 5. Proof by cases — exhaust the possibilities
When the claim splits naturally, cover every case.

**Setup:** Prove $n^2 \ge n$ for all integers $n$.
**Solution:** Case 1: $n \le 0$ → $n^2 \ge 0 \ge n$. Case 2: $n \ge 1$ → $n^2 \ge n$ (multiply $n \ge 1$ by $n > 0$). Cases cover all integers. ∎

### 6. Proof by counterexample — one case kills a ∀ claim
**Setup:** Disprove: "all primes are odd."
**Solution:** $2$ is prime and even. ∎

## Common proof idioms

**"WLOG" (without loss of generality):** when a symmetric assumption is harmless.
```text
Prove the sum of two evens is even: "WLOG let the two numbers be 2a and 2b."
(Ordering them as "first" and "second" adds nothing — the argument is symmetric.)
```

**Uniqueness proofs — two-part structure:**
```text
To prove "there is EXACTLY ONE x with P(x)":
  1. Existence: exhibit such an x.
  2. Uniqueness: suppose x and y both work; show x = y.
```

**Setup:** Prove the equation $2x + 3 = 7$ has a unique solution.
**Solution:** Existence: $x = 2$ works. Uniqueness: if $2x+3 = 7$ and $2y+3 = 7$, then $2x = 4 = 2y$, so $x = y$. ∎

## Recognizing the technique

| Statement shape | Go-to technique |
|----------------|-----------------|
| $P \to Q$, straightforward | Direct |
| $P \to Q$, $\lnot Q$ is simpler | Contrapositive |
| "No such thing exists" / "impossible" | Contradiction |
| "There exists ..." | Construction |
| "Every ..." | Arbitrary element / general argument |
| Case-sensitive | Cases |
| "Exactly one" | Existence + uniqueness |

---

**Setup:** Prove that if $x$ and $y$ are rational, then $xy$ is rational.

**Solution:** $x = a/b$, $y = c/d$ with $a,b,c,d$ integers, $b,d \ne 0$. Then $xy = ac/bd$, a ratio of integers with $bd \ne 0$. Rational. ∎

**Key insight:** Direct proof with the *definition*: unwrap the definition of "rational", do algebra, rewrap. Most direct proofs are "unpack definitions → manipulate → repack."

---

**Setup:** Prove that if $n^2$ is odd, then $n$ is odd.

**Solution:** Contrapositive — assume $n$ even: $n = 2k$ → $n^2 = 4k^2 = 2(2k^2)$, even. So $n$ even → $n^2$ even, which is the contrapositive of the claim. ∎

**Key insight:** Parity proofs (even/odd) are the canonical classroom for contrapositive — "odd" is harder to work with than "not odd = even."

---

**Setup:** Prove there are infinitely many primes (Euclid).

**Solution:** Suppose finitely many: $p_1, \dots, p_n$. Let $N = p_1 p_2 \cdots p_n + 1$. $N$ has a prime factor $q$. $q$ can't be any of the $p_i$ (each divides $p_1\cdots p_n$ but not $+1$, so not $N$). Contradiction — a prime outside the list exists.

**Key insight:** The "construct something that evades every candidate" trick is the backbone of many contradiction proofs — it's also the same logic as Cantor's diagonalization (countability) and the halting problem proof in computability.

---

**Setup:** Prove that $\lnot(Q \to P) \to \lnot(P \to Q)$ is a tautology.

**Solution:** Assume $\lnot(Q \to P)$: since $Q \to P \equiv \lnot Q \lor P$, the negation is $Q \land \lnot P$ — so $Q$ true, $P$ false. Now $P \to Q$ has a false premise, so $P \to Q$ is *true* — meaning $\lnot(P \to Q)$ is false. The implication "if (negation1) then (negation2)" has a true premise giving a false conclusion — wait, recheck: premise true, conclusion false → whole statement FALSE?

Hmm — let me redo: the claim $\lnot(Q \to P) \to \lnot(P \to Q)$. Assume $\lnot(Q\to P)$ true → $Q \land \lnot P$ → $P$ false → $P \to Q$ is vacuously TRUE → $\lnot(P \to Q)$ is FALSE. So premise true, conclusion false → the implication is FALSE. This is not a tautology! (Checking with a truth table confirms.) Good catch — the honest answer: it's a contingency, false when Q true and P false.

**Key insight:** This example shows why you *verify* rather than assume — a plausible-looking implication can fail. The vacuous-truth rule (false premise → implication true) is doing the work.

---

## Practice (try before peeking)

1. Prove: if $m$ and $n$ are odd, then $mn$ is odd.
2. Prove or disprove: "all perfect squares are even."
3. Prove: there is no integer $n$ with $n^2 = 2 \pmod 4$.

<details><summary>Answers</summary>

1. $m = 2a+1$, $n = 2b+1$ → $mn = 4ab + 2a + 2b + 1 = 2(2ab + a + b) + 1$, odd.
2. Disprove by counterexample: $9 = 3^2$ is odd. (∀ claim → one counterexample suffices.)
3. Contradiction: if $n^2 \equiv 2 \pmod 4$, then $n$ is even (squares of evens are $0 \bmod 4$, squares of odds are $1 \bmod 4$), say $n = 2k$ → $n^2 = 4k^2 \equiv 0 \pmod 4$, contradiction.

</details>

---

**Common traps:**
- Proving a universal claim with examples — examples can't prove "for all"
- Affirming the consequent — $P \to Q$ and $Q$ doesn't give $P$
- Confusing contrapositive with converse/inverse
- Contradiction proof that doesn't contradict — derive a *genuine* inconsistency
- Hand-waving "obviously" for the hard step — each step needs justification

---
