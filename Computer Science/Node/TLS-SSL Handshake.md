# TLS/SSL Handshake

TLS (and its deprecated predecessor SSL) is how plain HTTP becomes HTTPS — it proves the server is who it claims to be, then negotiates encrypted keys so everything after can be sent privately. No handshake = no identity check, no encryption.

**The Intuition:** Think of it like meeting a courier with a sealed briefcase. First you check their ID badge (certificate, signed by someone you trust). Then you agree on a secret combination for the briefcase (session keys) using a loud conversation eavesdroppers can't use — shouting paint colors to mix (Diffie-Hellman) or handing over a locked box only they can open (RSA key exchange). After that, you just use the fast combination lock (symmetric encryption) for everything.

## What it solves

1. **Authentication** — am I talking to `example.com`, not an imposter? (certificates + CA chain)
2. **Confidentiality** — can anyone reading the wire understand it? (symmetric session keys)
3. **Integrity** — was anything tampered with? (MAC / AEAD tags)

SSL 2.0/3.0 are dead and insecure. TLS 1.2 is widespread, TLS 1.3 (RFC 8446, 2018) is faster and removes legacy cruft. Always prefer 1.2+ with 1.3 enabled.

## Core ingredients

| Piece | Role |
|-------|------|
| Certificate (X.509) | Server's public key + identity (`CN`/`SAN`), signed by a CA. Client verifies chain to a trusted root. |
| CA chain | `leaf → intermediate(s) → root`. Server sends leaf + intermediates; client holds roots. |
| Cipher suite | The agreed recipe, e.g. `TLS_AES_128_GCM_SHA256` (TLS 1.3) or `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` (TLS 1.2). Covers key exchange + auth + bulk cipher + hash. |
| SNI | Client sends hostname in clear in `ClientHello` so one IP can serve many certs. |
| ALPN | Negotiates `h2` vs `http/1.1` inside the handshake. |

## TLS 1.2 handshake (full, ~2-RTT)

```text
Client                                              Server
  ---- ClientHello (version, random_C, cipher suites, SNI, ALPN) ---->
                     <---- ServerHello (chosen cipher, random_S) ----
                     <---- Certificate (leaf + chain) ---------------
                     <---- ServerKeyExchange (ECDHE params) ---------
                     <---- ServerHelloDone --------------------------
  ---- ClientKeyExchange (ECDHE public) + ChangeCipherSpec + Finished ->
                     <---- ChangeCipherSpec + Finished ---------------
  ==== encrypted application data (symmetric keys) ====
```

1. **Hello:** agree version + cipher + exchange randoms (`random_C`, `random_S` prevent replay).
2. **Auth:** server sends cert; client verifies chain, hostname (SAN), expiry, revocation (OCSP/CRL).
3. **Key exchange:** with ECDHE, both sides combine private + peer public to derive `pre-master secret` — eavesdropper sees only publics. RSA key transport (no forward secrecy) is legacy.
4. **Finished:** both sides send MAC of transcript under new keys — confirms no tampering, handshake done.

Cost: 2 round trips before first byte. Session resumption (session IDs/tickets) can shortcut this on reconnect.

## TLS 1.3 handshake (1-RTT, the modern default)

```text
Client                                              Server
  ---- ClientHello (random_C, key_share, suites, SNI, ALPN) --------->
                     <---- ServerHello (key_share) + EncryptedExtensions
                          + Certificate + CertificateVerify + Finished --
  ---- Finished ----------------------------------------------------->
  ==== application data ====
```

Key changes:
- Client **guesses** and sends a `key_share` upfront — server picks it, so keys are ready after 1 RTT.
- Everything after `ServerHello` is encrypted.
- Removed: RSA key transport, static DH, CBC-only suites, renegotiation, compression — smaller attack surface.
- `CertificateVerify` proves server owns the cert's private key (signs transcript).
- **0-RTT resumption** possible (replayable — only safe for idempotent GETs, enable carefully).
- **Forward secrecy is mandatory** — compromise of long-term key doesn't decrypt past sessions.

## In Node.js

```js
import https from 'node:https';
import fs from 'node:fs';

// Server — needs cert + private key (from Let's Encrypt, mkcert for local)
const server = https.createServer({
  cert: fs.readFileSync('cert.pem'),
  key: fs.readFileSync('key.pem'),
  minVersion: 'TLSv1.2', // allow 1.2 + 1.3
}, (req, res) => {
  res.end('secure hello');
});
server.listen(443);

// Client — Node verifies chain + hostname by default. Never disable in prod:
// rejectUnauthorized: false  // <-- MITM hole, only for local debugging
```

Check what negotiated: `req.socket.getCipher()` → `{ name: 'TLS_AES_256_GCM_SHA384', version: 'TLSv1.3' }`.

## Related: MX lookup & email delivery

MX lookup is DNS asking "where does mail for this domain go?" — it returns mail servers (with priority) that SMTP then connects to. Same TLS ideas apply, but over SMTP instead of HTTPS.

```js
import dns from 'node:dns/promises';

// MX lookup — where does mail for gmail.com go?
const mx = await dns.resolveMx('gmail.com');
// → [{ exchange: 'alt1.gmail-smtp-in.l.google.com', priority: 20 }, ...]
// Lowest priority number = tried first.
console.log(mx.sort((a, b) => a.priority - b.priority));

// Related lookups:
await dns.resolve4('alt1.gmail-smtp-in.l.google.com'); // A — host → IPv4
await dns.resolve6('alt1.gmail-smtp-in.l.google.com'); // AAAA — host → IPv6
await dns.resolveTxt('gmail.com'); // TXT — carries SPF, DKIM, DMARC policies
```

| Record | Role | Example |
|--------|------|---------|
| MX | Mail servers for a domain + priority | `gmail.com → alt1.gmail-smtp-in.l.google.com (prio 20)` |
| A / AAAA | Host → IPv4 / IPv6 (MX targets resolve here) | `mail.example.com → 142.250.x.x` |
| CNAME | Alias (never at MX target per RFC — must be A/AAAA) | `mail → server1.host.com` |
| TXT (SPF) | Which servers may send for this domain | `v=spf1 include:_spf.google.com ~all` |
| TXT (DKIM) | Public key to verify message signatures | `v=DKIM1; k=rsa; p=MIIB...` |
| TXT (DMARC) | What to do when SPF/DKIM fail | `v=DMARC1; p=quarantine; rua=mailto:...` |
| PTR | Reverse DNS — IP → hostname (spam filter check) | `x.x.250.142.in-addr.arpa → mail...` |

**How MX meets TLS:**
- **STARTTLS (ports 25 / 587):** SMTP starts plain, then upgrades with `STARTTLS` — same handshake as above, just triggered mid-protocol. Vulnerable to downgrade (attacker strips `STARTTLS`) unless paired with MTA-STS/DANE.
- **Implicit TLS (port 465):** TLS from the first byte, like HTTPS. No downgrade possible.
- **MTA-STS:** HTTPS-hosted policy saying "always use TLS for my MX hosts" — closes the STARTTLS-strip hole.
- **DANE (TLSA record):** Pins the MX host's cert in DNS (DNSSEC-signed), so the CA chain check is bound to DNS.
- SPF/DKIM/DMARC are **not encryption** — they are sender authentication (see [[Authentication and Security]]). You need both: TLS for privacy in transit, SPF/DKIM/DMARC to prove the sender.

---

**Setup:** A local HTTPS server that reports the negotiated cipher.

**Solution:**
```js
import https from 'node:https';
import fs from 'node:fs';

const server = https.createServer({
  cert: fs.readFileSync('cert.pem'),
  key: fs.readFileSync('key.pem'),
  minVersion: 'TLSv1.2',
}, (req, res) => {
  res.setHeader('Content-Type', 'application/json');
  res.end(JSON.stringify(req.socket.getCipher()));
});

server.listen(443);
```

**Key insight:** `req.socket.getCipher()` shows what the handshake in [[HTTP Servers and the Request Lifecycle]] negotiated — e.g. `{ name: 'TLS_AES_256_GCM_SHA384', version: 'TLSv1.3' }`. Same request lifecycle, just over a TLS socket from [[Node.js Fundamentals and the Event Loop]].

---

**Setup:** Client fails with `UNABLE_TO_VERIFY_LEAF_SIGNATURE` — why?

**Solution:** The server sent only the leaf cert and forgot the intermediate chain. The client holds roots, the server must send `leaf + intermediate(s)` so the chain builds to a trusted root. Fix by concatenating the intermediate into `cert.pem`. Verify hostname matches `SAN`, not just `CN`, and check expiry/OCSP.

**Key insight:** Most TLS errors are chain-building failures, not crypto failures. See [[Authentication and Security]] for why fail-closed is correct.

---

**Setup:** Why does stealing the server private key decrypt past RSA captures but not past ECDHE captures?

**Solution:** With RSA key transport the session key is encrypted to the long-term public key — steal the private key, decrypt everything ever captured. With ECDHE each session uses fresh ephemeral keys that are never transmitted; the long-term key only signs. Past sessions stay safe — this is forward secrecy, mandatory in TLS 1.3.

**Key insight:** Authentication (long-term key) and confidentiality (ephemeral keys) are separate jobs. Compromising identity must not compromise past privacy.

---

**Setup:** Find where mail for a domain goes and whether its servers support STARTTLS.

**Solution:**
```js
import dns from 'node:dns/promises';
import net from 'node:net';

const mx = await dns.resolveMx('example.com');
mx.sort((a, b) => a.priority - b.priority);
console.log(mx); // try lowest priority first

// STARTTLS probe — connect plain on 587, ask for extensions:
const sock = net.connect(587, mx[0].exchange);
sock.once('data', (banner) => {
  sock.write('EHLO probe\r\n'); // server replies with 250-STARTTLS if supported
  sock.once('data', (exts) => {
    console.log(exts.toString().includes('STARTTLS') ? 'STARTTLS yes' : 'STARTTLS no');
    sock.end();
  });
});
```

**Key insight:** MX gives you *who*; the STARTTLS probe tells you *whether TLS is offered*. No STARTTLS + no MTA-STS/DANE policy = mail can fall back to plaintext — the email version of the downgrade trap.

---

## Practice (try before peeking)

1. What are the 3 jobs of the handshake?
2. Why is ECDHE preferred over RSA key transport?
3. What does SNI solve, and what leaks about it?
4. MX says where mail goes — what upgrades that SMTP connection to TLS, and what stops a downgrade?

<details><summary>Answers</summary>

1. Authenticate the server (cert chain), negotiate session keys (confidentiality), confirm integrity (Finished MAC).
2. ECDHE gives forward secrecy — each session gets fresh ephemeral keys. With RSA transport, stealing the server private key decrypts all past captures.
3. SNI lets one IP/port serve many domains by sending the hostname in `ClientHello`. It leaks the hostname in clear (ECH — Encrypted Client Hello — fixes this, still rolling out).
4. `STARTTLS` on ports 25/587 (or implicit TLS on 465). Downgrade is stopped by MTA-STS policy or DANE TLSA — without them an attacker can strip `STARTTLS` and keep the connection plain.

</details>

---

**Common traps:**
- Expired / wrong-SAN cert, or forgetting to send the intermediate chain — clients fail closed
- Supporting TLS 1.0/1.1 or weak ciphers for "compat" — opens POODLE/BEAST/ROBOT-class attacks
- `rejectUnauthorized: false` or `process.env.NODE_TLS_REJECT_UNAUTHORIZED=0` left in production
- Terminating TLS at a load balancer then forwarding plain HTTP internally without securing that hop (see [[Proxies]])
- Confusing TLS (transport encryption) with auth (JWT/session) — you need both, see [[Authentication and Security]]
- Pointing MX at a CNAME instead of A/AAAA, or assuming SPF/DKIM/DMARC encrypt mail — they authenticate the sender, TLS encrypts the transit

---
Verify who you're talking to with certs, agree ephemeral keys in the open, then speak symmetric.

**Up:** [[Node_Index]]
