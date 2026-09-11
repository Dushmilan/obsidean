# Number Theory & Modular Arithmetic

Number theory studies the integers; modular arithmetic is their finite arithmetic. Together they power cryptography (RSA), hashing, checksums, random generation, and compiler optimizations. The stars: the Euclidean algorithm, modular inverses, and Fermat/Euler theorems.

**The Intuition:** Modular arithmetic is clock arithmetic: after 12 o'clock comes 1, after 23:59 comes 00:00. $a \bmod n$ is the remainder on the clock with $n$ hours. This finite world has beautiful structure — and because computers use fixed-width integers, *real machines literally compute mod $2^{32}$*. Understanding modular arithmetic is understanding what computers actually do.

## Divisibility

```text
a | b  ⟺  b = a·k for some integer k   ("a divides b")
Every integer a:  a | 0  (0 = a·0)
Prime: divisible only by 1 and itself
Composite: has a proper divisor
```

**Fundamental Theorem of Arithmetic:** every integer $n \ge 2$ factors uniquely into primes.

## GCD & the Euclidean algorithm

```text
gcd(a, b) = largest d with d|a and d|b.
gcd(a, 0) = a.

Euclidean algorithm:
  gcd(a, b) = gcd(b, a mod b)
  repeat until remainder 0; the last non-zero remainder is the gcd.

gcd(48, 18):
  48 = 2·18 + 12
  18 = 1·12 + 6
  12 = 2·6 + 0   → gcd = 6
```

**Bézout's identity:** there exist integers $x, y$ with $ax + by = \gcd(a,b)$. The **extended Euclidean algorithm** computes $x, y$ — the machinery behind modular inverses.

```text
Back-substitute for 48x + 18y = 6:
  6 = 18 - 12        (from step 2)
    = 18 - (48 - 2·18) = 3·18 - 48
  → x = -1, y = 3.  Check: -48 + 54 = 6 ✓
```

**Complexity:** $O(\log \min(a,b))$ — fast enough for thousand-digit numbers (the foundation of RSA).

## Modular arithmetic

```text
a ≡ b (mod n)  ⟺  n | (a - b)

The ring ℤ/nℤ: addition, subtraction, multiplication work normally.
  (a + b) mod n, (a·b) mod n — just reduce at the end.

Modular exponentiation — fast: repeated squaring, O(log e) multiplications.
  3^13 mod 7:
    3¹=3, 3²=2, 3⁴=4, 3⁸=2  (squaring)
    13 = 8+4+1 → 3^13 = 3⁸·3⁴·3¹ = 2·4·3 = 24 ≡ 3 (mod 7)
```

## Modular inverses — division mod n

```text
The inverse of a (mod n): a·a⁻¹ ≡ 1 (mod n).

EXISTS ⟺ gcd(a, n) = 1.
  If gcd(a,n) = 1, the extended Euclidean algorithm gives it directly:
  ax + ny = 1 → a·x ≡ 1 (mod n) → a⁻¹ ≡ x (mod n).

5⁻¹ mod 17:
  17 = 3·5 + 2; 5 = 2·2 + 1
  1 = 5 - 2·2 = 5 - 2·(17 - 3·5) = 7·5 - 2·17 → 5⁻¹ ≡ 7.
  Check: 5·7 = 35 ≡ 1 (mod 17) ✓
```

## Fermat & Euler — the shortcuts

```text
Fermat's little theorem:  p prime, p∤a  ⇒  a^(p-1) ≡ 1 (mod p)
  Consequence: a^p ≡ a (mod p) for all a.

Euler's theorem:  gcd(a, n) = 1  ⇒  a^(φ(n)) ≡ 1 (mod n)
  φ(n) = count of integers 1..n coprime to n (Euler's totient)
  φ(p) = p - 1 for prime p
  φ(pq) = (p-1)(q-1) for distinct primes  ← RSA's key fact
```

**Setup:** Compute $2^{1000000} \bmod 7$.
**Solution:** By Fermat, $2^6 \equiv 1$ (mod 7). $1000000 = 6 \cdot 166666 + 4$, so $2^{1000000} \equiv 2^4 = 16 \equiv 2$ (mod 7).

## Chinese Remainder Theorem (CRT)

```text
System:  x ≡ a₁ (mod n₁), x ≡ a₂ (mod n₂), ..., pairwise coprime nᵢ
Has a UNIQUE solution mod N = n₁n₂···nₖ.

x = Σ aᵢ · Nᵢ · (Nᵢ⁻¹ mod nᵢ)   where Nᵢ = N / nᵢ
```

**Setup:** $x \equiv 2 \pmod 3$, $x \equiv 3 \pmod 5$.
**Solution:** $N = 15$, $N_1 = 5$, $N_2 = 3$. $5^{-1} \bmod 3 = 2^{-1} = 2$; $3^{-1} \bmod 5 = 2$. $x = 2·5·2 + 3·3·2 = 20 + 18 = 38 \equiv 8 \pmod{15}$.

## Applications in CS

| Application | Modular idea |
|-------------|--------------|
| RSA encryption | exponentials mod $n = pq$; inverse mod $\phi(n)$ |
| Diffie-Hellman | $g^{ab} \bmod p$ from $g^a, g^b$ |
| Hash functions | $h(x) = (ax + b) \bmod p$ |
| Checksums / ISBN | weighted sums mod 10/11 |
| PRNG (LCG) | $x_{n+1} = (ax_n + c) \bmod m$ |
| Compiler strength reduction | $x \bmod 2^k = x \& (2^k - 1)$ |
| Fixed-width arithmetic | everything mod $2^{32}$ or $2^{64}$ |

---

**Setup:** Find $gcd(252, 198)$ and express it as a Bézout combination.

**Solution:**
```text
252 = 1·198 + 54
198 = 3·54 + 36
54 = 1·36 + 18
36 = 2·18 + 0   → gcd = 18
Back-substitute:
18 = 54 - 36 = 54 - (198 - 3·54) = 4·54 - 198
   = 4·(252 - 198) - 198 = 4·252 - 5·198
→ 4·252 - 5·198 = 18 ✓
```

**Key insight:** The algorithm gives the gcd *and* the coefficients simultaneously — one pass, and the inverse computation falls out. This is why RSA's key generation is efficient.

---

**Setup:** Solve $5x \equiv 3 \pmod{11}$.

**Solution:** First find $5^{-1} \bmod 11$: Euclid gives $9$ ($5·9 = 45 \equiv 1$). Then $x \equiv 3·9 = 27 \equiv 5 \pmod{11}$. Check: $5·5 = 25 \equiv 3$ ✓.

**Key insight:** "Divide" mod n = multiply by the inverse. Division is only defined when the inverse exists ($\gcd = 1$). This is why textbooks say "division mod n" needs care.

---

**Setup:** RSA key setup: $p = 61$, $q = 53$. Find $n$, $\phi(n)$, and a valid public/private exponent pair.

**Solution:** $n = 61·53 = 3233$. $\phi(n) = 60·52 = 3120$. Pick $e = 17$ (coprime to 3120). Find $d \equiv 17^{-1} \pmod{3120}$: extended Euclid gives $d = 2753$ ($17·2753 = 46801 = 15·3120 + 1$). Public key: $(e, n) = (17, 3233)$; private: $(d, n) = (2753, 3233)$.

**Key insight:** The security relies on: factoring $n$ back into $p, q$ is hard, but *generating* needs only multiplication and a modular inverse. The inverse via Euclid is the whole trick — and it's $O(\log n)$.

---

**Setup:** Compute $7^{222} \bmod 11$ using Fermat.

**Solution:** Fermat: $7^{10} \equiv 1 \pmod{11}$. $222 = 10·22 + 2$, so $7^{222} \equiv 7^2 = 49 \equiv 5 \pmod{11}$.

**Key insight:** Reduce the exponent mod $(p-1)$ — that's the entire shortcut. Any huge exponent becomes a tiny one.

---

## Practice (try before peeking)

1. Find $6^{-1} \bmod 13$.
2. Solve $x \equiv 1 \pmod{2}$, $x \equiv 2 \pmod{3}$.
3. Does $4^{-1} \bmod 8$ exist?

<details><summary>Answers</summary>

1. $11$ — $6·11 = 66 = 5·13 + 1$. (Euclid: $13 = 2·6+1$ → $1 = 13 - 2·6$ → inverse $-2 \equiv 11$.)
2. $x \equiv 5 \pmod 6$ (CRT: $N=6$; $5 \equiv 1 \bmod 2$, $5 \equiv 2 \bmod 3$).
3. No — $\gcd(4, 8) = 4 \ne 1$. An inverse exists iff gcd = 1.

</details>

---

**Common traps:**
- $a \bmod n = 0$ when $n \mid a$ — the remainder can be 0
- Negative remainders: define mod so the result is in $0..n-1$; different languages differ on negative division!
- Division mod n isn't always defined — only when $\gcd(a, n) = 1$
- $a^k \bmod n$: never compute $a^k$ first (overflow!) — use repeated squaring
- Fermat requires prime modulus and $p \nmid a$ — don't apply it to composites (use Euler)

---
