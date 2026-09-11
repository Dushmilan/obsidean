# Modular Arithmetic

Modular arithmetic is clock arithmetic: after 12 comes 1, and every number that differs by a multiple of 12 "is the same time." Formally it is the first place we build a brand-new number system from an old one — and $\mathbb{Z}_n$ will be the star worked example of group theory in 04-Groups-Definition-and-Examples.

**The Intuition:** Wrap the number line around a circle with $n$ marks. Adding or multiplying now means walking around the circle and landing somewhere; where you land depends only on your *starting mark*, not on how many full loops you made. This is why we can reduce digits of a huge number before doing anything else. The circle has exactly $n$ positions — the congruence classes — and arithmetic on classes is what defines $\mathbb{Z}_n$.

**The Math:** For $n \ge 1$, define $a \equiv b \pmod n$ iff $n \mid (a - b)$.

- This is an equivalence relation (01-Sets-Relations-Functions), with $n$ classes $[0], [1], \dots, [n-1]$. The set of classes is $\mathbb{Z}_n$.
- **Well-defined arithmetic:** if $a \equiv a'$ and $b \equiv b'$, then
$$a + b \equiv a' + b' \pmod n, \qquad ab \equiv a'b' \pmod n.$$
So $[a] + [b] := [a+b]$ and $[a] \cdot [b] := [ab]$ make sense regardless of which representative you pick.
- **Units:** $[a]$ has a multiplicative inverse in $\mathbb{Z}_n$ iff $\gcd(a, n) = 1$. The inverse is found via Bézout: if $ax + ny = 1$ then $[a]^{-1} = [x]$.
- **Linear congruences:** $ax \equiv b \pmod n$ has solutions iff $d = \gcd(a,n)$ divides $b$; if so there are exactly $d$ solutions mod $n$.
- **Fermat's Little Theorem:** if $p$ prime and $p \nmid a$, then $a^{p-1} \equiv 1 \pmod p$, hence $a^p \equiv a \pmod p$ for all $a$.

**Setup:** Find the inverse of $7$ in $\mathbb{Z}_{26}$.

**Solution:** Run Bézout: $26 = 3\cdot 7 + 5$, $7 = 1 \cdot 5 + 2$, $5 = 2 \cdot 2 + 1$. Back-substitute: $1 = 5 - 2(2) = 5 - 2(7 - 5) = 3 \cdot 5 - 2 \cdot 7 = 3(26 - 3\cdot7) - 2\cdot7 = 3 \cdot 26 - 11 \cdot 7$. So $-11 \cdot 7 \equiv 1 \pmod{26}$, giving $7^{-1} \equiv -11 \equiv 15 \pmod{26}$. Check: $7 \cdot 15 = 105 = 4 \cdot 26 + 1$. ✓

**Key insight:** Inverse hunting is just the Euclidean algorithm run backwards; the coefficient of $a$ in Bézout's identity is the inverse.

**Setup:** Solve $3x \equiv 4 \pmod 7$.

**Solution:** $\gcd(3,7)=1$, so a unique solution exists. Multiply both sides by $3^{-1} \bmod 7$: since $3 \cdot 5 = 15 \equiv 1$, the inverse is $5$. Then $x \equiv 5 \cdot 4 = 20 \equiv 6 \pmod 7$. Check: $3 \cdot 6 = 18 \equiv 4$. ✓

**Key insight:** When $\gcd(a,n)=1$, solving a linear congruence is just "multiply by the inverse" — mirroring how you'd solve $3x=4$ over the rationals.

**Setup:** Compute $7^{100} \bmod 5$.

**Solution:** By Fermat's Little Theorem, $7^4 \equiv 1 \pmod 5$ (since $5 \nmid 7$). Write $100 = 4 \cdot 25$, so $7^{100} \equiv (7^4)^{25} \equiv 1^{25} = 1 \pmod 5$.

**Key insight:** Reduce the *exponent* modulo $p - 1$ first; giant powers collapse instantly.

---

### Additional Notes

Two classic sanity checks live here:

- **Casting out nines:** $n \equiv (\text{sum of its digits}) \pmod 9$, because $10 \equiv 1 \pmod 9$. It is the same trick as computing $7^{100} \bmod 5$: replace things by equal-mod representatives before working.
- The units of $\mathbb{Z}_n$ form a set closed under multiplication — foreshadowing that "invertible elements of a ring" always form a group once groups are defined (05-Subgroups-and-Cyclic-Groups).

---
