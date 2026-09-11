# CI Pipeline & Automation

**Continuous Integration** is the practice of merging code frequently and verifying every merge with an automated pipeline: build → test → static analysis → artifact. The goal is *fast feedback* — a broken change is caught in minutes, not weeks, and "integration hell" never gets a chance to build.

**The Intuition:** CI is the assembly line of software: every push to the repository rolls a fresh build, runs the tests, and reports green or red. It's the difference between "it worked on my machine" and "it's proven to work on a clean machine, every time." The pipeline is a *quality gate* — nothing merges unless it passes.

## The CI pipeline stages

```text
Push → Checkout → Install deps → Build → Unit tests → Static analysis →
       Integration tests → Test coverage → Security scan → Artifact

Each stage fails fast — the earliest failing stage stops the run.
```

| Stage | Catches | Failure mode |
|-------|---------|--------------|
| Build | compile errors, missing deps | instant |
| Unit tests | logic regressions | seconds-minutes |
| Static analysis | style, bugs, security smells | minutes |
| Integration tests | wiring, contract breaks | minutes |
| Coverage gate | untested code | configurable |

## The feedback loops

```text
Local:   pre-commit hooks (lint, quick tests)      — seconds
CI:      on every push (build + test)              — minutes
Nightly: full suite + performance tests            — hours
```

**The golden rule: the pipeline should catch what you'd find by testing locally — but on a clean machine, automatically.**

## Key concepts

- **Build reproducibility:** pinned versions, lockfiles, container images — the build is deterministic
- **Clean environment:** CI runs from scratch (no "works on my laptop" state)
- **Parallelism:** shard tests across machines — a 45-minute suite becomes 5
- **Caching:** dependencies cached between runs (but never the test results!)
- **Fail fast:** the first failing stage halts — don't burn 30 min to learn the build broke

## Example — GitHub Actions

```yaml
name: CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with: { python-version: '3.12' }
      - run: pip install -r requirements-dev.txt
      - run: pytest --cov=src
      - run: ruff check src
```

## Quality gates

```text
A gate is a CHECK that blocks merge:
  - build passes
  - tests pass
  - coverage ≥ threshold (80%)
  - no lint errors
  - no known vulnerabilities (dependency scan)
  - no merge conflicts
  - required review approvals

Gates are the contract: "nothing lands without this being true."
```

## The branches CI guards

```text
Every PR: build + test (the main gate)
main:      after merge — deploy-prepare, artifact
release:   tagged — the deploy trigger
```

---

**Setup:** Tests take 45 minutes; developers wait, or skip CI. Fix.

**Solution:** Split the suite by speed:
```text
FAST gate (every push):  unit tests on changed modules (~2 min)
SLOW gate (merge/nightly): full suite, sharded across machines
```
Or: run the fast tests first, shard the slow ones in parallel, cache dependencies. The rule: *the 5-minute gate catches 90% of bugs; the full suite protects the long tail.*

**Key insight:** Pipeline latency is a feature. If CI is slow, devs bypass it (merge without waiting, or batch pushes) — the gates stop working. Optimize feedback time for the most common failures.

---

**Setup:** A test passes locally, fails only in CI. Debug.

**Solution:** The difference is *environment*:
1. Locally you have state CI doesn't (installed tools, a running DB, files)
2. Version differences (lockfile not committed, or Python version drift)
3. Test-order dependence (CI runs everything; local runs a subset)
4. Timing (sleeps, flaky async)

Fix by *reproducing*: run the full suite on a clean machine (`docker run` a CI image locally). Then pin versions, isolate tests, remove timing dependence.

**Key insight:** "Works locally" is an environment claim, not a correctness claim. CI's value is exactly that it runs *clean* — the gap it exposes is real. Reproduce it, fix it, never ignore the flake.

---

**Setup:** Should CI deploy to production automatically?

**Solution:** Only with discipline. CI *delivers* (build + test + artifact, always ready); *deploying* is a separate decision. For many teams: auto-deploy to staging, and to production via approval (or canary + monitoring). The pipeline can do it, but the *risk* of the release is yours.

**Key insight:** CI/CD pipeline separates "proven ready to ship" from "actually ship." The gate protects the merge; deployment strategy (canary, blue-green, feature flags) protects the release — see the CD & Deployment note.

---

**Setup:** What belongs in CI vs pre-commit hooks?

**Solution:**
```text
PRE-COMMIT (local, seconds):  formatting, lint, type-check, quick sanity tests
CI (clean, minutes):          full build, full test suite, coverage, security scans
```
Hooks catch the trivial instantly; CI proves the whole. Hooks should never be the only gate — they're trivially skippable (`--no-verify`) and run on the developer's dirty machine.

**Key insight:** Hooks = ergonomics (fast feedback); CI = truth (clean machine, complete). Both, in that order.

---

## Practice (try before peeking)

1. Why must CI run on a clean environment?
2. "Fail fast" means what in a pipeline?
3. A coverage gate of 100% — good or bad?

<details><summary>Answers</summary>

1. Reproducibility — local state (installed packages, config, timestamps) hides integration problems; a clean build is the honest test.
2. Run the cheapest, most likely-to-fail stages first — don't spend 30 minutes before learning the compile failed.
3. Usually bad — it incentivizes shallow tests over meaningful ones and adds noise. Choose a sensible threshold (80-90%) tied to *important* code.

</details>

---

**Common traps:**
- Slow pipelines → devs skip them → gates are fiction
- Non-reproducible builds (unpinned deps, "works on my machine")
- Flaky tests ignored — every flake is a real bug in hiding
- Coverage as the goal — coverage gates without assertion quality are theater
- Deploying from a developer's laptop — deploy only what CI built

---
