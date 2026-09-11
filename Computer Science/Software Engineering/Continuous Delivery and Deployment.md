# Continuous Delivery & Deployment

**Continuous Delivery** keeps every change *ready to ship* (artifact built, tested, staged); **Continuous Deployment** ships every passing change to production automatically. The deployment *strategies* — rolling, blue-green, canary, feature flags — are how you release software without the "deploy Friday" fear.

**The Intuition:** CD is about making the last mile routine. If shipping is a tense, manual, twice-a-year ritual, releases are big-bang and risky. If shipping is a small, reversible, every-day event, releases are boring — and boring is good. The strategies exist to make each release *low-risk and reversible*.

## Delivery vs Deployment

```text
CONTINUOUS DELIVERY:
  Every change that passes CI is automatically ready to deploy.
  The final push to production is a manual, one-click decision.
  → "Always releasable, release when you choose."

CONTINUOUS DEPLOYMENT:
  Every change that passes CI goes to production automatically.
  No human button. Safe ONLY with strong tests + metrics + rollback.
  → "Always released, release is the normal path."
```

## The deploy pipeline

```text
CI: build → test → artifact (container/image/jar)
CD: artifact → staging deploy → smoke tests → production
                                 (strategy chosen per risk)
```

## Deployment strategies

| Strategy | How | Risk | Rollback |
|----------|-----|------|----------|
| **Rolling** | Update instances one by one | low — mixed versions during deploy | redeploy old |
| **Blue-green** | Two envs; switch traffic atomically | very low — instant switch back | flip the switch |
| **Canary** | 5% of traffic to new version, watch metrics, ramp | low, metric-driven | stop the ramp |
| **Feature flags** | Ship code disabled; enable per-feature remotely | lowest | flip the flag |

### Blue-green
```text
BLUE (old, live)  ── traffic ──► users
GREEN (new, deployed + smoke-tested)
   → switch the router: traffic now goes to GREEN
   → keep BLUE ready; rollback = switch back (seconds)
```

### Canary
```text
v2 deployed to 5% of servers → watch error rate & latency
   → 25% → 100% (or roll back if metrics degrade)
   Requires: monitoring, metrics, a decision rule in advance
```

### Feature flags
```java
if (featureFlags.isEnabled("new_checkout")) {
    newCheckout();      // new code, shipped but hidden
} else {
    oldCheckout();
}
// Deploy and release are now DECOUPLED:
// deploy the code today, enable the flag when you're ready,
// disable instantly if it misbehaves — no redeploy needed.
```

## Release vs deployment — the decoupling

```text
DEPLOYMENT: new code is running on the servers
RELEASE:    users can actually use the new feature

Feature flags decouple them:
  - Deploy on Tuesday (code lands, flag off)
  - Release on Thursday (flag on, watched)
  - Roll back in seconds (flag off — no redeploy)

This is why modern teams ship daily:
  the risky moment (exposing users) is a flag flip, not a deploy.
```

## Monitoring — the safety net

```text
Without metrics, a canary is a guess. Minimum set:
  - Error rate (5xx, exceptions)
  - Latency (p50/p95/p99)
  - Traffic volume
  - Business metric (signups, orders, conversions)

The decision rule: "if error rate > X for 5 minutes, roll back."
Write it BEFORE the deploy, not during.
```

## The deploy checklist

```text
1. CI is green (build + tests passed)
2. Artifact is immutable and versioned (build once, deploy the same artifact)
3. Database migrations are backward-compatible (deploy old code against new schema works)
4. Rollback path is known and tested
5. Monitoring is watching the right metrics
6. The release decision rule is written down
```

## Database migrations — the hidden deploy risk

```sql
-- BAD: drop-first migration breaks the old code still running during rollout
ALTER TABLE users DROP COLUMN legacy_field;

-- GOOD: additive, backward-compatible (old + new code both work):
ALTER TABLE users ADD COLUMN new_field TEXT NULL;
-- deploy new code → backfill → later (cleanup release) drop old column

-- Expand-contract (expand, migrate, contract):
--  1. expand: add nullable column
--  2. migrate: backfill + write new code
--  3. contract: drop the old column in a LATER release
```

---

**Setup:** A team wants to deploy daily without Friday-night firefighting.

**Solution:** Small, reversible releases:
```text
1. Feature flags on every risky change (release decoupled from deploy)
2. Canary + monitoring with a written rollback rule
3. Blue-green for infra-level changes (schema-free swaps)
4. Backward-compatible migrations (expand-contract)
5. "Deploy early in the week, small, often"
```
The fear comes from big-bang releases; the cure is making every release boring.

**Key insight:** Risk ∝ release size and novelty. Small + frequent + reversible = boring releases. Boring is the goal — it means the system is healthy enough that shipping is routine.

---

**Setup:** Canary reveals a 5% error-rate spike. What now?

**Solution:** Stop the ramp immediately — the canary is only 5%, so blast radius is limited. Roll back (redeploy the previous version or flip the flag), then investigate from the logs. The *point* of canary is catching this early, with few users affected.

**Key insight:** A canary only works if you (1) watch metrics during the ramp and (2) have a *pre-decided* rollback threshold. "Watch it and see" without a rule invites exactly the slow-motion outage canaries exist to prevent.

---

**Setup:** Blue-green for a database schema change — what's the gotcha?

**Solution:** Blue-green handles *application* switches atomically, but if the schema changed, the old (blue) version may not work against the new schema during rollback. Fix: keep migrations backward-compatible (expand-contract) so *both* versions work against *both* schemas during the transition window.

**Key insight:** The app is easy to version; the database is state that can't be "switched back" like traffic. Compatibility between old code and new schema is the discipline that makes blue-green (and rollback in general) actually safe.

---

**Setup:** Should a startup adopt continuous *deployment*?

**Solution:** With discipline — but start with continuous *delivery*: every change is releasable, and the release is a conscious click. Add full auto-deploy when the pipeline, monitoring, and rollback are boring enough. Auto-deploying a system you can't observe is just automating risk.

**Key insight:** The pipeline's maturity ladder: manual release → CI → CD (delivery) → continuous deployment → canary-automated. Move up each rung only when the rung below is proven. The *goal* is fast, safe releases — not necessarily full automation.

---

## Practice (try before peeking)

1. Difference between delivery and deployment?
2. Fastest rollback: canary, blue-green, or feature flags?
3. Why must migrations be backward-compatible?

<details><summary>Answers</summary>

1. Delivery = always *ready* to deploy (human releases); Deployment = releases automatically on every passing change.
2. Feature flags — flip off, no redeploy (seconds). Blue-green is the fastest *deploy-level* rollback; canary needs a redeploy.
3. During a rolling deploy, old and new code run simultaneously against the same schema — a drop-first migration breaks the still-running old version.

</details>

---

**Common traps:**
- "Deploy Friday" + big-bang releases — the recipe for weekend outages
- Canaries without metrics or a pre-written rule — you won't know it's failing until it's all failing
- Migrations that aren't backward-compatible — rollback becomes impossible
- Deploying from a developer machine — deploy exactly what CI built
- Feature flags that never get removed — flag debt accumulates; schedule removal

---
